---
title: 应用安装程序文件概述
description: 了解有关应用安装程序文件和它们的工作原理的内容。
author: mcleanbyron
ms.author: mcleans
ms.date: 12/12/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 34143433ba07c68c86c394466f9be7dc75632194
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795258"
---
# <a name="app-installer-file-overview"></a>应用安装程序文件概述

通常情况下，需要使用多个用户共享你的应用。 需要更新应用程序的更高版本，并且你想要确保可以执行的操作是为非技术用户，即使无缝且轻松的方式。

为了帮助您实现此目的，我们引入了应用安装程序文件。 这是您可以自己创建或使用 Visual Studio 创建一个 XML 文件 (请参阅 Visual Studio 说明[此处](create-appinstallerfile-vs.md))。 应用安装程序文件指定您的应用程序所在的位置以及如何更新它。 如果您选择使用这样的应用分发方法，您必须与用户共享的应用安装程序文件，而不是实际的应用程序容器。 然后，用户必须单击应用安装程序文件。 此时熟悉的应用程序安装程序 UI 将出现，并引导用户完成安装。  用户安装后使用这些步骤的应用程序，该应用程序是与应用安装程序文件相关联。  

更高版本，对应用程序的更新后，你只能更新应用安装程序 (.appinstaller) 文件。 更新文件，该应用程序的新版本会推送到用户。 这是尤其适用于你的用户，因为它们无需执行任何操作来获取更新。 它们只继续使用该应用程序像往常一样，并更新将直接发送到它们。

下面是一个示例，演示这的工作原理：

1. IT 专业人员 Joe 想要分发到其企业人力资源应用。
2. IT 专业人员 Joe 将人力资源应用程序放在共享上，并创建一个名为 HumanResources.appinstaller 的应用安装程序文件。 此应用安装程序文件是与应用关联。
3. IT 专业人员 Joe 将 HumanResources.appinstaller 放在共享上。
4. IT 专业人员 Joe 点 HumanResources.appinstaller 到企业的员工。
5. 管理器 Maggie 单击 HumanResources.appinstaller，获取应用程序安装程序 UI，指导她安装人力资源应用程序。
6. 从该点管理器上 Maggie 的设备人力资源是只是另一个应用程序和她与之交互，她与任何其他应用。 她可以将其固定到任务栏或开始菜单，它会显示在她的应用列表等等。
7. 一周后 IT 专业人员 Joe 获取人力资源应用的更新。 若要与用户共享，他只需更新 HumanResources.appinstaller 以指向新的应用程序版本，并设置他想要的更新类型。
8. 在下一步的早上，Manager Maggie，不会知道有关更新的任何启动已在她台式机的人力资源应用程序。
9. 应用程序检测到认为那里更新，并自动应用更新
10. 管理器 Maggie 很高兴她现在具有最新版本的应用程序和可以充分利用新功能。

从 Windows 10 Fall Creators Update （版本 1709，内部版本 16299） 和更高版本，Windows SDK 还提供了几个 Api，可以使用以编程方式修改通过应用安装程序文件的包，或若要检索有关与应用的应用的信息安装程序关联。 有关详细信息，请参阅[相关文档](app-installer-documentation.md)。

## <a name="contents-of-the-app-installer-file"></a>应用安装程序文件的内容

下图显示了示例应用安装程序文件。 有关应用安装程序文件中的 XML 元素的完整详细信息，请参阅[应用程序安装程序文件架构参考](https://docs.microsoft.com/uwp/schemas/appinstallerschema/schema-root)。 有关如何在应用安装程序文件中配置更新设置的详细信息，请参阅[中的应用安装程序文件的配置更新设置](update-settings.md)。

![使用更新设置的应用安装程序文件示例](images/App-Installer-File-Update.png)

## <a name="related-topics"></a>相关主题

* [使用 Visual Studio 创建的应用安装程序文件](create-appinstallerfile-vs.md)
* [手动创建的应用安装程序文件](how-to-create-appinstaller-file.md)
* [在应用安装程序文件中配置更新设置](update-settings.md)
