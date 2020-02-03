---
title: 从代码更新非 Store 发布的应用
description: 介绍如何在代码中由开发人员更新在商店外发货的 .MSIX 包。
author: Huios
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, 应用包, 应用更新, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 3dea6cf0f60846fd35190c642a1686134895d364
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726607"
---
# <a name="update-non-store-published-apps-from-your-code"></a>从代码更新非 Store 发布的应用

以 .MSIX 的形式交付应用时，可以通过编程方式启动应用程序的更新。 如果你将应用程序部署到应用商店外部，你只需在服务器上检查是否有新版本的应用程序并安装新版本。 可以通过利用[PackageManager. UpdatePackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.updatepackageasync)方法更新为新版本。 执行此操作需要应用声明 `packageManagement` 功能。

本文提供的示例演示如何在包清单中声明 `packageManagement` 功能，以及如何使用[UpdatePackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.updatepackageasync)方法来检查和安装更新。

## <a name="add-the-packagemanagement-capability-to-your-package-manifest"></a>将 PackageManagement 功能添加到包清单

若要使用 `PackageManager` Api，应用必须在[包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中声明 `packageManagement`[受限功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

## <a name="check-for-updates-on-your-server"></a>检查服务器上的更新

第一步是检查是否有新版本的应用程序可用。 下面的示例将检查服务器上包的版本是否大于当前版本的应用程序（此示例指的是用于演示的测试服务器）。

```csharp
using Windows.Management.Deployment;

//check for an update on my server
private async void CheckUpdate(object sender, TappedRoutedEventArgs e)
{
    WebClient client = new WebClient();
    Stream stream = client.OpenRead("https://trial3.azurewebsites.net/HRApp/Version.txt");
    StreamReader reader = new StreamReader(stream);
    var newVersion = new Version(await reader.ReadToEndAsync());
    Package package = Package.Current;
    PackageVersion packageVersion = package.Id.Version;
    var currentVersion = new Version(string.Format("{0}.{1}.{2}.{3}", packageVersion.Major, packageVersion.Minor, packageVersion.Build, packageVersion.Revision));

    //compare package versions
    if (newVersion.CompareTo(currentVersion) > 0)
    {
        var messageDialog = new MessageDialog("Found an update.");
        messageDialog.Commands.Add(new UICommand(
            "Update",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.Commands.Add(new UICommand(
            "Close",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.DefaultCommandIndex = 0;
        messageDialog.CancelCommandIndex = 1;
        await messageDialog.ShowAsync();
    } else
    {
        var messageDialog = new MessageDialog("Did not find an update.");
        await messageDialog.ShowAsync();
    }
}
```

## <a name="download-an-update"></a>下载更新

确定可用更新后，可以将其排队等待下载和安装。 下次关闭应用时，将应用此更新。 应用重新启动后，新版本将可供用户使用。

```csharp

// Queue up the update and close the current app instance.
private async void CommandInvokedHandler(IUICommand command)
{
    if (command.Label == "Update")
    {
        PackageManager packagemanager = new PackageManager();
        await packagemanager.UpdatePackageAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.msix"),
            null,
            DeploymentOptions.ForceApplicationShutdown
        );
    }
}
```
