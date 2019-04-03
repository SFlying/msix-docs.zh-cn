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
ms.openlocfilehash: dd04eb516e41d9de8650895a78f5eb99ccfe6653
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900449"
---
# <a name="msix-packaging-tool"></a>MSIX 打包工具 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

MSIX 打包工具，可重新打包现有 Win32 应用程序为 MSIX 格式。 它提供了交互式 GUI 和命令行的转换，并使你能够将应用程序而无需的源代码。 我们想要使 IT 专业人员能够将现有的资产转换为 MSIX，若要为其提供更好的方式进行打包和应用管理。

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

 
 ## <a name="whats-new"></a>新增功能
 **v1.2019.110.0**
- 改进了的打包时间 
- 更新的默认文件的排除列表
- 合并到工具报告 MSIExec 错误日志
- 已更新的日志，以添加更多的清晰度和故障排除步骤
- 添加了的对捕获从 PowerShell ISE 安装在手动打包过程
- 添加了对作为安装程序 UI 和命令行模板文件中的参数声明的 PowerShell 脚本支持
- 添加详细日志记录标志 (-verbose |-v) 的命令行接口
- 修复了有时无法访问运行在 VM 上的网络路径是
- 修复了在其中存储版本控制要求验证失败时使用命令行接口
- 修复了以下问题已不接受文件路径引用中的
- 修复了问题，其中 VM 不正确清理转换后
- 修复了问题，将文件添加到包编辑器中的包已不能正常工作
- UI 清理 


 ## <a name="tasks"></a>任务
 
下面是可以预期要能够使用此工具执行操作：
 
- 打包你最喜欢的应用程序 (msi、 exe、 APP-V 5.x 和为 MSIX 格式由启动该工具，然后选择**应用程序包**图标。
- 通过启动工具并选择为新创建的应用程序 MSIX 包创建修改包**修改包**图标。 
- 打开 MSIX 包以进行查看和编辑其内容/属性通过导航到**编辑器中打开包**选项卡上，浏览到 MSIX 包，并选择**打开包**。

