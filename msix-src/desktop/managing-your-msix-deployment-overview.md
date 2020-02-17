---
Description: 本文提供了在企业和零售环境中管理 MSIX 应用程序部署所需的所有详细信息。  本文的目标读者是企业和 IT 专业人员。
title: 管理 MSIX 部署概述
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 8a1d9cec6226529c0d00cc663c34611abc0d15f3
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074133"
---
# <a name="manage-your-msix-deployment"></a>管理 MSIX 部署

打包你的应用程序只是成功的一半。 接下来，你需要能够为用户部署应用程序。 如何部署应用程序取决于你的客户是谁。  本部分（管理 MSIX 部署）将讨论针对企业和零售市场的 MSIX 程序包部署。 它将提供相关链接和技巧，以确保打造成功的体验。 

为了成功部署 MSIX，你需要考虑以下事项：
* 谁是我的客户？
* 我有哪些依赖项？
* 我将如何为客户提供最佳支持？

## <a name="who-is-my-customer"></a>谁是我的客户？
你如何进行部署通常取决于谁是你的客户以及你作为开发人员或管理员的角色。   确定你的角色以了解要使用哪些工具非常重要。

### <a name="it-pros"></a>IT 专业人员
IT 专业人员通常使用软件管理系统来安装和管理其应用。  在[在企业环境中分发 MSIX](managing-your-msix-deployment-enterprise.md) 部分中，我们将讨论：
* 使用 Microsoft Endpoint Configuration Manager 和 Intune 管理你的应用
* 使用预配和 DISM 为计算机预先配置 MSIX
* 使用组策略和 AppLocker 控制应用访问
* 通过 Web 或适用于企业的 Microsoft Store 分发应用
* 使用 AppCenter 生成、测试和发布桌面应用
 
### <a name="developers"></a>开发人员
开发人员有不同的工具可用来分发应用程序。  在[在零售环境中分发 MSIX](managing-your-msix-deployment-retail.md) 部分中，我们将讨论：  
* Microsoft Store
* Web 安装程序
* 使用 AppCenter 生成、测试和发布桌面应用

## <a name="dependencies"></a>依赖关系
MSIX 是一种强大且可靠的新式应用安装体验。 MSIX 体验的安装成功率为 99.96%。  但有一些注意事项。 默认情况下，并非所有 Windows 版本都支持 MSIX，支持的功能集可能取决于要部署到的 Windows 10 版本。  在[规划部署](managing-your-msix-deployment-targetdevices.md)部分中，我们将讨论了解 MSIX 将部署到的目标设备的重要性。 

## <a name="providing-support-for-my-customer"></a>为我的客户提供支持
尽管 MSIX 的安装成功率为 99.96%，但你仍然需要计划如何为你的客户提供支持。  在 [MSIX 验证和故障排除](managing-your-msix-deployment-overview.md)部分中，我们将讨论可用来诊断安装问题的工具。


 
