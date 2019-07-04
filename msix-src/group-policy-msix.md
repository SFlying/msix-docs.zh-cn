---
title: 组策略如何与 MSIX 打包的应用配合工作
description: 介绍组策略如何与转换成 MSIX 的应用配合工作
author: dianmsft
ms.author: diahar
ms.date: 04/12/2019
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: d85f37e08d0748b1e8cab5cfc288def793c92d02
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "66308439"
---
# <a name="group-policy-and-msix-packaged-apps"></a>组策略和 MSIX 打包应用

使用 MSIX 的开发人员可以利用类似于任何其他安装程序类型的组策略。

如果你已将 Win32 应用打包成 MSIX（或者使用桌面桥生成了应用），则该应用已启用完全信任功能。 这样，便可以从组策略注册表项中读取数据。 在运行时，你的应用获得的组策略注册表视图与任何其他应用相同。 如果你的应用是通用 Windows 平台 (UWP) 应用，则从 Windows 10 1809 开始，该应用可以访问相同的组策略注册表项。 有关创建组策略的详细信息，请参阅[此文](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3)。

如果你正在使用 [MSIX 打包工具](mpt-overview.md)将现有的安装程序转换为 MSIX，则应用无需执行新的操作即可支持组策略。 你可以像平时使用原始安装程序时那样管理组策略。 转换为 MSIX 的应用仍可以从现有的组策略注册表项中读取数据。 

组策略原生并不支持安装 MSIX 应用程序。 