---
Description: 有关基于源代码创建 MSIX 程序包的主题概述
title: 基于代码生成 MSIX 程序包概述
author: Huios
ms.date: 02/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 34308aefcd0417bc4461c69f3b3459b125a8bf8c
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091215"
---
# <a name="building-an-msix-package-from-your-code"></a>基于代码生成 MSIX 程序包 

如果你的桌面应用程序当前正在开发中，建议在生成环境中生成 MSIX 程序包，而不是生成安装程序并通过 MSIX 打包工具运行它。 在 Visual Studio 2017 版本 15.5 及更高版本（包括 Visual Studio 2019）中，可以使用 Windows 应用程序打包项目为应用程序生成 MSIX。 如果不是在 Visual Studio 中进行开发，则可以将 MSIX 命令行工具集成到你的生成系统，以将应用程序二进制文件打包为 MSIX。

如果是开发 UWP 应用程序，则 Visual Studio 默认会将 MSIX 用作应用程序的打包格式。

|主题| 说明 |
|:---|:---|
|[在打包桌面应用之前需要了解的内容](before-packaging-overview.md)| 有关 MSIX 要求和打包的桌面应用运行时行为的背景知识。 在为桌面应用程序生成 MSIX 程序包之前了解这些知识非常有用。 如果是生成 UWP 应用，则可以跳过此部分。 | 
|[在 Visual Studio 中打包桌面或 UWP 应用](vs-package-overview.md)| 本部分介绍了如何在 Visual Studio 中将桌面（Windows 窗体、WPF、Win32，等等）应用或 UWP 应用打包为 MSIX。|
|[用于生成和部署 MSIX 的 CI/CD 管道](azure-dev-ops.md)| 本部分介绍了如何在 Azure DevOps 中使用 CI/CD 管道将生成和部署工作流自动化。|
|[从命令行打包](../package/manual-packaging-root.md)| 本部分讨论了如何使用命令行工具将应用打包为 MSIX。|
|[扩展 MSIX 应用程序](extend-overview.md)| 本部分讨论了如何使用扩展和可选程序包来扩展应用程序。|

## <a name="add-modern-windows-10-experiences"></a>添加新式 Windows 10 体验

为桌面应用创建 MSIX 包后，可以使用 UWP API、包扩展和 UWP 组件来为引入注目的新式 Windows 10 体验增加亮点（例如动态磁贴和通知）。

### <a name="enhance-with-uwp-apis"></a>使用 UWP API 进行增强

将应用打包后，可以通过添加动态磁贴和推送通知等功能来为其增加亮点。 其中某些功能可以显著提高应用程序的参与度，并且只需花费很少的时间即可添加。 某些增强功能需要更多的代码。

请参阅[在桌面应用程序中使用 UWP API](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)。

### <a name="integrate-with-package-extensions"></a>集成包扩展

如果应用程序需要与系统集成（例如，建立防火墙规则），请在应用程序的包清单中描述集成任务，系统将完成其余操作。 对于其中的大多数任务，根本不必编写任何代码。 在清单中添加少量的 XML 后，可以执行一些操作，例如，在用户登录时启动进程、将应用程序集成到文件资源管理器中，以及为应用程序添加显示在其他应用中的打印目标列表。

请参阅[将桌面应用程序与包扩展集成](/windows/apps/desktop/modernize/desktop-to-uwp-extensions)。

### <a name="extend-with-uwp-components"></a>使用 UWP 组件进行扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 一般情况下，首先应确定是否可以通过 UWP API [增强](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)现有桌面应用程序来增加你的体验。 如果你必须使用 UWP 组件来可实现此体验，则可以将 UWP 项目添加到你的解决方案中，并使用应用服务在桌面应用程序和 UWP 组件之间进行通信。

请参阅[使用 UWP 组件扩展桌面应用程序](/windows/apps/desktop/modernize/desktop-to-uwp-extend)。