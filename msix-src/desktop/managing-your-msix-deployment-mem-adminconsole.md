---
description: 本指南说明了如何使用 Microsoft 终结点管理器管理控制台创建和部署 MSIX 应用
title: Microsoft 终结点管理器管理控制台
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, mem 管理控制台, 应用
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6073f787630751df88734f293b65e63a90bebc02
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090515"
---
# <a name="microsoft-endpoint-manager-admin-console"></a>Microsoft 终结点管理器管理控制台
[Microsoft 终结点管理器管理控制台](https://devicemanagement.microsoft.com)可用于将 MSIX 程序包部署到客户端设备。 通过 Microsoft 终结点配置管理器管理控制台部署 MSIX 应用时，你需要具有要部署的 MSIX 应用，以及要用作安装目标的 Azure Active Directory 组。

## <a name="deploying-msix-application"></a>部署 MSIX 应用程序
从 Microsoft 终结点管理器管理控制台中部署要交付的应用时，你首先需要具有 MSIX 程序包的本地或网络路径。 将从 MSIX 应用程序包中包含的元数据中检索通过 Microsoft 终结点管理器管理控制台创建的应用的详细信息，并自动将检索到的信息加载到应用属性中。

|||
|-----|------|
| 1.从安装程序捕获 MSIX 程序包，或者检索现有的 MSIX 应用 | [通过桌面安装程序创建 MSIX 程序包](../packaging-tool/create-app-package.md)  |
| 2.启动 Microsoft 终结点管理器管理控制台 | [Microsoft 终结点管理器管理控制台](https://devicemanagement.microsoft.com) |
| 3.创建和部署业务线应用 | [创建业务线应用](/intune/apps/lob-apps-windows) |
