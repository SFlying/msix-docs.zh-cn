---
author: mcleanbyron
title: 使用应用安装程序安装 Windows 10 应用
description: 此部分包含或链接至有关应用安装程序及其功能如何使用的文章。
ms.author: mcleans
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 3578d993f2d88cdcfea4781748015ac9ecba509b
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900439"
---
# <a name="install-windows-10-apps-with-app-installer"></a>使用应用安装程序安装 Windows 10 应用

## <a name="purpose"></a>用途
此部分包含或链接至有关应用安装程序及其功能如何使用的文章。

应用安装程序可以通过双击应用包安装的 Windows 10 应用。 这意味着用户无需使用 PowerShell 或其他开发人员工具来部署 Windows 10 应用。 应用安装程序还可以从 Web、可选包和相关的集中安装应用。 若要了解如何使用应用安装程序安装应用，请参阅表中的主题。

| 主题 | 描述 |
|-------|-------------|
| [应用安装程序文件概述](app-installer-file-overview.md) | 了解有关应用安装程序文件和它们的工作原理的内容。 |
| [在 Visual Studio 中创建的应用安装程序文件](create-appinstallerfile-vs.md)| 了解如何使用 Visual Studio 通过 .appinstaller 文件启用自动更新。 |
| [手动创建的应用安装程序文件](how-to-create-appinstaller-file.md)| 了解如何手动创建.appinstaller 文件。 这是用于安装主包和可选包包含一组相关特别有用。 |
| [在应用安装程序文件中配置更新设置](update-settings.md)  |  了解如何使用应用安装程序文件配置应用更新。 |
| [从 web 安装的 Windows 10 应用](installing-windows10-apps-web.md) | 在此部分中，我们将查看允许用户直接从网页中安装应用所需执行的步骤。 |
| [可选包和相关的集](install-related-set.md) | 了解包含主包和相关的可选包的相关集。  |
| [使用应用安装程序文件遇到的安装问题的疑难解答](troubleshoot-appinstaller-issues.md) | 使用应用安装程序文件旁加载应用程序时的常见问题和解决方案。 |
| [相关文档](app-installer-documentation.md) | 提供指向相关文档，包括 Api，可以使用来修改通过应用安装程序文件的包，或若要检索有关与应用安装程序相关联的应用的信息。  |
| [应用安装程序文件 (.appinstaller) 参考](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file?context=/windows/msix/render) | 查看应用安装程序文件的完整 XML 架构。 |

## <a name="tutorials"></a>教程

按照这些教程，了解如何承载和从各种分发平台安装的 Windows 10 应用。 这些教程适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业和开发人员。

| 教程 | 描述 |
|----------|-------------|
| [从 Azure Web 应用中安装 Windows 10 应用](web-install-azure.md) | 创建 Azure Web 应用并使用它来承载和分发 Windows 10 应用包。 |
| [从 IIS 服务器安装的 Windows 10 应用](web-install-IIS.md) | 安装 IIS 服务器，验证 Web 应用可以托管应用包，并有效使用应用安装程序。 |
| [承载 Windows 10 上 AWS 适用于 web 的应用包安装](web-install-aws.md) | 了解如何设置 Amazon 简单存储服务来托管网站上的 Windows 10 应用包。 |