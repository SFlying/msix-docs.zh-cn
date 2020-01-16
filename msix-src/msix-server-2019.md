---
title: 服务器2019上的 .MSIX 支持
description: 本文详细介绍了如何在服务器2019上支持 .MSIX
ms.date: 04/12/2019
ms.topic: article
author: dianmsft
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 423c58ab10a3e30558b9076c0d2a6c2e1ded9107
ms.sourcegitcommit: 0bd5ebc32feba8a4f4669f232129a8953d5cf773
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030173"
---
# <a name="msix-support-on-server-2019"></a>服务器2019上的 .MSIX 支持

Windows Server 2019 （LTSC）版本中现在支持 .MSIX，并且具有桌面体验。 Windows Server 2019 基于 Windows 10 1809，其中大部分 .MSIX 功能可用，你将能够利用这些功能。
 
此支持允许你在客户端和服务器 Sku 上的企业中分发相同的 .MSIX 包，通过**PowerShell**安装或通过**程序包管理器 api**直接安装。 
 
## <a name="considerations"></a>注意事项
1. Windows server 不支持 Microsoft Store 和业务 Microsoft Store，因此所有应用程序都需要使用**PowerShell**进行安装。
2. 由于 Windows Server 2019 基于 Windows 10 1809，因此它支持具有最低目标版本**10.0.17763**或更低版本的应用程序。
3. 此外，以下 Api 和框架在服务器 SKU 上不可用：
- Microsoft Advertising 框架
- 地理位置，Cortana 搜索
- 应用内购买

4. 以下框架未安装在服务器 sku 上，因此请确保在需要时将其与应用程序一起安装： 
- .NET 运行时
- .NET Framework
- VCLibs.

5. 以下 .MSIX 扩展不受支持： 
- FirewallRules
- AppExecutionAlias
- ComServer
- ComInterface
- PosPaymentConnector
- DataProtection。

> [!NOTE]
> 由于带有桌面体验的 Windows Server 2019 不支持 Windows 10 客户端 sku 的所有功能，因此建议在生产环境中部署之前，在服务器上测试应用程序。
