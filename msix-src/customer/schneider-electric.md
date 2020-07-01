---
title: 施耐德电气
description: Schneider 电气介绍了 .MSIX 的体验
author: dianmsft
ms.date: 06/25/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8c095897ea6a8b58c32e265a26066305ec5e4a05
ms.sourcegitcommit: fe85d57b3c9478de5fd5347010d2dee1e73f2268
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2020
ms.locfileid: "85549085"
---
# <a name="schneider-electric"></a>施耐德电气

![施耐德电气](../images/Logo_SE_Green_RGB-Screen.png)

Schneider 电气开发 GIS 软件，以支持美国和国际范围内的小型和大型公用事业（电气、天然气、水、纤维、同轴电缆）。

在使用 Microsoft 时，我们希望将 WPF 应用程序引入最新的安装程序技术 .MSIX。 我们有特定的客户端驱动用例;我们需要经常作为开发运营相投软件公司发布和部署，但每个公用事业公司希望管理更改，并单独确定将向其用户发布的应用程序的版本和时间。 我们已与 Microsoft 密切合作以满足此客户需求。 完成升级到 .MSIX 后，我们将能够删除一些第三方依赖项，并且更接近将应用程序更新到 .NET Core 3.0 的步骤。

通过利用 .MSIX，我们将简化开发堆栈。 我们将使用代码（c #/WPF）中的 Microsoft 技术生成（Azure Dev Ops）到安装程序（.MSIX）。 .MSIX 的主要好处之一是，开发人员可以直接从 Visual Studio 开发、调试和测试安装程序。 这极大地简化了安装特定的故障排除，并使我们能够与 Microsoft 密切合作。

*"随着我们迁移到 .MSIX，Microsoft 的支持非常宝贵。我们有一个独特的用例，Microsoft 已帮助我们在更新应用程序时导航到我们遇到的任何障碍。开发人员都很高兴将我们的应用程序迁移到 .MSIX。 "* – Darlene Rouleau from Schneider 电力公司
