---
Description: 为现有 Windows 窗体、WPF 或 Win32 应用或游戏创建现代 Windows 应用包。 添加新式 Windows 10 用户体验并简化部署和货币化率。
title: 打包桌面应用程序
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7e831023a6d21447d1175a1f802c568c5023be87
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "67126824"
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

如果已有应用包 （例如，MSI 或 APP-V 安装程序），我们建议你使用[MSIX 打包工具](../mpt-overview.md)重新打包为 MSIX 格式对现有桌面应用程序。 它提供了交互式用户界面和命令行的转换，并使你能够将应用程序而无需的源代码。 

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

### <a name="third-party-installer"></a>第三方安装程序

 多个常用第三方产品和安装程序现在支持的功能打包桌面应用程序。 只需单击几下即可使用它们来生成 MSI 安装程序或应用包。 我们不提供有关如何使用这些工具的文档，不过你可以访问它们的网站以了解详细信息。

#### <a name="advanced-installer"></a>Advanced Installer

Caphyon 提供基于 GUI 的免费桌面应用打包工具。使用此工具，你只需单击几下即可为应用程序生成 Windows 应用包。 它可以使用任何安装程序;甚至是那些在静默模式下运行，并执行验证检查以确定应用程序是否适用于打包。
Desktop App Converter 还与 Hyper-V 和 [VMware](https://www.vmware.com/) 集成。 这意味着你可以使用自己的虚拟机，而无需下载大小可能超过 3GB 的匹配的 [Docker](https://docs.docker.com/) 映像。

<img width="20%" src="images/Advanced_Installer_Vertical.png">

可以使用 [Advanced Installer](https://www.advancedinstaller.com/) 从现有项目中生成 MSI 和 [Windows 应用包](https://www.advancedinstaller.com/uwp-app-package.html)。 还可以使用 Advanced Installer 导入使用 Microsoft Desktop App Converter 生成的 Windows 应用包。 导入后，可以使用专为 UWP 应用设计的可视化工具对它们进行维护。

Advanced Installer 还提供用于 Visual Studio 2017 和 2015 的扩展，可用于[生成和调试桌面桥应用](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html)。

请观看此[视频](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be)进行简要了解。

> [!TIP]
> 请务必检查最近发布的 [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html)。

#### <a name="cloudhouse-compatibility-containers"></a>Cloudhouse 兼容性容器

对于其业务线应用程序与 Windows 10 和 10 S 不兼容的企业客户，Cloudhouse 的兼容性容器支持在 Window 10 上运行 Windows XP 和 7 应用，然后转换为在通用 Windows 平台 (UWP) 上运行，以便无需更改源代码就可通过适用于企业的 Microsoft Store 或 Microsoft InTune 交付。 注册即可[免费试用](https://www.cloudhouse.com/free-trial)。

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse 打包线业务应用程序提供自动 Packager[兼容性容器](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications)应用立即运行的操作系统上 (例如：Windows XP)，然后[准备用于转换](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905)到 UWP。 该容器随后将通过与 Microsoft 的 Desktop App Converter 工具集成转换为新的 Windows 应用包格式。

该自动打包工具使用安装/捕获和运行时分析来为应用程序创建一个容器，其中包括应用程序文件、注册表、运行时、依赖项以及允许该应用程序在 Windows 10 上运行的兼容性和重定向引擎。 该容器为应用程序及其运行时提供隔离，从而使其影响用户设备上运行的其他应用程序或与之冲突。

有关如何通过适用于企业的 Microsoft Store 提供企业应用程序的详细信息，请阅读[版本博客](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp)。

#### <a name="firegiant"></a>FireGiant

[FireGiant MSIX 扩展](https://www.firegiant.com/products/wix-expansion-pack/msix)，可以从相同的 WiX 源代码的同时创建 Windows 应用包和 MSI 包。 每次生成时，可以针对 Windows 10 的 Windows 应用包和早期版本的 Windows 使用 MSI。

<img width="20%" src="images/FG3rdPartyLogo.png">

FireGiant MSIX 扩展使用静态分析和智能仿真的 WiX 项目来创建 Windows 应用程序包，而无需容器或虚拟机的磁盘空间和运行时开销。

因为 FireGiant MSIX 扩展不会将您的安装程序转换通过运行它，可以维护 WiX 安装程序，而无需反复将其转换为 Windows 应用包。 你的使用不同版本 Windows 的所有用户都可获取最新改进功能，你无需担心 MSI 和 Windows 应用包不同步。

请查看此[视频](https://www.youtube.com/watch?v=AFBpdBiAYQE)并查看如何使用几行代码 FireGiant 首席执行官 Rob Mensching 创建 Appx （Windows 应用包） 的版本受欢迎的开源 7 Zip 压缩工具，然后他如何改进这两个 Windows 应用程序和使用相同的 WiX 源代码中的更改的 MSI 包。

#### <a name="installaware"></a>InstallAware

安装 **Aware** 及快速支持 Microsoft 创新的[跟踪记录](https://www.installaware.com/press-room.htm)，通过单个来源构建 [Windows 应用包（桌面桥）](https://www.installaware.com/appx-builder.htm)、App-V（应用程序虚拟化）、MSI (Windows Installer) 和 EXE（本机代码）包。

<img width="20%" src="images/installaware.png">

Install**Aware** 为 2012-2017 版本的 Visual Studio 提供了免费的 Install**Aware** 扩展。 你可以直接从 [Visual Studio 工具栏](https://www.installaware.com/visual-studio-installer-2015.htm)单击一下，就可使用它们来创建 Windows 应用包。

即使你没有设置的源代码，你也可以通过使用 Package**Aware** 或数据库导入向导（适用于所有 MSI 安装程序和 MSM 合并模块）导入任何设置（无快照设置捕获）。 你可以使用 [GUI 工具](https://www.installaware.com/scripting-two-way-integrated-ide.htm) 来以可视形式或脚本形式维护和增强你的导入。

[高级 APPX 创建选项](https://www.installaware.com/mhtml5/desktop/appx.htm)可帮助你应对 Microsoft Store 提交，或生成签名 Windows 应用包二进制文件以旁加载形式分发给最终用户。 你甚至可以生成从单个来源定向部署到 **Nano 服务器** 的 **WSA**（Windows Server 应用程序）安装程序包，并且，除了 GUI 之外，还完全支持[命令行自动化](https://www.installaware.com/scripting-automation-interface.htm)。

另外，通过 GNU Affero GPL 许可证安装 **Aware**[开源](https://www.installaware.com/gnu.asp)**APPX 生成器库** 以及命令行小应用程序示例。 这些都是专为用于开源平台（如 WiX）而设计。

#### <a name="installshield"></a>InstallShield

InstallShield 提供单一解决方案来开发 MSI 与 EXE 安装程序、创建通用 Windows 平台 (UWP) 和 Windows Server App (WSA) 包，以及虚拟化应用程序，而所需的脚本编写、编码和改编工作极少。

<img width="20%" src="images/InstallShield-logo.jpg">

在数秒内扫描你的 InstallShield 项目，通过自动识别你的应用程序与 UWP 及 WSA 包之间潜在的兼容性问题，省去数小时的调查工作。

通过基于现有 InstallShield 项目生成 UWP 应用包，为 Microsoft Store 做准备，并简化软件在 Windows 10 上的安装体验。 生成 Windows Installer 和 UWP 应用包，以支持你的所有客户所需的部署方案。 通过基于现有 InstallShield 项目生成 WSA 包，支持 Nano Server 和 Windows Server 2016 部署。

以模块形式开发你的安装程序，以便更轻松地进行部署和维护，然后在生成时将各个组件和依赖项合并到单个 UWP 应用包中，通过 Microsoft Store 分发。 在存储外的直接分发，捆绑包 UWP 应用包和套件/高级 UI 安装程序以及其他依赖项。

在此[电子书](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0)中了解详细信息。

#### <a name="pace-suite"></a>PACE 套件

[PACE 套件](https://pacesuite.com/)是用于将你的桌面应用引入通用 Windows 平台的应用程序打包工具。

<img width="20%" src="images/PACE.png">

有了 PACE 套件，你无需再准备特殊的打包环境或安装额外的 Windows SDK 组件。 PACE 套件可以在 Windows 10 或 Windows Server 2016 下的标准包装环境中独立构建 Windows 应用包。 查看此[带图示例](https://pacesuite.com/convert-exe-to-appx/)以了解 PACE 套件如何将安装程序重新打包到 Windows 应用包。

除创建 Windows 应用包外，你还可以使用 PACE 套件创建 Windows Installer 包 (MSI)、修补程序 (MSP)、转换程序 (MST) 和 APP-V 包。 针对 MSI 创作，PACE 套件可帮助管理升级、权限设置、自定义操作和脚本等。 你可以将你的应用程序直接发布到 System Center Configuration Manager。

若要查看所有应用程序打包功能，请参阅 [PACE 套件功能](https://pacesuite.com/features/)。

#### <a name="rad-studio"></a>RAD Studio

查看 [Embarcadero 的 RAD Studio](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### <a name="raypack-studio"></a>RayPack Studio

Raynet 的打包解决方案[RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio)、 作为一个高效且易于配置转换的多个可能的结果对于桌面应用程序支持的包的创建和重新打包框架。

<img width="20%" src="images/RaynetLogo_v3.png">

现有的虚拟环境（VMware 工作站、Hyper-V）可在不进行长时间环境设置的情形下用于执行自动/批量转换。 RayPack Studio 的组件，[RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)，可利用预转换屏蔽和兼容性测试来验证可用于转换的合格软件。 此外，用户现在可以对多种版本的 Windows 10（包括周年和创意者更新）进行全面的冲突性和兼容性检查。

除创建 Windows 10 APPX/UWP 格式软件包外，RayPack Studio 还可用于创建经典 Windows Installer 包 (MSI)、修补程序 (MSP)、转换程序 (MST) 和 APP-V 包。 此外，此解决方案附带一组软件产品和专业企业软件包组件。 除了软件打包和虚拟化，RayPack Studio 还可以完成所有打包相关任务：软件应用程序及软件包的冲突性和兼容性检查 ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad))、软件评估 ([RayEval](https://raynet.de/Raynet-Products/RayEval)) 和质量保证 ([RayQC](https://raynet.de/Raynet-Products/RayQC))。

通过与 [RayFlow](https://raynet.de/Raynet-Products/RayFlow)（Raynet 的企业工作流系统）结合使用，用户可以通过完整的企业应用程序生命周期（从程序包订购到评估、分析、打包、质量保证、用户接受度测试和部署）高效地开发软件。 所有的程序包和格式可被直接储存和部署到 SCCM 或其他解决方案中。 RayFlow 会对整个应用程序生命周期进程进行追踪和管理。 除此之外，RayFlow 还可以对 ServiceNow 等所有订单系统进行集成。 Raynet 利用面向服务提供者的工具在世界各地构建软件打包工厂。

说服自己获取 Raynet 提供的 RayPack Studio 和 RayFlow [免费试用版许可](https://raynet.de/contact?init=license)。 有关详细信息，请访问 [www.raynet.de](https://raynet.de/home)。

**相关链接**：

* Raynet：[https://raynet.de/home](https://raynet.de/home)
* RayPack Studio：[https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow：[https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval：[https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC：[https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced：[https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* 免费试用版许可：[https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

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
