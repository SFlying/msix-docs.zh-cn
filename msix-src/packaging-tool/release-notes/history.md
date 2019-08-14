---
title: .MSIX 打包工具发行说明
description: .MSIX 打包工具发行说明的完整历史记录
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10、uwp、.msix、.msix 打包工具、预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 676c58574457261adbeaf150279a8eda82b7fc7a
ms.sourcegitcommit: 031e481716f6f02ad2d5dd67d5eea35869c99c0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991424"
---
# <a name="msix-packaging-tool-release-notes"></a>.MSIX 打包工具发行说明

### <a name="version-120198080"></a>版本1.2019.808。0
- 设置改进
    - 从其他设置中分离工具默认值
    - 添加了导入和导出设置的功能
- 签名改进
    - 你现在可以为工作流选择默认签名选项
- 在打包过程中更新了步骤, 以改善体验

## <a name="version-120197010---public-release"></a>版本 1.2019.701.0-公共版本

- 支持需要重新启动的桌面安装程序-[了解详细信息](../support-restart.md)
    - 自动重启选项 
- 应用设置中的新选项
    - 指定用于对包进行签名的默认证书 
    - 指定需要重新启动的安装程序的退出代码
    - "设置" 中的 "退出代码" 列表中的列表包括常见的默认值
- 能够通过包编辑器添加新的、未知的和自定义的功能
- 在设置中关闭存储版本要求时, 自动将 MinVersion 设置为1709
- 可以在 "包编辑器" 中的 "资产" 下添加新文件夹
- 还原默认设置和排除项现在还会清除签名证书密码和退出代码
- 修复了无法正确删除首次启动任务的问题
- 在包创建过程中将忽略排除的文件的快捷方式
- 如果在设置中指定了默认签名证书, 则默认为对包进行签名
- 遵循 PowerShell 安装程序退出代码
- 当用户需要重新启动驱动程序时通知用户
