---
title: 从代码更新 Store 发布的应用
description: 介绍开发人员如何在代码中更新 .MSIX 包。
author: Huios
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, 应用包, 应用更新, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 1d387789c072782199b85d52c79b57c1c94e8312
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726627"
---
# <a name="update-store-published-apps-from-your-code"></a>从代码更新 Store 发布的应用

从 Windows 10 版本1607（版本14393）开始，Windows 10 允许开发人员更好地保证应用商店中的应用更新。 执行此操作需要几个简单的 Api，创建一致且可预测的用户体验，并使开发人员能够专注于他们最能完成的工作，同时允许 Windows 执行繁重的工作。

可以通过两种基本方式管理应用更新。 在这两种情况下，这些方法的最终结果相同-应用更新。 但是，在一种情况下，您可以选择让系统完成所有工作，而在另一种情况下，您可能希望更深入地控制用户体验。

## <a name="simple-updates"></a>简单更新

首先，最重要的是非常简单的 API 调用，告诉系统检查更新、下载更新，然后请求用户提供安装权限。 首先，使用[StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext)类获取[StorePackageUpdate](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate)对象，然后下载并安装它们。

```csharp
using Windows.Services.Store;

private async void GetEasyUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation = 
            updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);
        StorePackageUpdateResult result = await downloadOperation.AsTask();
    }
}
```

此时，用户有两个选项可供选择：立即应用更新或推迟更新。 用户所做的任何选择都将通过 `StorePackageUpdateResult` 对象返回到，使开发人员可以执行更多操作，例如关闭应用程序（如果需要继续更新或只是稍后重试）。

## <a name="fine-controlled-updates"></a>精细控制的更新

对于希望获得完全自定义体验的开发人员，提供了额外的 Api，可对更新过程进行更多控制。 平台使你能够执行以下操作：

* 获取单个包下载或整个更新上的进度事件。
* 在用户和应用的方便而不是其中的一个上应用更新。

开发人员可以在后台下载更新（当应用程序正在使用时），然后请求用户安装更新（如果它们拒绝），只需禁用受更新影响的功能。

### <a name="download-updates"></a>下载更新

```csharp
private async void DownloadUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
            updateManager.RequestDownloadStorePackageUpdatesAsync(updates);

        downloadOperation.Progress = async (asyncInfo, progress) =>
        {
            // Show progress UI
        };

        StorePackageUpdateResult result = await downloadOperation.AsTask();
        if (result.OverallState == StorePackageUpdateState.Completed)
        {
            // Update was downloaded, add logic to request install
        }
    }
}
```

### <a name="install-updates"></a>安装更新

```csharp
private async void InstallUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();    

    // Save app state here

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    StorePackageUpdateResult result = await installOperation.AsTask();

    // Under normal circumstances, app will terminate here

    // Handle error cases here using StorePackageUpdateResult from above
}
```

## <a name="making-updates-mandatory"></a>强制执行更新

在某些情况下，可能需要将更新安装到用户的设备上，使其真正是必需的（例如，对无法等待的应用程序的关键修补程序）。 在这些情况下，可以采取其他措施来强制更新。

1. 在应用程序代码中实现必需的更新逻辑（需要在强制更新本身之前完成）。
2. 在提交到开发人员中心的过程中，请确保选中 "**使此更新成为必需**的" 框。

### <a name="implementing-app-code"></a>实现应用代码

为了充分利用必需的更新，你需要对上面的代码进行一些细微的修改。 需要使用[StorePackageUpdate 对象](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate)来确定更新是否是必需的。

```csharp
 private async bool CheckForMandatoryUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        foreach (StorePackageUpdate u in updates)
        {
            if (u.Mandatory)
                return true;
        }
    }
    return false;
}
```

然后，需要创建一个自定义的应用程序对话框，以通知用户有必需的更新，并且必须安装它以继续充分利用应用。 如果用户拒绝更新，应用程序可能会降低功能（例如，阻止联机访问）或完全终止（例如仅联机游戏）。

### <a name="partner-center"></a>合作伙伴中心

若要确保[StorePackageUpdate](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate)对强制更新显示 true，则需要在 "**包**" 页面的 "合作伙伴中心" 中将更新标记为必需。

请注意以下几点：

* 如果在强制的更新与另一个非强制更新取代后设备恢复联机状态，则在该设备强制执行之前，非必需的更新仍会在设备上显示为必需的。
* 开发人员控制的更新和强制更新当前仅限于应用商店。
