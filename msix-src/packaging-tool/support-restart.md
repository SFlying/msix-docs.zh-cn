---
title: 对需要重启设备的桌面安装程序的支持
description: 介绍如何支持需要重启设备的桌面安装程序。
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX 打包工具, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8a48358c94e9c799ff285cd3d1e20940e79f4731
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829123"
---
# <a name="support-for-desktop-installers-that-require-device-restart"></a>对需要重启设备的桌面安装程序的支持

 > [!NOTE] 
 > 设备重启是当前仅在 Insider Preview 内部版本中可用的一项新功能。
 > 加入 MSIX 打包工具预览体验计划即可访问此功能。

我们现在支持在转换过程中使用 MSIX 打包工具重启的功能！ 

可以通过交互式 UI 和命令行接口支持重启。 此外，通过命令行还可以指定在重启后自动登录，以顺利地进行自动转换。 

可以在“设置(安装程序退出代码)”页中指定哪些退出代码表示重启。  

可以在执行转换工作流期间通过 UI 触发重启，或者通过转换计算机的开始菜单触发重启。 发生重启后，你将返回到先前在工具中的同一位置，以继续转换。

请记住，此功能仍在开发中，可能还有一些 bug。 如果发现任何 bug，请务必[提交反馈](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/insider-program#share-your-feedback)，让我们修复它！

