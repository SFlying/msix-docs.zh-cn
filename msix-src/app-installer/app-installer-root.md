---
title: 使用应用安装程序安装 Windows 10 应用
description: 此部分包含或链接至有关应用安装程序及其功能如何使用的文章。
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 042cb23a3a38fe5ab3e6ba37fc77b7b5411b5424
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89089875"
---
# <a name="install-windows-10-apps-with-app-installer"></a>使用应用安装程序安装 Windows 10 应用

## <a name="purpose"></a>目的
此部分包含或链接至有关应用安装程序及其功能如何使用的文章。

利用应用安装程序，可以通过双击应用包来安装 Windows 10 应用。 这意味着用户无需使用 PowerShell 或其他开发人员工具来部署 Windows 10 应用。 应用安装程序还可以从 Web、可选包和相关的集中安装应用。 

可以从适用于企业的 Microsoft Store [Web 门户](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1)下载应用安装程序，以便在企业中离线使用。 可在[此处](/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)详细了解离线分发。

若要了解如何使用应用安装程序安装应用，请参阅表中的主题。

| 主题 | 说明 |
|-------|-------------|
| [应用安装程序文件概述](app-installer-file-overview.md) | 了解应用安装程序文件的内容及其工作原理。 |
| [在 Visual Studio 中创建应用安装程序文件](create-appinstallerfile-vs.md)| 了解如何使用 Visual Studio 通过 .appinstaller 文件启用自动更新。 |
| [手动创建应用安装程序文件](how-to-create-appinstaller-file.md)| 了解如何手动创建 appinstaller 文件。 这对于安装包含主包和可选包的相关集特别有用。 |
| [在应用安装程序文件中配置更新设置](update-settings.md)  |  了解如何使用应用安装程序文件配置应用更新。 |
| [从 web 安装 Windows 10 应用](installing-windows10-apps-web.md) | 在此部分中，我们将查看允许用户直接从网页中安装应用所需执行的步骤。 |
| [可选包和相关集](../package/optional-packages.md) | 了解包含主要包和相关可选包的相关集。  |
| [排查使用应用安装程序文件遇到的安装问题](troubleshoot-appinstaller-issues.md) | 使用应用安装程序文件旁加载应用程序时的常见问题和解决方案。 |
| [相关文档](app-installer-documentation.md) | 提供指向相关文档的链接，包括可用于通过应用安装程序文件修改包的 Api，或用于检索应用程序安装程序关联的应用程序的相关信息。  |
| [应用安装程序文件 (.appinstaller) 参考](/uwp/schemas/appinstallerschema/app-installer-file?context=%252fwindows%252fmsix%252frender) | 查看应用安装程序文件的完整 XML 架构。 |

## <a name="tutorials"></a>教程

遵循这些教程，了解如何从各种分发平台托管和安装 Windows 10 应用。 这些教程适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业和开发人员。

| 教程 | 说明 |
|----------|-------------|
| [从 Azure Web 应用安装 Windows 10 应用](web-install-azure.md) | 创建 Azure Web 应用，并使用它来托管和分发 Windows 10 应用包。 |
| [从 IIS 服务器安装 Windows 10 应用](web-install-IIS.md) | 安装 IIS 服务器，验证 Web 应用可以托管应用包，并有效使用应用安装程序。 |
| [在 AWS 上托管进行 Web 安装的 Windows 10 应用包](web-install-aws.md) | 了解如何设置 Amazon 简单存储服务以托管网站中的 Windows 10 应用包。 |