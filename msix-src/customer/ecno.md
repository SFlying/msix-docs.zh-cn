---
title: ECNO
description: ECNO 介绍了 .MSIX 的体验
author: dianmsft
ms.date: 06/25/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1c79c75febccd9e38d60949c6dadcbde78e0cee2
ms.sourcegitcommit: fe85d57b3c9478de5fd5347010d2dee1e73f2268
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2020
ms.locfileid: "85549082"
---
# <a name="ecno"></a>ECNO

![ECNO 徽标](../images/ECNO_masterlogo.png)

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
