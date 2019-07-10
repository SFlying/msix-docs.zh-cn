---
title: MSIX 打包工具发行说明
description: MSIX 打包工具发行说明的完整历史记录
author: c-don
ms.author: cdon
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10、 uwp、 msix、 msix 打包工具、 预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 6eb6f4cedc8b25046450080c0a6a2e07b0a4fb12
ms.sourcegitcommit: d505622e4f89daa0a9e7c7df0bd2228fd8ab792d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2019
ms.locfileid: "67694982"
---
# <a name="msix-packaging-tool-release-notes"></a>MSIX 打包工具发行说明

## <a name="version-120197010---public-release"></a>版本 1.2019.701.0-公开发布的版本

- 支持桌面安装程序需要重新启动-[了解详细信息](../support-restart.md)
    - 重新启动的自动登录选项 
- 应用设置中的新选项
    - 指定的默认证书进行签名的包 
    - 对于需要重新启动的安装程序指定的退出代码
    - 退出代码中设置的重新启动列表包括常见的默认值
- 添加新、 未知与自定义功能，通过包编辑器功能
- 自动设置为 1709 设置中关闭了应用商店版本控制要求时的 MinVersion
- 在包编辑器中，可以将新文件夹添加下资产
- 还原默认设置和排除项现在也清除签名证书的密码并退出代码
- 修复了第一个启动任务未获得正确删除
- 包创建期间将忽略排除的文件的快捷方式
- 默认值为如果在设置中指定默认签名证书对程序包进行签名
- 使用 PowerShell 安装程序退出代码
- 在需要重新启动的驱动程序时通知用户
