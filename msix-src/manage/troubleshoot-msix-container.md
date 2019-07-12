---
Description: 本指南介绍如何排查 MSIX 容器中的运行时问题。
title: 对 MSIX 容器中的运行时问题进行故障排除
ms.date: 07/11/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: fdfaedf51333f93edd7e12f1c2ad8ea879bfc8ae
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829386"
---
# <a name="troubleshoot-runtime-issues-in-an-msix-container"></a>对 MSIX 容器中的运行时问题进行故障排除 

在本文中，我们将回顾如何排查 MSIX 容器中发生运行时问题。 MSIX 容器本身是相对较简单明了。 更多应用程序运行在相同的包标识内修改包的帮助，因为虚拟注册表和虚拟文件系统将转移放置在其中安装应用程序的顺序。 

可以在其中安装这些应用程序的顺序可能导致意外问题，预期的注册表项可能会覆盖和预期的文件可能被替换的情况。 

若要帮助诊断此类问题[Invoke CommandInDesktopPackage](https://docs.microsoft.com/en-us/powershell/module/appx/invoke-commandindesktoppackage?view=win10-ps)是一个可用于运行 MSIX 容器内的应用程序的 PowerShell cmdlet。 这样，用户可运行命令提示符下，注册表编辑器中，MSIX 容器内的 PowerShell 和获取合并的文件系统和合并的注册表配置单元的视图。 

 > [!IMPORTANT]
 > 调用 CommandInDesktopPackage 要求设备为开发人员模式。 


## <a name="view-the-merged-file-system"></a>查看合并的文件系统

若要查看文件系统为在容器内部运行的应用程序已观察到，请使用以下 PowerShell 命令：

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "29270sandstorm.AppPackage1_gah1vdar1nn7a" -Command "cmd.exe" -PreventBreakaway
```

上述命令将启动 cmd.exe 中的一个实例*29270sandstorm。AppPackage1_gah1vdar1nn7a*包容器。 在运行命令提示符下从容器内时，您可以浏览文件系统并查看合并的文件。 

## <a name="view-the-merged-registry-hive"></a>查看合并的注册表配置单元

若要为通过正在运行容器内部的应用程序已观察到，请查看完整的设备注册表配置单元，请使用以下 PowerShell 命令：

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "29270sandstorm.AppPackage1_gah1vdar1nn7a" -Command "regedit.exe" -PreventBreakaway
```

上述命令将启动注册表编辑器中的上下文中*29270sandstorm。AppPackage1_gah1vdar1nn7a*包容器。 此处您可以浏览本地计算机和当前用户注册表项，并标识可能导致出现此问题的恶意程序。 

 >[!TIP]
 > 使用-PreventBreakaway 时使用 Invoke CommandInDesktopPackage，如果你想要启动后续进程在同一容器中的标志。 否则，任何后续启动将会脱离容器。 

 >[!NOTE]
 > 并非所有应用程序可以在容器内启动。 例如，explorer.exe 将容器的多个专题。
