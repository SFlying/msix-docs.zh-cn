---
Description: 本指南说明了如何使用 Microsoft 终结点管理器管理控制台创建和部署 MSIX 应用
title: Microsoft 终结点管理器管理控制台
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, mem 管理控制台, 应用
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f0ed835c3eb94dda358fbf88fe587bcb44277af1
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073913"
---
# <a name="microsoft-endpoint-manager-admin-console"></a>Microsoft 终结点管理器管理控制台
[Microsoft 终结点管理器管理控制台](https://devicemanagement.microsoft.com)可用于将 MSIX 程序包部署到客户端设备。 通过 Microsoft 终结点配置管理器管理控制台部署 MSIX 应用时，你需要具有要部署的 MSIX 应用，以及要用作安装目标的 Azure Active Directory 组。

## <a name="deploying-msix-application"></a>部署 MSIX 应用程序
从 Microsoft 终结点管理器管理控制台中部署要交付的应用时，你首先需要具有 MSIX 程序包的本地或网络路径。 将从 MSIX 应用程序包中包含的元数据中检索通过 Microsoft 终结点管理器管理控制台创建的应用的详细信息，并自动将检索到的信息加载到应用属性中。

|||
|-----|------|
| 1.从安装程序捕获 MSIX 程序包，或者检索现有的 MSIX 应用 | [通过桌面安装程序创建 MSIX 程序包](../packaging-tool/create-app-package-msi-vm.md)  |
| 2.启动 Microsoft 终结点管理器管理控制台 | [Microsoft 终结点管理器管理控制台](https://devicemanagement.microsoft.com) |
| 3.创建和部署业务线应用 | [创建业务线应用](https://docs.microsoft.com/intune/apps/lob-apps-windows) |
