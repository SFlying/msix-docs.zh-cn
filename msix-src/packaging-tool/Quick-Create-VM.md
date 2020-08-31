---
title: Hyper-V Quick Create 中的 MSIX 打包环境
description: 使用 Hyper-V Quick Create 功能创建 MSIX 打包项目的虚拟环境。
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX 打包工具, Hyper-V Quick Create
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 33571ce00065ac6ba7e1b135bc4467504aca8c00
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090945"
---
# <a name="msix-packaging-environment-on-hyper-v-quick-create"></a>Hyper-V Quick Create 中的 MSIX 打包环境
 
可以使用 [Hyper-V Quick Create](/virtualization/hyper-v-on-windows/quick-start/quick-create-virtual-machine) 功能创建 MSIX 打包项目的虚拟环境。 此功能已从 Windows 10 版本 1709 开始推出。

若要开始，请在“开始”菜单中键入“Hyper-V Quick Create”，选择“MSIX 打包工具环境”，然后单击“创建虚拟机”。******** 完成向导后，在 VM 上通过“开始”菜单启动 MSIX 打包工具。 或者，如果已打开 Hyper-v 管理器，请单击 "快速创建 ..."在应用程序的右上角，它将显示相同的 UI。

.MSIX 打包工具环境是自定义 Windows 10 评估版本 (1909 版) ，其中包括最新版本的 .MSIX 打包工具和其他必备组件，以便您可以快速开始使用有限的设置任务。

![quickCreatepic1](images/QuickCreateVM.png)

> [!NOTE]
> 此功能需要 Hyper-V。 可在[此处](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)了解 Hyper-V 及其启用方法。