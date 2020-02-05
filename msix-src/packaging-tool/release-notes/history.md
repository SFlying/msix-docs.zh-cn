---
title: .MSIX 打包工具发行说明
description: 本文提供了 .MSIX 打包工具不同版本发行说明的完整历史记录。
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、uwp、.msix、.msix 打包工具、预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: be79b90229ed23b04cfa58f12ee3372491f4b818
ms.sourcegitcommit: b54bb64bcc05a91b4444b62d52586f8a794e7267
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77027660"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>.MSIX 打包工具的发行说明

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
