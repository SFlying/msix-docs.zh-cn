---
description: 本文介绍为现有 Windows 窗体、WPF 或 Win32 应用或游戏创建新式 Windows 10 应用包的端到端过程。
title: 打包桌面应用程序
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fc70cab0074087d7ae2d2ea123ed9b41bfdac8ea
ms.sourcegitcommit: d749fa662214bddaa6854f1ee95761c547db8dae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2019
ms.locfileid: "75008087"
---
# <a name="package-desktop-applications-msix"></a>打包桌面应用程序（.MSIX）

获取现有的桌面应用程序，并为 Windows 10 用户添加新式体验。 然后，将应用发布到 Microsoft Store，以拓展国际市场。 你可以通过利用在应用商店中内置的功能，以更简单的方式盈利你的应用程序。 当然，您不必使用该商店。 你完全可以使用现有的渠道。

当你为桌面应用程序创建 .MSIX 包时，你的应用程序将获得标识并使用该标识，你的桌面应用程序可以访问 Windows 通用平台（UWP） Api。 你可以使用它们添加具有吸引力的现代体验，例如动态磁贴和通知。 仅当应用程序在 Windows 10 上运行时，才使用简单的条件编译和运行时检查来运行 UWP 代码。

除了用于浅 Windows 10 体验的代码，你的应用程序仍保持不变，你可以继续将其分发给以前版本的 Windows 上的用户。 在 Windows 10 上，你的应用程序将继续在完全信任的用户模式下运行，就像现在正在执行的操作一样。

## <a name="prepare"></a>准备

首先，请查看文章[准备打包桌面应用](desktop-to-uwp-prepare.md)，然后解决应用于应用程序的任何问题，然后再为应用程序创建 Windows 应用包。 在创建包之前，您可能无需对应用程序进行许多更改。 但是，在某些情况下，可能需要在为应用程序创建包之前调整应用程序。

<a id="convert" />

## <a name="package"></a>应用包

可以通过多种不同的方式为桌面应用程序创建 .MSIX 包。

### <a name="build-an-msix-from-an-existing-app-installer"></a>从现有应用安装程序构建 .MSIX

如果你已经有一个应用程序包（例如，MSI 或 App-v 安装程序），我们建议你使用[.Msix 打包工具](../mpt-overview.md)将现有桌面应用重新打包为 .msix 格式。 此工具提供交互式 UI 和命令行用于转换，并且可以在没有源代码的情况下转换应用程序。 

以前称为[桌面应用转换器](desktop-to-uwp-run-desktop-app-converter.md)的工具仍可用于重新打包现有的桌面应用程序包。 但是，此工具现在已弃用，我们建议改用 .MSIX 打包工具。

下表显示了这些工具支持的 Windows 10 版本和包格式。

|  工具  | 用于创建包的支持的操作系统版本  | 已安装包的支持的操作系统版本  |  支持的包格式  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  [.MSIX 打包工具](../mpt-overview.md)        |  Windows 10 版本1809及更高版本           | Windows 10 版本 1709 和更高版本            |  。仅 .msix                 |
|  [桌面应用转换器](desktop-to-uwp-run-desktop-app-converter.md)        |  Windows 10 版本 1607 和更高版本           | Windows 10 版本 1607 和更高版本            |  仅 appx    |

若要开始使用 .MSIX 打包工具，请参阅以下文章：

* [通过 VM 上的桌面安装程序（MSI、EXE 或 App-v）创建 .MSIX 包](../packaging-tool/create-app-package-msi-vm.md)
* [使用所有其他安装程序类型创建 .MSIX 包](../packaging-tool/create-other-installer.md)
* [使用命令行创建 .MSIX 包](../packaging-tool/package-conversion-cli.md)

### <a name="build-an-msix-from-source-code-using-visual-studio"></a>使用 Visual Studio 从源代码生成 .MSIX

如果你通过除 Visual Studio 之外的开发环境来维护应用，但没有应用安装程序或安装程序无法执行太多复杂任务，请考虑使用 Visual Studio。

使用 Visual Studio 可以轻松地创建包。 你将向解决方案中添加一个**Windows 应用程序包项目**，引用桌面项目，然后按 F5 调试你的应用程序。 以下是你可以利用它执行的一些其他操作。

:heavy_check_mark: 自动生成可视资源。

:heavy_check_mark: 使用可视设计器对清单进行更改。

:heavy_check_mark: 使用向导生成包。

： heavy_check_mark：从你在[合作伙伴中心](https://partner.microsoft.com/dashboard)中已保留的名称轻松将标识分配给你的应用程序。

有关说明，请参阅[使用 Visual Studio 打包桌面应用程序](desktop-to-uwp-packaging-dot-net.md)。 下表显示了受支持的 Visual Studio、Windows 10 和包格式版本。

|  支持的 Visual Studio 版本 | 用于创建包的支持的操作系统版本  | 已安装包的支持的操作系统版本  |  支持的包格式  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15.5 及更高版本       |  Windows 10 版本 1607 和更高版本           |  Windows 10 版本 1607 和更高版本            |  . .msix （适用于 Windows 10，版本1709及更高版本）<br/>.appx （适用于 Windows 10，版本1607及更高版本）                 |

### <a name="third-party-installers"></a>第三方安装程序

一些受欢迎的第三方产品和安装程序现在支持打包桌面应用程序的功能。 有关这些第三方产品的详细信息和列表，请参阅[使用第三方安装程序打包桌面应用程序](desktop-to-uwp-third-party-installer.md)。

### <a name="manual-packaging"></a>手动打包

作为最后一个选项，你可以在不使用任何这些工具的情况下转换应用程序。 如果想精细地控制你的转换，则可以创建清单文件，然后运行 **MakeAppx.exe** 工具来创建 Windows 应用包。

请参阅[手动打包桌面应用程序](desktop-to-uwp-manual-conversion.md)。

## <a name="add-modern-windows-10-experiences"></a>添加新式 Windows 10 体验

为桌面应用程序创建 .MSIX 包后，可以使用 UWP Api、包扩展和 UWP 组件，将新式和吸引 Windows 10 体验（如动态磁贴和通知）。

### <a name="enhance-with-uwp-apis"></a>利用 UWP Api 增强

将应用打包后，就可以通过添加动态磁贴和推送通知等功能对应用进行完善。 其中的一些功能可以显著提高应用程序的参与度，并可以花费极少的时间来添加应用程序。 某些增强功能需要更多的代码。

请参阅[在桌面应用程序中使用 UWP api](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)。

### <a name="integrate-with-package-extensions"></a>集成包扩展

如果你的应用程序需要与系统集成（例如：建立防火墙规则），请在应用程序的包清单中描述这些内容，系统将执行其余操作。 对于其中大多数任务，你根本不必编写任何代码。 对于清单中的 XML，你可以执行一些操作，例如，在用户登录时启动进程，将应用程序集成到文件资源管理器，然后添加你的应用程序显示在其他应用中的打印目标的列表。

请参阅[将桌面应用程序与包扩展集成](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions)。

### <a name="extend-with-uwp-components"></a>使用 UWP 组件进行扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 一般情况下，首先应确定是否可以通过 UWP API [增强](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)现有桌面应用程序来增加你的体验。 如果必须使用 UWP 组件来实现体验，则可以将 UWP 项目添加到解决方案，并使用应用服务在桌面应用程序和 UWP 组件之间进行通信。

请参阅[利用 UWP 组件扩展桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)。

## <a name="test"></a>测试

若要在准备分发时以真实的设置测试应用程序，最好先对应用程序进行签名，然后再进行安装。 请参阅[测试你的应用](desktop-to-uwp-debug.md#test-your-app)。

>[!IMPORTANT]
> 如果你计划将应用程序发布到 Microsoft Store，请确保应用程序在 S 模式下运行 Windows 10 的设备上正常运行。 这是一个存储要求。 请参阅[测试 Windows 应用是否适用于 S 模式下的 Windows 10](desktop-to-uwp-test-windows-s.md)。

## <a name="validate"></a>验证

若要为应用程序提供在 Microsoft Store 上发布的最大机会或变为[Windows 认证](https://go.microsoft.com/fwlink/p/?LinkID=309666)，请在提交应用进行认证之前，先对其进行验证和测试。

如果使用 DAC 打包应用程序，则可以使用新的 ``-Verify`` 标志根据打包的桌面应用程序和存储要求来验证包。 请参阅[打包应用，对应用进行签名，并准备好应用商店提交](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters)。

如果你使用的是 Visual Studio，则可以从 "**创建应用包**" 向导验证你的应用程序。 请参阅[创建应用包上传文件](../package/packaging-uwp-apps.md#create-an-app-package-upload-file)。

若要手动运行该工具，请参阅 [Windows 应用认证工具包](/windows/uwp/debug-test-perf/windows-app-certification-kit)。

若要查看 Windows 应用认证用于验证应用的测试列表，请参阅 [Windows 桌面桥应用测试](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests)。

## <a name="distribute"></a>分配

可以通过将应用程序发布到 Microsoft Store 来分发应用程序，也可以将其旁加载到其他系统。

请参阅[分发打包的桌面应用](/windows/apps/desktop/modernize/desktop-to-uwp-distribute)。

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
