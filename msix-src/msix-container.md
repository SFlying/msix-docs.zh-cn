---
title: .MSIX 容器
description: 本文介绍 .MSIX 容器
ms.date: 02/04/2020
ms.topic: article
author: dianmsft
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 760491c0cbfe31bb47e5deb201e43d885101b631
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073483"
---
# <a name="msix-container"></a>.MSIX 容器

使用 .MSIX 打包的应用在轻型应用容器中运行。 .MSIX 应用进程及其子进程在容器中运行，并使用文件系统和注册表虚拟化进行隔离。 所有 .MSIX 应用程序都可以读取全局注册表。 .MSIX 应用写入其自己的虚拟注册表和应用程序数据文件夹，并在卸载或重置应用时删除此数据。 其他应用无法访问 .MSIX 应用的虚拟注册表或虚拟文件系统。

有关详细信息，请访问[了解 .msix 包在 Windows 上的运行方式](desktop/desktop-to-uwp-behind-the-scenes.md)。 
