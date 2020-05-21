---
title: 客户旅程
description: 本文介绍 .MSIX 客户旅程。
author: dianmsft
ms.date: 05/12/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: aa87e92294a970f82f1653cb1510c7eddf371dc2
ms.sourcegitcommit: 8b02a0d376ab16873607ed84d1560b1c69c9e915
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83632615"
---
# <a name="customer-journeys-with-msix"></a>具有 .MSIX 的客户旅程

许多客户都成功使用 .MSIX 来改善其 Windows 应用程序的部署。 本文重点介绍了 .MSIX 的多个客户旅程。

* [了解 DB Systel 如何实现 Windows 应用程序的敏捷部署](customer-journey.md#db-systel)
* [了解安大略省（ECNO）教育计算网络如何提高为安大略学生提供的桌面应用程序交付和安装体验](customer-journey.md#ecno)
* [了解 Schneider 电力公司如何开发和部署 WPF 桌面应用程序](customer-journey.md#schneider-electric)

## <a name="db-systel"></a>DB Systel

![DB Systel 徽标](images/DB_logo_red_outlined_200px_rgb.png)

DB Systel GmbH （总部在法兰克福 am Main 中）是数据库 AG 的完全拥有的子公司和所有集团公司的数字合作伙伴。 德国 Bahn AG 是世界上第二大的传输公司，是欧洲最大的铁路运营商和基础结构所有者。 它运行了德国铁路的大型部分，每年约2000000000乘客。

DB Systel 员工围绕4600人，他们运营600系列业务应用程序，100000 PC 工作站、93000 VoIP Pbx 和200000移动设备等。它们处理公司的所有 IT 基础结构，从传统的 IT 服务到开发所有用于控制铁路系统的各个方面的内部应用程序。 

对于 DB Systel，桌面应用程序是基础结构的关键组件。 它们是许多关键任务的主要界面，从管理员工到确保铁路系统的正常运行。 DB Systel 开发、维护和部署总共600个胖客户端桌面应用程序和大约200个 Java 应用程序。

当涉及到桌面应用程序时，他们面临着一些涉及以下主题的挑战：

* 许多服务器端应用程序都是通过生成管道生成、测试和提供的，它们使用高度自动化的过程（DevOps）几次。 然而，当前的部署技术目前无法实现与 Windows 桌面应用程序相同的目标。
* 许多团队涉及到几天延迟的开发和部署过程，用户可能会获得最新版本的软件。
* 旧的软件部署过程非常耗时、冗长且昂贵。
* 许多业务应用程序都基于 Java Web 启动技术，该技术已不再使用。

由于这些挑战，DB Systel 只能提供短期的更新，并提供大量的精力。 这就成为了严重问题，因为许多应用程序都依赖于后端中的特定软件版本。 在后端软件更新之后直接更新用户的客户端软件非常重要。 如果不是这种情况，用户使用所述软件的能力将不再得到保证，并且可能会导致铁路服务中断。

数据库 Systel 在开始研究如何取代 Java Web 启动技术时，先听说 .MSIX。 .MSIX 是一种建议的方法，因为它会使他们能够创建独立的应用程序，这些应用程序不依赖于所安装 Java Runtime Environment。 这会使团队耗时协调和同步工作，并导致更稳定的操作。 当他们开始尝试 .MSIX 时，他们很快就会认识到，它不只是为了支持 Java Web 启动迁移，而且还能解决它们在打包和分发方面的最大难题。

.MSIX enabled DB Systel to：

* 简化软件包的传统打包和部署。
* 使软件开发人员拥有构建和部署软件的整个端到端过程，而不是将打包和分发过程委托给特殊团队。
* 通过管道实现现有手动过程的自动化。
* 在 Windows 桌面应用程序部署中实现速度和简洁性，这将通过新的自助服务方法显著节省成本。

*"过去，我们会在此过程中涉及许多团队，并在达到应用程序管理员可以使用和更新软件的点之前花了时间。因此，我们只会努力将发布（更新）分发给我们的客户。在与 Microsoft 专家一起使用非常丰富的成效 .MSIX 研讨会后，我们确信我们可以使用 .MSIX 自助服务在 DB Systel 上革新软件预配过程。在速度和简易性方面，.MSIX 提供了与容器格式更大的优势。应用程序管理员可以使用 .MSIX 将软件打包，并通过我们的商店提供其软件。 "*
-在数据库的新式部署团队中 Markus Thomann，软件顾问

DB 系统将 .MSIX 作为容器格式集成到生成过程中。 其中的大多数应用程序（包括许多任务关键型应用程序）将移植到 .MSIX 格式。 这会使软件预配过程更简单、更快、更便宜。 由于 .MSIX 和新式部署团队，应用程序管理员现在可以直接提供最终用户软件更新，也可以在一天中多次提供。

*"借助 .MSIX 技术，我们可以采用 DevOps 的方法，即使我们提供客户端软件，而不是云软件。这是 inconceivable 的。 "* -在数据库的新式部署团队中 Markus Thomann，软件顾问

## <a name="ecno"></a>ECNO

![ECNO 徽标](images/ECNO_masterlogo.png)

安大略省（ECNO）教育计算网络是一家组织，它协作查找并执行适用于安大略 72 K-12 school 板（2000000学生）的有效 IT 解决方案。 ECNO 负责保持最新的计算技术，因为它们与省市自治区的教育目标相关。 它们将为其提供基础结构建议，并与成员学校面板共享最佳实践。

School 板是自治板，可免费实现其自己的 IT 策略，最适合它们。 例如，一个 school 板使用 Configuration Manager 专用于分发应用程序，而另一个则仅将其用于计算机映像和少量的应用。

在 ECNO 的涵盖范围内，共享的技术服务项目团队评估、测试和打包450第三方应用的应用目录，以满足 K –12名学生和员工的需求。

到目前为止，ECNO STS 团队已通过 .MSIX 打包工具和 .MSIX 工具箱中包含的批量转换工具转换了其大部分应用。

ECNO 当前在合作伙伴中心包含170个应用，其中包含链接到其 LOB 帐户的十个学校板。 目前，有两种方式允许通过 "自助服务" 访问应用商店中的应用。 第三方已 Configuration Manager 链接到其商店，并希望以这种方式添加对应用程序的访问权限。 还有六个公司希望通过 Intune 进入软件管理/分配，并请求使用一些示例应用。 另一 school 委员会正在完成项目以将其 Ipad 作为 MDM 工具移动到 Intune，然后将 Windows 设备移到 Intune （其中，他们正在使用 .MSIX，而商店是其软件分发模型）。 他们希望更多的主板在移动时开始切换，以采用 Intune/Configuration Manager。

*"这是一项直接的指导，在 COVID-19 期间，我们将向目前无法在学校网络上的用户提供软件。多年来，AppV 包是 boon 的，使用 .MSIX 进行的下一次迭代将允许我们的员工和学生使用其所需的软件，这些软件需要使用从世界上的任何 internet 连接进行新式和无缝的交付系统。 "*
-Jason David Flannery，Systems 工程师，Simcoe 县地区学校

使用 autopilot 将设备注册到 Windows Intune 后，可以轻松地配置和轻松部署。

利用 .MSIX，ECNO 的好处是什么？

* 作为安大略学校的软件部署的领导者，.MSIX 提供了来自 App-v 进程的无缝转换。
* 允许我们的学校板保持最新的新式软件部署工具和终结点管理。
* 对于我们的项目团队，它提高了工作效率，并为我们的学校板提供了更简单的部署和支持。

*"由于 IT 团队正在努力实现更高的灵活性，尤其是在我们的距离学习的情况下，我们将越来越多地依赖云。 我们的组织的下一次逻辑发展是云管理。我们希望我们的基于学校的 IT 人员更详细地关注技术的成功实现，而不是部署和故障排除。随着我们的设备和应用程序的零接触部署，我们将与 ECNO 的 STS 团队密切合作，将我们所有使用的应用程序集成到云托管的方法中。 "*
-小红 Heuchert，信息管理器，Peterborough 维多利亚湖区 Clarington 天主教地区学校板

## <a name="schneider-electric"></a>施耐德电气

![施耐德电气](images/Logo_SE_Green_RGB-Screen.png)

Schneider 电气开发 GIS 软件，以支持美国和国际范围内的小型和大型公用事业（电气、天然气、水、纤维、同轴电缆）。

在使用 Microsoft 时，我们希望将 WPF 应用程序引入最新的安装程序技术 .MSIX。 我们有特定的客户端驱动用例;我们需要经常作为开发运营相投软件公司发布和部署，但每个公用事业公司希望管理更改，并单独确定将向其用户发布的应用程序的版本和时间。 我们已与 Microsoft 密切合作以满足此客户需求。 完成升级到 .MSIX 后，我们将能够删除一些第三方依赖项，并且更接近将应用程序更新到 .NET Core 3.0 的步骤。

通过利用 .MSIX，我们将简化开发堆栈。 我们将使用代码（c #/WPF）中的 Microsoft 技术生成（Azure Dev Ops）到安装程序（.MSIX）。 .MSIX 的主要好处之一是，开发人员可以直接从 Visual Studio 开发、调试和测试安装程序。 这极大地简化了安装特定的故障排除，并使我们能够与 Microsoft 密切合作。

*"随着我们迁移到 .MSIX，Microsoft 的支持非常宝贵。我们有一个独特的用例，Microsoft 已帮助我们在更新应用程序时导航到我们遇到的任何障碍。开发人员都很高兴将我们的应用程序迁移到 .MSIX。 "* – Darlene Rouleau from Schneider 电力公司
