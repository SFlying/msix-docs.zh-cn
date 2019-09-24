---
title: .MSIX 打包工具发行说明
description: .MSIX 打包工具发行说明的完整历史记录
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10、uwp、.msix、.msix 打包工具、预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 03a5b366f028a8686658cd3836c8b2ba5af84c55
ms.sourcegitcommit: 1c3a0f5c27ee741d26f693ad657754c4ab29ac73
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2019
ms.locfileid: "71189891"
---
# <a name="msix-packaging-tool-release-notes"></a>.MSIX 打包工具发行说明

### <a name="version-120199190"></a>版本1.2019.919。0
- Device Guard 签名现在可用。 此签名选项需要为 Business Microsoft Store 配置的活动 Microsoft Azure Active Directory 帐户。 有关详细信息，请参阅[此文章](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)。
- **包编辑器**现在支持选择要对其执行操作的多项功能。

### <a name="version-120198080"></a>版本 1.2019.808.0
- 设置改进
    - 从其他设置中分离出了工具默认设置
    - 添加了导入和导出设置的功能
- 签名改进
    - 现在可以为工作流选择默认签名选项
- 更新了打包过程中的步骤以改善体验

## <a name="version-120197010---public-release"></a>版本 1.2019.701.0-公共版本

- 支持需要重新启动的桌面安装程序-[了解详细信息](../support-restart.md)
    - 自动重启选项 
- 应用设置中的新选项
    - 指定用于对包进行签名的默认证书 
    - 指定需要重新启动的安装程序的退出代码
    - "设置" 中的 "退出代码" 列表中的列表包括常见的默认值
- 能够通过包编辑器添加新的、未知的和自定义的功能
- 在设置中关闭存储版本要求时，自动将 MinVersion 设置为1709
- 可以在 "包编辑器" 中的 "资产" 下添加新文件夹
- 还原默认设置和排除项现在还会清除签名证书密码和退出代码
- 修复了无法正确删除首次启动任务的问题
- 在包创建过程中将忽略排除的文件的快捷方式
- 如果在设置中指定了默认签名证书，则默认为对包进行签名
- 遵循 PowerShell 安装程序退出代码
- 当用户需要重新启动驱动程序时通知用户
