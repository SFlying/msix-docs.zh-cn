---
title: 组策略如何与 MSIX 一起打包的应用
description: 介绍组策略应用到 MSIX 已转换的工作方式
author: dianmsft
ms.author: diahar
ms.date: 04/12/2019
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: d85f37e08d0748b1e8cab5cfc288def793c92d02
ms.sourcegitcommit: 958d9e8177036c8485fb79b49f35cb2deb8d553c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308439"
---
# <a name="group-policy-and-msix-packaged-apps"></a>组策略和 MSIX 打包的应用

使用 MSIX 的开发人员可以利用组策略类似于任何其他安装程序类型。

如果已打包到 MSIX 的 Win32 应用程序 （或生成你的应用使用桌面桥），您的应用程序已启用完全信任功能。 这样，你可以读取组策略注册表项中。 在运行时，您的应用程序将在实际应用像任何其他应用程序具有相同的组策略注册表视图。 如果您的应用程序是通用 Windows 平台 (UWP) 应用，在 Windows 10 1809年中启动你的应用可以访问相同的组策略密钥。 有关创建组策略的详细信息，请参阅[这篇文章](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3)。

如果要将转换的现有安装程序为 MSIX 通过使用[MSIX 打包工具](mpt-overview.md)，没有新的工作所需的应用以支持组策略。 继续像通常那样原始安装程序的管理组策略。 应用程序转换为 MSIX 仍将能够读取现有组策略注册表项。 

组策略不具有安装 MSIX 应用程序的本机支持。 
