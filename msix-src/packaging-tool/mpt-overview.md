---
title: MSIX 打包工具概述
description: 有关如何开始使用 MSIX 打包工具的概述文档
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: ee9d47ef88f8281a707550b67cf976da29efe9e2
ms.sourcegitcommit: f47c140e2eb410c2748be7912955f43e7adaa8f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72776545"
---
# <a name="msix-packaging-tool"></a>MSIX 打包工具 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

使用 MSIX 打包工具可将现有的 Win32 应用程序重新打包为 MSIX 格式。 此工具提供交互式 UI 和命令行用于转换，并且可以在没有源代码的情况下转换应用程序。 我们希望 IT 专业人员能够将现有资产转换为 MSIX，使其能够更好地进行打包和应用管理。

MSIX 打包工具现已在 Microsoft Store 中提供。 可以通过此工具运行桌面安装程序，并获取可在计算机上安装的 MSIX 包。

如果你有意成为 MSIX 打包工具的预览体验成员，请单击[此处](insider-program.md)了解更多详细信息。

## <a name="prerequisites"></a>必备条件

- Windows 10 版本 1809（或更高版本）
- 参与 Windows 预览体验计划（如果使用的是预览体验内部版本）
- 用于访问 Microsoft Store 中的应用的有效 Microsoft 帐户 (MSA) 别名 
- 电脑上的管理员特权（可运行该工具）
 
 ## <a name="install"></a>安装
 
若要从 Microsoft Store 安装 MSIX 打包工具，请转[此处](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf)，并确保使用用于 Windows 预览体验计划的 MSA 登录。 接下来，转到产品说明页，并单击“安装”图标开始安装。

也可以从适用于企业的 Microsoft Store [Web 门户](https://businessstore.microsoft.com/)下载 MSIX 打包工具供离线使用。 可在[此处](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)详细了解离线分发。

 
## <a name="latest-public-version---1201910180"></a>最新公共版本 - 1.2019.1018.0

### <a name="new-features"></a>新增功能：
- Device Guard 签名功能现已提供。 此签名选项需要一个已为适用于企业的 Microsoft Store 配置的活动 Microsoft Azure Active Directory 帐户。 有关详细信息，请参阅[此文章](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)。
- **包编辑器**现在支持选择要执行某个操作的多个项目的功能。
- 现在支持右键单击以编辑 MSIX 包。
- 用户体验对打包工作流的改进。

可在[此处](release-notes/history.md)找到 MSIX 打包工具发行说明的完整历史记录。

 ## <a name="tasks"></a>任务
 
下面是使用此工具预期可以实现的目的：
 
- 通过启动该工具并选择“应用程序包”图标，以 MSIX 格式将你偏爱的应用程序（msi、exe、App-V 5.x 和自定义脚本）打包。 
- 通过启动该工具并选择“修改包”图标，为 MSIX 包创建修改包。  
- 通过选择“包编辑器”图标，浏览到 MSIX 包并选择“打开包”，来打开 MSIX 包以查看和编辑其内容/属性。  

## <a name="try-it-out"></a>试用 

以下文章是有关如何使用 MSIX 打包工具转换桌面应用程序的教程： 

| 文章 | 描述 |
|-------|-------------|
| [从 MSI/App-V 文件创建 MSIX 包](create-app-package-MSI-VM.md) | 此教程介绍如何使用 MSIX 打包工具的 UI 将桌面应用程序（具体而言，是 MSI、EXE 或 App-V 等安装程序）转换为 MSIX 包。 |
| [从其他安装程序类型创建 MSIX 包](create-other-installer.md) | 此教程介绍如何使用 MSIX 打包工具的 UI 将桌面应用程序（批处理脚本、PowerShell 等安装程序）转换为 MSIX 包。 |
| [使用 MSIX 打包工具的命令行接口创建 MSIX 包](package-conversion-cli.md) | 此教程介绍如何使用 MSIX 打包工具的命令行接口将桌面应用程序转换为 MSIX 包。 |
| [自动执行 MSIX 包转换](automate-conversion.md) | 此教程介绍如何使用命令行接口将桌面应用程序自动转换为 MSIX 包。 |
| [在远程设备上创建 MSIX 包](remote-conversion-setup.md) | 此文提供有关如何在远程设备上将桌面应用程序转换为 MSIX 包的说明。 |
