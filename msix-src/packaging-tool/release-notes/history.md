---
title: .MSIX 打包工具发行说明
description: 本文提供了 .MSIX 打包工具不同版本发行说明的完整历史记录。
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10、uwp、.msix、.msix 打包工具、预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 6aa47ea616a28694a9e22cb68a2a89a100e86629
ms.sourcegitcommit: 6c517bd2f6354db1a2c51217a208e1d2cfd466da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85295574"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>MSIX 打包工具发行说明

## <a name="version-120206180"></a>版本1.2020.618。0
- 为远程命令行转换添加了较长的超时会话
- 改进了 .MSIX 核心 OS 选择，以减少冲突和混淆

## <a name="version-120206030"></a>版本1.2020.603。0
- 修复了转换期间 App-v 注册表值的问题

## <a name="version-120205280"></a>版本1.2020.528。0
- 能够将多个文件添加到包编辑器
- 能够将多个 .reg 文件导入包编辑器
- 改进了对转换任何安装程序类型的支持
- Bug 修复：
    - 禁用未提供 Hyper-v 时选择本地 vm 计算机
    - 选中 "强制实施存储要求" 复选框后，禁止创建不被存储区接受的图标

## <a name="version-120204230"></a>版本1.2020.423。0
- 将项从包编辑器添加到排除列表的功能
- 在包编辑器中使用 ctrl 键选择多个选项
- 在移动或添加时覆盖文件时添加了提示

## <a name="version-120204020---public-release"></a>版本 1.2020.402.0-公共版本
- 默认情况下启用包完整性设置
- 能够自动将[.Msix Core 支持](../../msix-core/msixcore.md)添加到 .msix
- 能够在包编辑器中导入或导出注册表文件（.reg）
- "创建包" 页现在显示默认保存位置
- 添加 InstalledLocationVirtualization 扩展
- 已提取的图标的改进质量
- 从快捷方式修复的 bug 中改进了图标提取：
- 编辑后验证清单格式 
- 第一次启动任务失败时发出消息 
- 禁止解压缩相对路径 
- 更新了文件筛选器，因此它们显示了哪些格式有效（例如，用于表示的安装程序） *。*  现在 `*.msi, *.exe, ...` ） 
- 修复了解包时将路径中的空格转换为 "%20"
- 一般 bug 修复

## <a name="version-1201912200---public-release"></a>版本 1.2019.1220.0-公共版本
- 对转换带有服务的现有安装程序的支持现已提供
  - 在新的[“服务”报表页](../convert-an-installer-with-services.md)中查看检测到的所有服务并对其进行修改
- 用于指定默认转换环境的新设置
- 用于指定默认安装程序浏览位置的新设置
- 用于将“包完整性”添加到应用的新设置
- 向第一个启动任务页添加运行和删除按钮
- 添加了拖放文件以将其移动到包编辑器中的功能
- 添加了解决导致其无效的 COM 服务器路径的问题的修补程序
