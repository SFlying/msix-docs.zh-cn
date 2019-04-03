---
author: joshusto
title: 应用安装程序文件 API 问题
description: 列出与应用安装程序文件 Api 的已知问题。
ms.author: joshusto
ms.date: 2/20/2019
ms.topic: article
keywords: windows 10、 uwp、 应用程序安装程序中，应用安装旁, 加载 API
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: be13529ecbb1845447719c7951b77ca66287ffc0
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900419"
---
# <a name="app-installer-file-api-issues"></a>应用安装程序文件 API 问题

## <a name="javascript-support-for-app-installer-file-apis"></a>应用安装程序文件 Api 的 JavaScript 支持

[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)并[包](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package)Windows SDK 中的类提供的方法，可以使用添加或修改通过应用安装程序文件的包或检索有关与应用程序安装程序的应用的信息关联。 有关详细信息，请参阅[相关文档](app-installer-documentation.md)。

这些方法[PackageManager.AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)， [PackageManager.RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)，和[Package.CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)在 JavaScript 中不支持。 但是，可以创建[Windows 运行时组件](https://docs.microsoft.com/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)，调用这些方法，然后从 JavaScript UWP 应用中调用此组件。

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
