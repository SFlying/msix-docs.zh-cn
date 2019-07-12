---
Description: 为现有 Windows 窗体、WPF 或 Win32 应用或游戏创建现代 Windows 应用包。 添加新式 Windows 10 用户体验并简化部署和货币化率。
title: 打包桌面应用程序
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 07477630e9fc9987aa609244301d8f6b44bbb0e1
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828961"
---
# <a name="package-desktop-applications-desktop-bridge"></a>打包桌面应用程序 （桌面桥）

需要您现有的桌面应用程序并添加对 Windows 10 用户的新式体验。 然后，将应用发布到 Microsoft Store，以拓展国际市场。 您可以将转化为收益应用程序中多更简单的方法通过利用存储内置的功能。 当然，您无需使用的存储。 你完全可以使用现有的渠道。

桌面应用程序创建包时，应用程序将收到一个标识和桌面应用程序与该标识具有访问权限对 Windows 通用平台 (UWP) Api。 你可以使用它们添加具有吸引力的现代体验，例如动态磁贴和通知。 使用简单的条件编译和运行时检查仅当你的应用程序运行在 Windows 10 上运行 UWP 代码。

除了利用所掌握 Windows 10 体验的代码，你的应用程序保持不变，并且可以继续将其分发到以前版本的 Windows 上的用户。 在 Windows 10 上继续在完全信任环境中运行你的应用程序现在执行用户模式下，就像它一样的操作。

## <a name="prepare"></a>准备

首先，通过本文的审阅准备你的应用程序[准备你的桌面应用打包](desktop-to-uwp-prepare.md)，然后解决任何之前为其创建 Windows 应用包应用到你的应用程序的问题。 可能不需要在创建包之前对应用程序进行很多更改。 但是，有某些情况下，可能会要求您调整你的应用程序之前为其创建包。

<a id="convert" />

## <a name="package"></a>package

有几种不同的方法来创建 MSIX 程序包为桌面应用。

### <a name="build-an-msix-from-an-existing-app-installer"></a>从现有的应用安装程序生成 MSIX

如果已有应用包 （例如，MSI 或 APP-V 安装程序），我们建议你使用[MSIX 打包工具](../mpt-overview.md)重新打包为 MSIX 格式对现有桌面应用程序。 此工具提供交互式 UI 和命令行用于转换，并且可以在没有源代码的情况下转换应用程序。 

名为早期工具[Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)也是仍可用于重新打包现有桌面应用程序包。 但是，此工具现已弃用，并且我们建议您改用 MSIX 打包工具。

下表显示了支持的版本的 Windows 10 和包格式的这些工具。

|  Tool  | 用于创建包支持的操作系统版本  | 支持的操作系统版本的已安装的包  |  支持的包格式  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  [MSIX 打包工具](../mpt-overview.md)        |  Windows 10，版本 1809 及更高版本           | Windows 10 版本 1709 和更高版本            |  仅.msix                 |
|  [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)        |  Windows 10 版本 1607 和更高版本           | Windows 10 版本 1607 和更高版本            |  仅.appx    |

若要开始使用 MSIX 打包工具，请参阅以下文章：

* [在 VM 上创建 MSIX 包从桌面安装程序 （MSI、 EXE 或 APP-V）](../packaging-tool/create-app-package-msi-vm.md)
* [创建 MSIX 包与所有其他安装程序类型](../packaging-tool/create-other-installer.md)
* [创建使用命令行对 MSIX 程序包](../packaging-tool/package-conversion-cli.md)

### <a name="build-an-msix-from-source-code-using-visual-studio"></a>生成从使用 Visual Studio 源代码 MSIX

如果你通过除 Visual Studio 之外的开发环境来维护应用，但没有应用安装程序或安装程序无法执行太多复杂任务，请考虑使用 Visual Studio。

Visual Studio 就可轻松创建包。 你将添加**Windows 应用程序的包项目**到解决方案中，引用桌面项目，然后再按 F5 来调试您的应用程序。 以下是你可以利用它执行的一些其他操作。

:heavy_check_mark:自动生成的视觉资产。

:heavy_check_mark:通过使用可视化设计器对清单进行更改。

:heavy_check_mark:使用向导生成包。

:heavy_check_mark:轻松地从你已在中保留的名称将标识分配到应用程序[合作伙伴中心](https://partner.microsoft.com/dashboard)。

有关说明，请参阅[通过使用 Visual Studio 打包桌面应用程序](desktop-to-uwp-packaging-dot-net.md)。 下表显示了受支持的版本的 Visual Studio、 Windows 10 和包格式。

|  支持的 Visual Studio 版本 | 用于创建包支持的操作系统版本  | 支持的操作系统版本的已安装的包  |  支持的包格式  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15.5年及更高版本       |  Windows 10 版本 1607 和更高版本           |  Windows 10 版本 1607 和更高版本            |  .msix （适用于 Windows 10 版本 1709年及更高版本)<br/>.appx （适用于 Windows 10，版本 1607年及更高版本)                 |

### <a name="third-party-installers"></a>第三方安装程序

多个常用第三方产品和安装程序现在支持的功能打包桌面应用程序。 有关更多详细信息和这些第三方产品的列表，请参阅[打包桌面应用程序使用第三方安装程序](desktop-to-uwp-third-party-installer.md)。

### <a name="manual-packaging"></a>手动打包

作为最后一个选项，可以将你的应用程序转换而无需使用任何一种工具。 如果想精细地控制你的转换，则可以创建清单文件，然后运行 **MakeAppx.exe** 工具来创建 Windows 应用包。

请参阅[手动打包桌面应用程序](desktop-to-uwp-manual-conversion.md)。

## <a name="add-modern-windows-10-experiences"></a>添加现代 Windows 10 体验

为桌面应用程序创建一个 MSIX 包后，可以使用 UWP Api，包扩展和 UWP 组件，例如更好现代、 更具吸引力的 Windows 10 体验实时磁贴和通知。

### <a name="enhance-with-uwp-apis"></a>使用 UWP Api 进行增强

将应用打包后，就可以通过添加动态磁贴和推送通知等功能对应用进行完善。 某些功能可以显著提高应用程序的参与度和它们成本很短的时间来添加。 某些增强功能需要更多的代码。

请参阅[桌面应用程序中使用 UWP Api](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)。

### <a name="integrate-with-package-extensions"></a>集成包扩展

如果你的应用程序需要与系统集成 (例如： 建立防火墙规则)、 描述您的应用程序在包清单中的这些事物和系统将完成其余部分。 对于其中大多数任务，你根本不必编写任何代码。 使用少量的 XML 清单中，您可以执行操作像在用户登录时启动的进程，将你的应用程序集成到文件资源管理器，并添加你的应用程序出现在其他应用中的打印目标的列表。

请参阅[桌面应用程序集成包扩展](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions)。

### <a name="extend-with-uwp-components"></a>使用 UWP 组件进行扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 一般情况下，首先应确定是否可以通过 UWP API [增强](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)现有桌面应用程序来增加你的体验。 如果您必须使用 UWP 组件，将获得的体验，你可以将 UWP 项目添加到你的解决方案和应用服务用于桌面应用程序和 UWP 组件之间进行通信。

请参阅[扩展桌面应用程序提供 UWP 组件](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)。

## <a name="test"></a>测试

中实际设置测试你的应用程序，如准备进行分发，它是应用程序进行签名，然后将其安装。 请参阅[测试你的应用](desktop-to-uwp-debug.md#test-your-app)。

>[!IMPORTANT]
> 如果你打算发布到 Microsoft Store 应用程序，请确保你的应用程序在 S 模式下运行 Windows 10 的设备上正常运行。 这是存储要求。 请参阅[测试 Windows 应用是否适用于 S 模式下的 Windows 10](desktop-to-uwp-test-windows-s.md)。

## <a name="validate"></a>验证

若要为应用程序提供的已发布在 Microsoft Store 或逐渐成为最大限度地[Windows 认证](https://go.microsoft.com/fwlink/p/?LinkID=309666)、 验证和本地测试，然后提交以进行认证。

如果使用 DAC 来打包你的应用，你可以使用新``-Verify``标志，用于针对打包桌面应用程序和应用商店要求验证包。 请参阅[打包应用程序，登录应用程序中，并准备将其提交到应用商店](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters)。

如果你使用 Visual Studio，可以验证你的应用程序从**创建应用程序包**向导。 请参阅[创建应用包上传文件](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps#create-an-app-package-upload-file)。

若要手动运行该工具，请参阅 [Windows 应用认证工具包](/windows/uwp/debug-test-perf/windows-app-certification-kit)。

若要查看 Windows 应用认证用于验证应用的测试列表，请参阅 [Windows 桌面桥应用测试](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests)。

## <a name="distribute"></a>分配

您可以将你的应用程序通过 Microsoft Store 发布或通过旁加载到其他系统。

请参阅[分发打包桌面应用程序](/windows/apps/desktop/modernize/desktop-to-uwp-distribute)。

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
