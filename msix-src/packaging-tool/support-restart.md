---
title: 对需要重启设备的桌面安装程序的支持
description: 介绍如何支持需要重启设备的桌面安装程序。
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX 打包工具, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b8c134d460764699cdcac082941df606df3c0bd3
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090775"
---
# <a name="support-for-desktop-installers-that-require-device-restart"></a>对需要重启设备的桌面安装程序的支持

从1.2019.701.0 版本的 .MSIX 打包工具开始，你可以在安装过程中触发重启。

通过交互式用户界面和命令行都支持重启。 此外，通过命令行，还可以指定在重新启动后自动登录，以便进行无缝的无人参与转换。 

你可以在 "设置" > [安装程序退出代码](tool-best-practices.md#other-settings) "页中指定哪些退出代码指示重启。 

可以在执行转换工作流期间通过 UI 触发重启，或者通过转换计算机的开始菜单触发重启。 发生重启后，你将返回到先前在工具中的同一位置，以继续转换。

我们始终希望收到你的反馈，因此如果你有任何反馈，请确保与我们进行 [反馈](./insider-program.md#share-your-feedback) 。