---
title: .MSIX 打包工具发行说明
description: 本文提供了 .MSIX 打包工具不同版本发行说明的完整历史记录。
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、uwp、.msix、.msix 打包工具、预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: 9618cbebe07696f9806fcdd07acdd8c5d834724e
ms.sourcegitcommit: 44b9510ea76623d668d87ddca575a7921c60a19a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2019
ms.locfileid: "75322612"
---
# <a name="release-notes-for-the-msix-packaging-tool"></a>.MSIX 打包工具的发行说明

## <a name="version-1201912180"></a>版本1.2019.1218。0

- 添加了拖放文件以将其移动到包编辑器中的功能
- 将包完整性添加到应用的新设置
- 在 "创建包" 页上显示默认保存位置

## <a name="version-1201910280"></a>版本1.2019.1028。0

- 现在提供了使用服务转换现有安装程序的支持
  - 查看检测到的所有服务并在 "新建[服务" 报表页](../convert-an-installer-with-services.md)中修改这些服务
- 用于指定默认转换环境的新设置
- 用于指定默认安装程序浏览位置的新设置
- 向第一个启动任务页添加运行和删除按钮

## <a name="version-1201910180---public-release"></a>版本 1.2019.1018.0-公共版本

- 已更新公共发布的字符串
- Device Guard 签名功能现已提供。 此签名选项需要一个已为适用于企业的 Microsoft Store 配置的活动 Microsoft Azure Active Directory 帐户。 有关详细信息，请参阅[此文](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)。
- **包编辑器**现在支持选择要执行某个操作的多个项目的功能。
- 现在支持右键单击以编辑 MSIX 包。
- 用户体验对打包工作流的改进。
- 设置改进：
    - 将工具默认设置为其他设置。
    - 添加了导入和导出设置的功能。
- 签名改进：
    - 你现在可以为工作流选择默认签名选项。
- 更新了打包过程中的步骤以改善体验。

