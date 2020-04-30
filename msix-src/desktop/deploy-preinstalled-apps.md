---
title: 预安装已打包的应用
description: 本文提供预装应用的概述
ms.date: 04/02/2020
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, msix, uwp, 可选包, 相关集, 包扩展, visual studio, dism, 预安装, 预先安装, 打包应用
ms.localizationpriority: medium
ms.openlocfilehash: be5c13d0374ed058934c920053f52a1fdc39df43
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "81613996"
---
# <a name="preinstalling-packaged-apps"></a>预安装已打包的应用
可以使用多个工具将 MSIX 打包应用安装到设备，供所有用户使用：

- 部署映像服务和管理 (DISM)
- 预配包
- PowerShell

本文概述预装应用的工作原理，以及预配和许可过程如何使用预装的应用。 

## <a name="overview"></a>概述
打包应用安装的预安装可以分为两个步骤： 
1. 分步
1. 注册

### <a name="staging"></a>分步
将打包应用暂存到设备，是将该打包应用的副本存储到本地文件系统的操作。 打包应用只能暂存一次，并且无需设备上拥有任何用户帐户即可执行。

可以在脱机映像（.wim、.vhd 或 .vhdx）或联机活动操作系统上执行打包应用的暂存。 

### <a name="registration"></a>注册
暂存打包应用后，可以将应用注册到设备上的用户。 注册按用户进行，在设备用户登录时开始。 然后，操作系统将加载预安装的打包应用包，以便在“开始”菜单中创建用户特定的应用数据、创建文件类型关联和应用磁贴。 这是由应用就绪服务 (ARS) 实现的，它可以识别所有预安装的应用。 

## <a name="dism"></a>DISM
DISM 是一个命令行工具，可用于服务和准备 WIndows 映像，包括用于 Windows 预执行 (Win-PE)、恢复环境 (Win) 和 Windows 安装程序的映像。 DISM 可用来维修 Windows 映像 (.wim) 或虚拟硬盘（.vhd 或 .vhdx）。

## <a name="provisioning-packages"></a>预配包
所有应用预配封装在 DISM 工具中，会同时执行暂存和 ARS 设置。 若要执行预配，IT 专业人员需要一个应用包（.msix、.msixbundle、.appx 或 .appxbundle）和任何依赖项包。 

从 Windows 10 1809 开始，IT 专业人员可以通过预配进行预安装。 预配的应用将安装到中心位置 %ProgramFiles%\WindowsApps，并且将立即可供注册用户使用。 只有将 MSIX 应用包注册到其帐户的用户才能访问该应用。

在 Windows 10 2004 中，将在重新预配期间重新安装预配的打包应用。 如果用户以前卸载了打包应用，以前版本的 Windows 10 会阻止重新安装这些打包应用。

## <a name="powershell"></a>PowerShell
相关 PowerShell 命令的列表
* **[Get-ProvisionedAppxPackages](https://docs.microsoft.com/powershell/module/dism/get-appxprovisionedpackage?view=win10-ps)** 此命令列出映像中预装的所有应用的列表。
* **[Add-ProvisionedAppxPackage](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps)** 此命令暂存 appx 包，并对其进行配置，以便可以预装。 还必须提供所有依赖项，可以在 SDK 或者从 Store 下载的包中找到这些依赖项。
* **[/Remove-ProvisionedAppxPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps)** 此命令可用于删除预装的应用。 请注意，如果已经为任何用户注册了应用，则此命令不会删除该应用 - 只会去掉自动注册行为，因此不会为任何新用户自动安装该应用。  如果没用任何用户安装该应用，则此命令还会删除暂存的文件。

## <a name="licensing"></a>授权
仅当预配 Windows Store 应用时，许可才适用。 任何其他应用无需许可证即可预配。 如果应用来自于 Store，则预配该应用时，还必须提供机器许可证。 目前，所有预装的 Windows Store 应用必须是免费应用，并配置为可通过 Windows Store 合作伙伴中心预装。 配置后，可以下载可预装的包和许可证，然后将其预配到任何映像中。

