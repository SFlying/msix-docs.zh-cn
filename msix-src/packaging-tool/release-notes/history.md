---
title: .MSIX 打包工具发行说明
description: 本文提供了 .MSIX 打包工具不同版本发行说明的完整历史记录。
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10、uwp、.msix、.msix 打包工具、预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 2fb911d4515816a02ad74064e4f7db78efbcb5eb
ms.sourcegitcommit: f6cee51b46fc36a57b5cf9c8cb3fd24a40ae858a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391661"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>MSIX 打包工具发行说明

## <a name="version-120203190"></a>版本1.2020.319。0
- 已提取的图标的改进质量
- 从快捷方式改进了图标提取
- 一般 bug 修复

## <a name="version-120202250"></a>版本1.2020.225。0
- 能够在包编辑器中导入注册表文件（.reg）
- 能够自动将[.Msix Core 支持](../../msix-core/msixcore.md)添加到 .msix
- 添加 InstalledLocationVirtualization 扩展修复的 bug：
- 编辑后验证清单格式 
- 第一次启动任务失败时发出消息 
- 禁止解压缩相对路径 
- 更新了文件筛选器，因此它们显示了有效的格式（例如，对于它*使用的安装程序， ，* 现在 `*.msi, *.exe, ...`） 
- 修复了解包时将路径中的空格转换为 "%20"

## <a name="version-120202030"></a>版本1.2020.203。0
- 默认情况下启用包完整性设置
- "创建包" 页现在显示默认保存位置
- 能够导出注册表文件（.reg）
- 一般 bug 修复改进

## <a name="version-1201912200---public-release"></a>版本 1.2019.1220.0-公共版本
- 对转换带有服务的现有安装程序的支持现已提供
  - 在新的[“服务”报表页](../convert-an-installer-with-services.md)中查看检测到的所有服务并对其进行修改
- 用于指定默认转换环境的新设置
- 用于指定默认安装程序浏览位置的新设置
- 用于将“包完整性”添加到应用的新设置
- 向第一个启动任务页添加运行和删除按钮
- 添加了拖放文件以将其移动到包编辑器中的功能
- 添加了解决导致其无效的 COM 服务器路径的问题的修补程序
