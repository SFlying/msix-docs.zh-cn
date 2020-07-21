---
title: .MSIX 打包工具发行说明
description: 本文提供了 .MSIX 打包工具不同版本发行说明的完整历史记录。
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10、uwp、.msix、.msix 打包工具、预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 68c627110ef2212f241fc44046ce4d19cb58b54c
ms.sourcegitcommit: 642563e98a52d4cc384987e618c5482022e29aba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86556252"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>MSIX 打包工具发行说明

## <a name="version-120207090---public-release"></a>版本 1.2020.709.0-公共版本
- 能够将多个文件添加到包编辑器
- 能够将多个 .reg 文件导入包编辑器
- 改进了对转换任何安装程序类型的支持
- 将项从包编辑器添加到排除列表的功能
- 在包编辑器中使用 ctrl 键选择多个选项
- 在移动或添加已修复 bug 时添加了覆盖文件的提示：
- 禁用未提供 Hyper-v 时选择本地 vm 计算机
- 选中 "强制实施存储要求" 复选框后，禁止创建不被存储区接受的图标
- 修复了转换期间 App-v 注册表值的问题
- 为远程命令行转换添加了较长的超时会话
- 改进了 .MSIX 核心 OS 选择，以减少冲突和混淆

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

