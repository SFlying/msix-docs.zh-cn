---
Description: 本文提供了在零售环境中管理 MSIX 应用程序部署所需的所有详细信息。  本文的目标读者是开发人员。
title: 在消费者环境中分发 MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 3a27a4804786ce82485ba3e7de9c855a2bf2e23c
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073853"
---
# <a name="distribute-your-msix-in-a-consumer-environment"></a>在消费者环境中分发 MSIX

如果你不是企业开发人员，可以通过正常的零售渠道完成 MSIX 的零售分发。  这包括在网页上托管 MSIX。  

## <a name="microsoft-store"></a>Microsoft Store

[Microsoft App Store](https://www.microsoft.com/store/apps/windows) 可能是分发你的应用程序的极好方式。  自 2012 年底 Windows 8 发布以来，它就已经向消费者开放。 用户可以从中浏览、搜索和下载适用于 Windows 的应用或游戏。

有关 Microsoft App Store 及其功能的详细信息，请参阅 [Microsoft App Store](https://docs.microsoft.com/windows/uwp/publish/)。 

若要开始将应用发布到 Microsoft App Store 并了解其功能，请参阅[合作伙伴中心](https://partner.microsoft.com/dashboard/home)

## <a name="app-center"></a>应用中心

利用[应用中心](https://appcenter.ms/)，你可以自动生成应用程序，在实际设备上测试它，以及将其分发给 beta 测试人员。  利用应用中心，你可以更频繁、更高质量且更有信心地交付应用。  利用应用中心，你可以连接你的存储库，在几分钟内自动化你的生成，在云中的真实设备上进行测试，将应用分发给 beta 测试人员，以及通过崩溃和分析数据监视真实的使用情况。 一站式服务。
有关应用中心的详细信息，请参阅[应用中心](https://docs.microsoft.com/appcenter/)。

## <a name="web-install"></a>Web 安装

虽然 MSIX 文件可以承载在任何 IIS 服务器上，  但通过使用应用安装程序启用直接安装，你可以显著改善体验。  直接安装将允许 AppInstaller 立即启动并从 Web 服务器运行安装，而不是先下载应用。  有关 Web 安装的更多详细信息，请参阅[从网页安装 Windows 10 应用](https://docs.microsoft.com/windows/msix/app-installer/installing-windows10-apps-web)。

 
