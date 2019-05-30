---
title: MSIX 打包工具概述
description: 如何开始使用 Msix 打包工具的概述文档
author: mcleanbyron
ms.author: mcleans
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 6432a4b33b93a6340acabfd6284f5f530b2fe8b9
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795429"
---
# <a name="msix-packaging-tool"></a>MSIX 打包工具 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

MSIX 打包工具，可重新打包现有 Win32 应用程序为 MSIX 格式。 它提供了交互式用户界面和命令行的转换，并使你能够将应用程序而无需的源代码。 我们想要使 IT 专业人员能够将现有的资产转换为 MSIX，若要为其提供更好的方式进行打包和应用管理。

从 Microsoft Store 现 MSIX 打包工具。 你可以运行此工具通过您桌面的安装程序并获得一个 MSIX 包，可以在计算机上安装。

想要 MSIX 打包工具深入了解，请单击[此处](packaging-tool/insider-program.md)的更多详细信息。

## <a name="prerequisites"></a>先决条件

- Windows 10，版本 1809 （或更高版本）
- 参加 Windows 预览体验计划 （如果使用的预览体验内部版本）
- 有效的 Microsoft 帐户 (MSA) 别名访问从 Microsoft Store 应用 
- 若要运行该工具在 PC 上的管理员权限
 
 ## <a name="install"></a>安装
 
若要从 Microsoft Store 安装 MSIX 打包工具，请转[此处](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf)，确保使用为你的 Windows 预览体验计划使用 MSA 登录。 接下来，转到产品说明页并单击安装图标以开始安装。

MSIX 打包工具还可以下载供脱机使用从 Microsoft Store 企业中的业务[web 门户](https://businessstore.microsoft.com/)。 您可以了解有关脱机分发的详细信息[此处](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)。

 
## <a name="latest-public-version---120194020"></a>最新的公共版本-1.2019.402.0

### <a name="new-features"></a>新的功能：

- 可以将远程计算机的上[的详细信息](packaging-tool/remote-conversion-setup.md)
- 改进了包编辑器中的管理体验
    - 在包的编辑器中保存时自动版本控制建议
    - 现在支持现有的文件夹添加 VFS 中打包
- 用户可以指定对 CLI 转换的已知有效的退出代码
- 添加了对时间戳已签名的包中所有其中签名是当前可用的工作流 
    - 在工具设置页中，可以指定您的默认时间戳 URL 和时间戳服务器的类型
- 更新[AppID 生成逻辑](packaging-tool/release-notes/history.md#appid-generation-logic)，并添加为包名称和应用程序的其他验证 

您可以找到 MSIX 打包工具发行说明的完整历史记录[此处](packaging-tool/release-notes/history.md)。

 ## <a name="tasks"></a>任务
 
下面是可以预期要能够使用此工具执行操作：
 
- 打包你最喜欢的应用程序 (msi、 exe、 APP-V 5.x 和自定义脚本) 中启动工具并选择 MSIX 格式**应用程序包**图标。
- 通过启动工具并选择为 MSIX 包创建修改包**修改包**图标。 
- 打开 MSIX 包以进行查看和编辑其内容/属性通过选择**包编辑器**图标，浏览到 MSIX 包，并选择**打开包**。

## <a name="try-it-out"></a>试用 

以下文章是有关如何使用 MSIX 打包工具将桌面应用程序的教程： 

| 文章 | 描述 |
|-------|-------------|
| [从 MSI/APP-V 文件创建 MSIX 包](packaging-tool/create-app-package-MSI-VM.md) | 本教程会演示如何使用 MSIX 打包工具的 UI 将转换到 MSIX 包对桌面应用程序 （尤其是如 MSI、 EXE 或 APP-V 的安装程序）。 |
| [从其他安装程序类型创建 MSIX 包](packaging-tool/create-other-installer.md) | 本教程将向演示如何使用 MSIX 打包工具的 UI 来转换你的桌面应用程序 （安装程序如批处理脚本，PowerShell 等） 到 MSIX 包。 |
| [创建 MSIX 包使用 MSIX 打包工具的命令行接口](packaging-tool/package-conversion-cli.md) | 本教程会演示如何使用 MSIX 打包工具命令行接口来转换到 MSIX 包桌面应用程序。 |
| [自动执行 MSIX 包转换](packaging-tool/automate-conversion.md) | 本教程将介绍如何使用命令行接口来自动执行桌面应用程序转换为 MSIX 包。 |
| [在远程设备上创建 MSIX 包](packaging-tool/remote-conversion-setup.md) | 本文将提供执行远程设备上的桌面应用程序转换为 MSIX 包所需的说明进行操作。 |