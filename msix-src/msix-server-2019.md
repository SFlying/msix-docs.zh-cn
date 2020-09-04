---
title: Server 2019 上的 MSIX 支持
description: 这篇文章详细介绍了 Server 2019 上的 MSIX 支持
ms.date: 01/16/2020
ms.topic: article
author: dianmsft
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: e897cd77785c345863c0e2e17bc4dbf8d73b854f
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091185"
---
# <a name="msix-support-on-windows-server-2019"></a>Windows Server 2019 上的 MSIX 支持

现在，具有桌面体验的 Windows Server 2019 (LTSC) 上支持 MSIX。 利用这种支持，你可以在企业中的客户端和服务器 SKU 上分发相同的 MSIX 包，通过 **PowerShell** 安装，或直接通过 [Package Manager API](/uwp/api/Windows.Management.Deployment.PackageManager) 安装。

## <a name="considerations"></a>注意事项

* Windows Server 2019 上不支持 Microsoft Store 和适用于企业的 Microsoft Store。 必须使用 PowerShell 安装所有应用程序。 
* 因为 Windows Server 2019 是基于 Windows 10 版本 1809 的，所以它支持最低目标版本为 10.0.17763 或以下的应用程序。 
* 以下 API 和框架在 Windows Server 2019 上不可用：
  * Microsoft Advertising
  * 地理位置
  * Cortana 搜索
  * 应用内购买
* Windows Server 2019 上未安装以下框架。 你有责任确保它们在需要时与你的应用程序一起安装。
  * .NET 运行时
  * .NET Framework
  * VCLibs
* Windows Server 2019 不支持以下包扩展：
  * windows.firewallRules
  * windows.appExecutionAlias
  * windows.comServer
  * windows.comInterface
  * windows.posPaymentConnector
  * windows.dataProtection

> [!NOTE]
> 由于带有桌面体验的 Windows Server 2019 不支持 Windows 10 客户端 SKU 的部分功能，因此建议先在 Windows Server 2019 上测试应用程序，再在生产环境中部署它。