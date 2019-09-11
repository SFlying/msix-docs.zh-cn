---
Description: 本指南说明如何排查 .MSIX 容器中的运行时问题。
title: 排查 .MSIX 容器中的运行时问题
ms.date: 07/11/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: c1a8d489ca93c075ce8b87e98fb2ada3bade7d91
ms.sourcegitcommit: 9cb3d2cdbe03b300bef60ed949e5e4d3b24d35ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70864018"
---
# <a name="troubleshoot-runtime-issues-in-an-msix-container"></a>排查 .MSIX 容器中的运行时问题 

本文介绍如何解决 .MSIX 容器中发生的运行时问题。 .MSIX 容器本身相对简单简单。 随着更多应用程序在包含修改包的同一包标识中运行，虚拟注册表和虚拟文件系统将按安装应用程序的顺序排列。 

在某些情况下，这些应用程序的安装顺序可能会导致意外问题，可能会覆盖所需的注册表项，并且可能会替换预期的文件。 

为了帮助诊断此类问题， [CommandInDesktopPackage](https://docs.microsoft.com/en-us/powershell/module/appx/invoke-commandindesktoppackage?view=win10-ps)是一个 PowerShell cmdlet，可用于在 .msix 容器中运行应用程序。 这样，用户便可以在 .MSIX 容器中运行命令提示符、注册表编辑器、PowerShell，并获得合并文件系统和合并的注册表配置单元。 

 > [!IMPORTANT]
 > CommandInDesktopPackage 要求设备处于18922之前的 Windows 10 内部版本的开发人员模式。


## <a name="view-the-merged-file-system"></a>查看合并文件系统

若要查看由容器内运行的应用程序观察到的文件系统，请使用以下 PowerShell 命令：

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "Contoso.AppPackage1_8h66172c634n0" -Command "cmd.exe" -PreventBreakaway
```

上述命令将在*AppPackage1_8h66172c634n0*包容器中启动 cmd.exe 的实例。 在容器内部运行命令提示符时，可以浏览文件系统并查看合并的文件。 

## <a name="view-the-merged-registry-hive"></a>查看合并的注册表配置单元

若要查看由运行有问必答容器的应用程序观察到的完整设备注册表配置单元，请使用以下 PowerShell 命令：

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "Contoso.AppPackage1_8h66172c634n0" -Command "regedit.exe" -PreventBreakaway
```

上述命令将在*AppPackage1_8h66172c634n0*包容器的上下文中启动注册表编辑器。 可在此处浏览 "本地计算机" 和 "当前用户" 注册表项，并确定导致此问题的可能的问题。 

 >[!TIP]
 > 如果要在同一容器中启动后续进程，请在使用 CommandInDesktopPackage 时使用 "-PreventBreakaway" 标志。 否则，任何后续启动都将中断容器。 

 >[!NOTE]
 > 并非所有应用程序都可以在容器中启动。 例如，资源管理器将对容器进行分类。
