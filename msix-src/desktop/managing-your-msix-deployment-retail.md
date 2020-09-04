---
description: 本文提供了在零售环境中管理 MSIX 应用程序部署所需的所有详细信息。  本文的目标读者是开发人员。
title: 在使用者环境中分发 MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 284023d31ef249e515afb42a2675f28bdacf875f
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090476"
---
# <a name="distribute-your-msix-in-a-consumer-environment"></a>在使用者环境中分发 MSIX

如果你不是企业开发人员，可以通过正常的零售渠道完成 MSIX 的零售分发。  这包括在网页上托管 MSIX。  

## <a name="microsoft-store"></a>Microsoft Store

[Microsoft App Store](https://www.microsoft.com/store/apps/windows) 可能是分发你的应用程序的极好方式。  自 2012 年底 Windows 8 发布以来，它就已经向消费者开放。 用户可以从中浏览、搜索和下载适用于 Windows 的应用或游戏。

有关 Microsoft App Store 及其功能的详细信息，请参阅 [Microsoft App Store](/windows/uwp/publish/)。 

若要开始将应用发布到 Microsoft App Store 并了解其功能，请参阅[合作伙伴中心](https://partner.microsoft.com/dashboard/home)

## <a name="app-center"></a>应用中心

利用[应用中心](https://appcenter.ms/)可以自动生成应用，在真实设备上测试该应用，以及将其分发给 beta 测试人员。  使用应用中心可以更频繁、以更高的质量且更自信地交付应用。  使用应用中心可以连接存储库，在几分钟内自动化生成，在云中的真实设备上进行测试，将应用分发给 beta 测试人员，以及通过崩溃和分析数据监视现实使用情况。 一站式服务。
有关应用中心的详细信息，请参阅[应用中心](/appcenter/)。

## <a name="web-install"></a>Web 安装

虽然 MSIX 文件可以承载在任何 IIS 服务器上，  但通过使用应用安装程序启用直接安装，你可以显著改善体验。  直接安装将允许 AppInstaller 立即启动并从 Web 服务器运行安装，而不是先下载应用。  有关 Web 安装的更多详细信息，请参阅[从网页安装 Windows 10 应用](../app-installer/installing-windows10-apps-web.md)。

