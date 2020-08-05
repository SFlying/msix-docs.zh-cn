---
title: 从代码更新非 Store 发布的应用
description: 介绍如何在代码中由开发人员更新在商店外发货的 .MSIX 包。
author: Huios
ms.date: 06/12/2020
ms.topic: article
keywords: windows 10, uwp, 应用包, 应用更新, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 1c120193a278fb8584761d7b6aaa4ab0430697ad
ms.sourcegitcommit: f1c366459764cf1f3c0bc9edcac4f845937794bd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87754506"
---
# <a name="update-non-store-published-app-packages-from-your-code"></a>从代码更新非存储已发布的应用包

以 .MSIX 的形式交付应用时，可以通过编程方式启动应用程序的更新。 如果你将应用程序部署到应用商店外部，你只需在服务器上检查是否有新版本的应用程序并安装新版本。 应用更新的方式取决于是否使用应用安装程序文件部署应用程序包。 若要从代码应用更新，应用包必须声明 `packageManagement` 功能。

本文提供的示例演示如何 `packageManagement` 在包清单中声明功能，以及如何从代码应用更新。 如果你使用的是应用程序安装程序文件，第二部分介绍了如何执行此操作，而第二部分介绍了如何在**不**使用应用程序安装程序文件时执行此操作。 最后一节将介绍如何在应用更新后确保应用重启。

## <a name="add-the-packagemanagement-capability-to-your-package-manifest"></a>将 PackageManagement 功能添加到包清单

若要使用 `PackageManager` api，应用必须 `packageManagement` 在[包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中声明[受限功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

## <a name="updating-packages-deployed-using-an-app-installer-file"></a>使用应用安装程序文件更新部署的包

如果你使用应用程序安装程序文件部署应用程序，则你执行的任何代码驱动的更新都必须使用[应用程序安装程序文件 api](https://docs.microsoft.com/windows/msix/app-installer/app-installer-documentation#app-installer-file-apis)。 这样做可确保定期应用安装程序文件更新将继续工作。 若要从代码初始化应用程序安装程序的更新，可以使用[PackageManager. AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync?view=winrt-19041)或[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync?view=winrt-19041)。 可以使用[CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync?view=winrt-19041) API 检查更新是否可用。 下面是示例代码：

```csharp
using Windows.Management.Deployment;

public async void CheckForAppInstallerUpdatesAndLaunchAsync(string targetPackageFullName, PackageVolume packageVolume)
{
    // Get the current app's package for the current user.
    PackageManager pm = new PackageManager();
    Package package = pm.FindPackageForUser(string.Empty, targetPackageFullName);

    PackageUpdateAvailabilityResult result = await package.CheckUpdateAvailabilityAsync();
    switch (result.Availability)
    {
        case PackageUpdateAvailability.Available:
        case PackageUpdateAvailability.Required:
            //Queue up the update and close the current instance
            await pm.AddPackageByAppInstallerFileAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.appinstaller"),
            AddPackageByAppInstallerOptions.ForceApplicationShutdown,
            packageVolume);
            break;
        case PackageUpdateAvailability.NoUpdates:
            // Close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
        case PackageUpdateAvailability.Unknown:
        default:
            // Log and ignore error.
            Logger.Log($"No update information associated with app {targetPackageFullName}");
            // Launch target app and close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
    }
}
```

## <a name="updating-packages-deployed-without-an-app-installer-file"></a>更新没有应用安装程序文件部署的包


### <a name="check-for-updates-on-your-server"></a>检查服务器上的更新

如果使用的**不**是应用安装程序文件来部署应用程序包，则第一步是直接检查是否有新版本的应用程序可用。 下面的示例将检查服务器上包的版本是否大于应用程序 (的当前版本，此示例引用测试服务器) 演示目的。

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

### <a name="apply-the-update"></a>应用更新 

确定可用更新后，可以使用[AddPackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackageasync?view=winrt-19041) API 将其排队，以便下载和安装。 下次关闭应用时，将应用此更新。 应用重新启动后，新版本将可供用户使用。 下面是示例代码：

```csharp

// Queue up the update and close the current app instance.
private async void CommandInvokedHandler(IUICommand command)
{
    if (command.Label == "Update")
    {
        PackageManager packagemanager = new PackageManager();
        await packagemanager.AddPackageAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.msix"),
            null,
            AddPackageOptions.ForceApplicationShutdown
        );
    }
}
```

## <a name="automatically-restarting-your-app-after-an-update"></a>更新后自动重启应用

如果应用程序是 UWP 应用，则在应用更新时传入 AddPackageByAppInstallerOptions 或 AddPackageOptions 应计划在关闭 + 更新后重新启动应用。 对于非 UWP 应用，需要在应用更新之前调用[RegisterApplicationRestart](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions#updates) 。

在应用开始关闭之前，必须先调用 RegisterApplicationRestart。 下面的示例使用互操作服务调用 c # 中的本机方法：

```csharp
 // Register the active instance of an application for restart in your Update method
 uint res = RelaunchHelper.RegisterApplicationRestart(null, RelaunchHelper.RestartFlags.NONE);
```

使用 c # 调用本机 RegisterApplicationRestart 方法的帮助器类的示例：

```csharp
using System;
using System.Runtime.InteropServices;

namespace MyEmployees.Helpers
{
    class RelaunchHelper
    {
        #region Restart Manager Methods
        /// <summary>
        /// Registers the active instance of an application for restart.
        /// </summary>
        /// <param name="pwzCommandLine">
        /// A pointer to a Unicode string that specifies the command-line arguments for the application when it is restarted.
        /// The maximum size of the command line that you can specify is RESTART_MAX_CMD_LINE characters. Do not include the name of the executable
        /// in the command line; this function adds it for you.
        /// If this parameter is NULL or an empty string, the previously registered command line is removed. If the argument contains spaces,
        /// use quotes around the argument.
        /// </param>
        /// <param name="dwFlags">One of the options specified in RestartFlags</param>
        /// <returns>
        /// This function returns S_OK on success or one of the following error codes:
        /// E_FAIL for internal error.
        /// E_INVALIDARG if rhe specified command line is too long.
        /// </returns>
        [DllImport("kernel32.dll", CharSet = CharSet.Unicode)]
        internal static extern uint RegisterApplicationRestart(string pwzCommandLine, RestartFlags dwFlags);
        #endregion Restart Manager Methods

        #region Restart Manager Enums
        /// <summary>
        /// Flags for the RegisterApplicationRestart function
        /// </summary>
        [Flags]
        internal enum RestartFlags
        {
            /// <summary>None of the options below.</summary>
            NONE = 0,

            /// <summary>Do not restart the process if it terminates due to an unhandled exception.</summary>
            RESTART_NO_CRASH = 1,
            /// <summary>Do not restart the process if it terminates due to the application not responding.</summary>
            RESTART_NO_HANG = 2,
            /// <summary>Do not restart the process if it terminates due to the installation of an update.</summary>
            RESTART_NO_PATCH = 4,
            /// <summary>Do not restart the process if the computer is restarted as the result of an update.</summary>
            RESTART_NO_REBOOT = 8
        }
        #endregion Restart Manager Enums

    }
}
```
