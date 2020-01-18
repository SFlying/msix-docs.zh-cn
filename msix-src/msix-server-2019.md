---
title: 服务器2019上的 .MSIX 支持
description: 本文详细介绍了如何在服务器2019上支持 .MSIX
ms.date: 01/16/2020
ms.topic: article
author: dianmsft
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d5da083602173560a9fa481f138e108e851d84fc
ms.sourcegitcommit: e1da85c80d639646f9c9dd7ab26e7e4bffd1c9dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76248419"
---
# <a name="msix-support-on-windows-server-2019"></a>Windows Server 2019 上的 .MSIX 支持

Windows Server 2019 （LTSC）上现在支持 .MSIX 的桌面体验。 此支持使你能够在客户端和服务器 Sku 上分发同一 .MSIX 包，通过**PowerShell**安装或通过[程序包管理器 API](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager)直接安装。

## <a name="considerations"></a>注意事项

* Windows Server 2019 不支持 Microsoft Store 和业务 Microsoft Store。 所有应用程序都必须使用**PowerShell**安装。
* 由于 Windows Server 2019 基于 Windows 10 版本1809，因此它支持具有最低目标版本**10.0.17763**或更低版本的应用程序。
* 以下 Api 和框架在 Windows Server 2019 上不可用：
  * Microsoft Advertising
  * 地理位置
  * Cortana 搜索
  * 应用内购买
* 以下框架未安装在 Windows Server 2019 上。 你有责任确保它们在需要时与你的应用程序一起安装。
  * .NET 运行时
  * .NET Framework
  * VCLibs
* Windows Server 2019 不支持以下包扩展：
  * FirewallRules
  * AppExecutionAlias
  * ComServer
  * ComInterface
  * PosPaymentConnector
  * dataProtection

> [!NOTE]
> 由于带有桌面体验的 Windows Server 2019 不支持 Windows 10 客户端 Sku 的所有功能，因此建议在生产环境中部署之前，在 Windows Server 2019 上测试应用程序。
