---
title: 部署预装的应用
description: 本文提供预装应用的概述
ms.date: 07/02/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, msix, uwp, 可选包, 相关集, 包扩展, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: a743e8e4f960bc50e904b133da07e6fb3feab181
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074233"
---
# <a name="deploying-preinstalled-apps"></a>部署预装的应用 
本文概述预装应用的工作原理，以及预配和许可过程如何使用预装的应用。 

## <a name="how-preinstalled-apps-work"></a>预装应用的工作原理 

应用安装过程可划分为两个步骤： 
1. 分步 
2. 注册 

当平台暂存某个应用时，会将应用包中的所有文件放入文件系统，然后设置相应的 ACL。 暂存后，可为当前用户注册该应用。 平台在注册步骤中完成所有设置操作，使应用可供用户使用并与操作系统集成。这些操作包括创建用户特定的应用数据位置、启用应用扩展点（例如文件类型关联）的发现，以及创建应用磁贴。 此外还会执行其他许多有意义的操作，但最重要的一点是，注册绝对需要按用户进行，而暂存应用只需完成一次，且无需任何用户的参与即可完成。 平台只需为任意数量的用户暂存应用一次，但每个用户必须登录以注册应用。

若要为某个用户预装应用，平台仍需执行这两个步骤：暂存和注册。 暂存步骤足够简单；随时可以完成此步骤，甚至在电脑远远还未生产出来时，就能在 Windows 映像中脱机完成暂存。 但注册要求用户登录，使用新电脑或新用户帐户时，只有在用户首次登录之后，才能执行注册。 若要完成预装，平台必须在用户登录后代表用户注册应用。 平台使用某个系统服务完成此步骤，该服务了解所有需要注册的预装应用，然后在用户首次登录时自动触发注册。 平台将此服务称作“应用就绪服务”(ARS)。

换言之，预装应用的工作原理是将应用包暂存到磁盘，然后将其配置为在用户下次登录时自动为每个用户注册。

## <a name="provisioning-apps"></a>预配应用 
所有应用预配封装在 DISM 工具中，会同时执行暂存和 ARS 设置。 若要执行预配，IT 专业人员需要一个应用包（.msix、.msixbundle、.appx 或 .appxbundle）和任何依赖项包。 

相关 PowerShell 命令的列表
* **/Get-ProvisionedAppxPackages** 此命令列出映像中预装的所有应用的列表。
* **/Add-ProvisionedAppxPackage** 此命令暂存 appx 包，并对其进行配置，以便可以预装。 还必须提供所有依赖项，可以在 SDK 或者从 Store 下载的包中找到这些依赖项。
* **/Remove-ProvisionedAppxPackage** 此命令可用于删除预装的应用。 请注意，如果已经为任何用户注册了应用，则此命令不会删除该应用 - 只会去掉自动注册行为，因此不会为任何新用户自动安装该应用。  如果没用任何用户安装该应用，则此命令还会删除暂存的文件。

## <a name="licensing"></a>许可
仅当预配 Windows Store 应用时，许可才适用。 任何其他应用无需许可证即可预配。 如果应用来自于 Store，则预配该应用时，还必须提供机器许可证。 目前，所有预装的 Windows Store 应用必须是免费应用，并配置为可通过 Windows Store 合作伙伴中心预装。 配置后，可以下载可预装的包和许可证，然后将其预配到任何映像中。

