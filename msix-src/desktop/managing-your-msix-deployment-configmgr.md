---
Description: 本指南说明了如何使用 Microsoft Endpoint Configuration Manager 创建和部署 MSIX 应用
title: Microsoft Endpoint Configuration Manager
ms.date: 1/31/2020
ms.topic: article
keywords: windows 10, uwp, mecm, configmgr, 应用, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: eb136e71cec242efebe9129d2c6c8bee5fa62a35
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77074173"
---
# <a name="microsoft-endpoint-configuration-manager"></a>Microsoft Endpoint Configuration Manager
[Microsoft Endpoint Configuration Manager](https://docs.microsoft.com/configmgr/) 可用来创建可以交付到客户端设备的 `Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)`。 部署 MSIX 应用需要具备以下先决条件：
1) 对要部署的 MSIX 程序包的访问权限。
2) 用作安装目标的设备或用户集合。

## <a name="deploying-msix-application"></a>部署 MSIX 应用程序
从Microsoft Endpoint Configuration Manager 控制台中部署要交付的应用时，你首先需要具有 MSIX 应用程序包的 UNC 路径。 将从 MSIX 应用程序包中包含的元数据中检索要创建的应用的详细信息，并自动将这些详细信息加载到 Windows 应用程序包属性中。

|||
|-----|------|
| 1.从安装程序捕获 MSIX 程序包，或者检索现有的 MSIX 应用 | [通过桌面安装程序创建 MSIX 程序包](../packaging-tool/create-app-package-msi-vm.md)  |
| 2.启动 Microsoft Endpoint Configuration Manager 控制台 | [Microsoft Endpoint Configuration Manager 控制台](https://devicemanagement.microsoft.com) |
| 3.创建业务线应用 | [创建 Windows 应用程序包](https://docs.microsoft.com/configmgr/apps/get-started/creating-windows-applications) |
| 4.创建用户或设备集合 | [创建集合](https://docs.microsoft.com/configmgr/core/clients/manage/collections/create-collections) |
| 5.部署业务线应用 | [通过 ConfigMgr 部署应用程序](https://docs.microsoft.com/configmgr/apps/deploy-use/deploy-applications) |
