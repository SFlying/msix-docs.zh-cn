---
title: 使用修改包生成应用
description: 本文介绍了当应用打包为 MSIX 时，修改包将如何用于它们的应用
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 91eacb13777804c63c62a0b3eedd9c39d1e67530
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77073983"
---
# <a name="building-an-app-with-a-modification-package"></a>使用修改包生成应用 
使用修改包，IT 专业人员可以自定义其 MSIX 应用，而无需重新打包应用。 这意味着，如果 IT 专业人员部署主应用和修改包并且用户运行该应用，则修改包中包含的自定义项将覆盖在主应用上面，对用户而言它就像一个应用。 这是因为修改包在主应用的 MSIX 容器内运行，只能通过主应用进行访问。 修改包示例有插件/加载项、文件和注册表项。 这是在开发应用程序时需要注意的事项，尤其是对于企业而言。 

有了主应用和修改包模型，作为开发人员，你可以在不重新打包的情况下进行更新，因而你的企业客户可以获得最新最好的体验。 这是因为自定义项都存储在可以独立部署和维护的修改包中。 

