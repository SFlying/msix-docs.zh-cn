---
title: 应用安装程序文件概述
description: 了解应用安装程序文件的内容及其工作原理。
author: mcleanbyron
ms.author: mcleans
ms.date: 12/12/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 34143433ba07c68c86c394466f9be7dc75632194
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "65795258"
---
# <a name="app-installer-file-overview"></a>应用安装程序文件概述

你经常需要与许多用户共享应用。 后来你需要更新应用，并希望确保即使是对于非技术用户，也能够轻松顺利地完成更新。

为帮助你实现此目的，我们引入了应用安装程序文件。 这是一个可由你自行创建的或者使用 Visual Studio 创建的 XML 文件（请参阅[此处](create-appinstallerfile-vs.md)的 Visual Studio 说明）。 应用安装程序文件指定应用所在的位置及其更新方式。 如果选择使用此应用分发方法，则必须与用户共享应用安装程序文件，而不是实际的应用容器。 然后，用户必须单击应用安装程序文件。 此时会显示用户所熟悉的应用安装程序 UI，并引导用户完成安装。  用户使用这些步骤安装应用程序后，该应用程序将与应用安装程序文件相关联。  

以后需要更新应用程序时，只需更新应用安装程序文件 (.appinstaller) 即可。 更新该文件时，应用程序的新版本将推送给用户。 此方法尤其适合用户，因为他们无需执行任何操作即可获得更新。 他们只需像平时一样使用应用程序，而更新会自动提供给他们。

以下示例演示了此方法的工作原理：

1. IT 专业人员 Joe 想要在企业中分发人力资源应用。
2. IT 专业人员 Joe 将该人力资源应用放在共享中，并创建名为 HumanResources.appinstaller 的应用安装程序文件。 此应用安装程序文件与该应用相关联。
3. IT 专业人员 Joe 将 HumanResources.appinstaller 放在共享中。
4. IT 专业人员 Joe 将企业员工指向 HumanResources.appinstaller。
5. 经理 Maggie 单击 HumanResources.appinstaller 并看到应用安装程序的 UI，其中会引导她安装该人力资源应用程序。
6. 从此时起，在经理 Maggie 的设备上，该人力资源应用不过是另一个应用，她可以像操作任何其他应用时一样与该应用交互。 她可以将该应用固定到任务栏或开始菜单，它也会显示在应用列表中，等等。
7. 一周后，IT 专业人员 Joe 获得了人力资源应用的更新。 若要与用户共享该应用，他只需更新 HumanResources.appinstaller 以指向新的应用版本，并设置所需的更新类型。
8. 第二天上午，不知道有应用更新的经理 Maggie 启动了桌面上的人力资源应用程序。
9. 该应用程序检测到有可用的更新，并自动应用更新
10. 经理 Maggie 很高兴她现在获得了最新版本的应用程序，并可以利用新的功能。

从 Windows 10 Fall Creators Update（版本 1709，内部版本 16299）和更高版本开始，Windows SDK 还提供多个 API 让你以编程方式通过应用安装程序文件修改包，或者使用应用安装程序关联检索有关应用的信息。 有关详细信息，请参阅[相关文档](app-installer-documentation.md)。

## <a name="contents-of-the-app-installer-file"></a>应用安装程序文件的内容

下图显示了一个示例应用安装程序文件。 有关应用安装程序文件中 XML 元素的完整详细信息，请参阅[应用安装程序文件架构参考](https://docs.microsoft.com/uwp/schemas/appinstallerschema/schema-root)。 有关如何在应用安装程序文件中配置更新设置的详细信息，请参阅[在应用安装程序文件中配置更新设置](update-settings.md)。

![应用安装程序文件和更新设置示例](images/App-Installer-File-Update.png)

## <a name="related-topics"></a>相关主题

* [使用 Visual Studio 创建应用安装程序文件](create-appinstallerfile-vs.md)
* [手动创建应用安装程序文件](how-to-create-appinstaller-file.md)
* [在应用安装程序文件中配置更新设置](update-settings.md)
