---
title: 应用安装程序文件 API 问题
description: 本文列出了用于管理应用程序安装程序文件的 PackageManager 和包 Api 的已知问题。
ms.date: 2/20/2019
ms.topic: article
keywords: windows 10，uwp，应用安装程序，AppInstaller，旁加载，API
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ca759a80d443daeb58dd66913be2b2acd9e17eef
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328266"
---
# <a name="app-installer-file-api-issues"></a>应用安装程序文件 API 问题

## <a name="javascript-support-for-app-installer-file-apis"></a>应用安装程序文件 Api 的 JavaScript 支持

Windows SDK 中的[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)和[包](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package)类提供了可用于通过应用安装程序文件添加或修改包的方法，或用于检索应用程序安装程序关联的应用程序的相关信息。 有关详细信息，请参阅[相关文档](app-installer-documentation.md)。

在这些方法中，JavaScript 中不支持[PackageManager、AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)、 [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)和[CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync) 。 不过，你可以创建一个[Windows 运行时组件](https://docs.microsoft.com/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)，该组件调用这些方法，然后从 JavaScript UWP 应用调用此组件。

下面是一个示例。

```csharp
namespace CSRuntimeComponent
{
    public sealed class UpdateAvailabilityChecker
    {
        public static IAsyncOperation<PackageUpdateAvailability> CheckForUpdatesAsync()
        {
            return AsyncInfo.Run<PackageUpdateAvailability>((result) => Task.Run<PackageUpdateAvailability>(async () =>
            {
                PackageManager pm = new PackageManager();
                Package currentPackage = pm.FindPackageForUser(string.Empty, Package.Current.Id.FullName);
                PackageUpdateAvailabilityResult apiResult = await currentPackage.CheckUpdateAvailabilityAsync();

                if (apiResult.Availability == PackageUpdateAvailability.Error)
                {
                    Logger.Error($"Error occurred, extended code: {apiResult.ExtendedError}");
                }

                return apiResult.Availability;
            }));
        }
    }
}
```

```javascript
window.onload = function () {
    document.getElementById('mainButton').onclick = function (evt) {
        CSRuntimeComponent.UpdateAvailabilityChecker.checkForUpdatesAsync().done(function (result) {
            document.getElementById("resultLabel").innerHTML = "Update availability result:" + result;
        });
    }
}
```
