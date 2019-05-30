---
title: MSIX 打包工具预览体验计划
description: 了解有关打包工具预览体验计划以及如何联接的详细信息
author: c-don
ms.author: cdon
ms.date: 02/14/2019
ms.topic: article
keywords: windows 10，uwp，MSIX，MSIX 打包工具
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 11461bd93c7ef4147077c34585cce8968ce6c588
ms.sourcegitcommit: b3564e47328d21916cdeb4c84d638ac12be0a461
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66186163"
---
# <a name="msix-packaging-tool-insider-program"></a>MSIX 打包工具预览体验计划

MSIX 打包工具预览体验计划提供了提前访问 IT 专业人员和开发人员感兴趣将现有桌面应用程序转换成 MSIX 包。 该程序为您提供评估 MSIX 打包工具的常规版本之前的新功能的机会。 更重要的是，可以提供反馈以帮助形状以满足特定的业务需求的工具。 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://aka.ms/MSIXPackagingPreviewProgram" data-linktype="external">单击此处参加</a></p></div>

## <a name="prerequisites"></a>先决条件
- Windows 10，版本 1809 （或更高版本）。
- 有效 Microsoft 帐户别名从 Microsoft Store 中访问该应用。
- 若要运行该工具在 PC 上的管理员权限。

## <a name="install"></a>安装

加入后该程序，将收到一封电子邮件，确认你的注册。 

从 Microsoft Store 安装 MSIX 打包工具[此处](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf)。 请使用用于注册 MSIX 打包工具预览体验计划的 Microsoft 帐户登录。 接下来，转到产品描述页中，单击**安装**以开始安装。

如果您的计算机上已安装该工具，检查已安装的版本。 运行 MSIX 打包工具，单击右上角上的齿轮图标，然后单击**有关**选项卡中看到版本。 应用程序版本应与当前 Insider Preview 生成从部分匹配[如下](#current-insider-preview-build)。 

### <a name="current-insider-preview-build"></a>当前 Insider Preview 版本 

#### <a name="ver-120195220"></a>Ver 1.2019.522.0

新功能：

- 支持桌面安装程序需要重新启动-[了解详细信息](support-restart.md)
    - 重新启动的自动登录选项 
- 应用设置中的新选项
    - 指定的默认证书进行签名的包 
    - 对于需要重新启动的安装程序指定的退出代码
    
已知问题：

- 目前不支持负的重新启动退出代码
- 如果指定默认证书，则每个转换工作流将需要选择使用证书
- 在远程或虚拟机重启，可能会出现额外的登录提示 
- 还原默认设置按钮不会删除证书密码或安装程序退出代码
- 有一些 UI incongruencies

您可以找到 MSIX 打包工具发行说明的完整历史记录[此处](release-notes/history.md)。

## <a name="share-your-feedback"></a>共享你的反馈 

如果使用应用时遇到问题，请按**Windows 键 + F**以启动**反馈中心**。 提供有关该问题以帮助我们诊断并解决问题的可能的尽可能多的详细信息。 

此外可以共享反馈直接从应用中。 单击 Settings(gear icon) 主屏幕上，然后选择**反馈**选项卡上，选择最能代表你的问题的按钮。 这将直接启动**反馈中心**并填充代表你的必要类别信息。 

**反馈中心**也是共享想法和你想要在应用中看到的新功能的建议的好办法。  

## <a name="faqs"></a>常见问题

1. 我没有收到确认我注册到预览体验计划的电子邮件。 
    - 请[加入该计划](https://aka.ms/MSIXPackagingPreviewProgram)试。  

2. 我注册到预览体验计划，但我的计算机上都没有 MSIX 打包工具的 Insider Preview 版本。 
    - 从 Microsoft Store 的更新都逐渐将向在世界各地的用户并获取自动更新时可能会出现延迟。 你可以通过转到触发更新检查[工具页](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf)存储区中。 
3. 我想要选择退出的 MSIX 打包工具预览体验计划。 
    - 很遗憾看到你退出计划。 在表单中填写[此处](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-NSOqDz219PqoOqk5qxQEZUMlEwNVNKMDhNUVlKOVpTRTlVWFhMMThLQy4u)程序取消订阅。 
