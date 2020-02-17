---
Description: 本指南说明了如何使用 Microsoft Intune 创建和部署 MSIX 应用
title: Microsoft Intune
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, mem 管理控制台, 应用, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cf2fe754de71cd5242833c3bfd5b74f3c95b1a37
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073923"
---
# <a name="microsoft-intune"></a>Microsoft Intune
可以使用 [Microsoft Intune 控制台](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview)创建可以交付到客户端设备的业务线应用。 通过 Microsoft Intune 部署 MSIX 应用时，你需要具有要部署的 MSIX 应用，以及要用作安装目标的 Azure Active Directory 组。

## <a name="deploying-an-msix-app"></a>部署 MSIX 应用
从 Microsoft Intune 中部署要交付到客户端设备的客户端应用时，你首先需要具有 MSIX 应用的本地或网络路径。 将从 MSIX 应用程序包中包含的元数据中检索通过 Microsoft Intune 创建的应用的详细信息，并自动将检索到的信息加载到应用属性中。

|||
|-----|------|
| 1.从安装程序捕获 MSIX 程序包，或者检索现有的 MSIX 应用 | [通过桌面安装程序创建 MSIX 程序包](../packaging-tool/create-app-package.md) |
| 2.启动 Intune 控制台 | [Microsoft Intune 控制台](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) |
| 3.创建和部署业务线应用 | [创建业务线应用](https://docs.microsoft.com/intune/apps/lob-apps-windows) |
