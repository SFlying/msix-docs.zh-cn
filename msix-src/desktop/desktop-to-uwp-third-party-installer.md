---
Description: 本指南提供了一系列第三方产品和打包桌面应用程序的安装程序。
title: 包使用第三方安装程序的桌面应用程序
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e07d44d2a809a183d57b59f3121683bf0e2ab971
ms.sourcegitcommit: 0378a8897e0691bee4d9a957982961a377974856
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67253482"
---
# <a name="package-a-desktop-app-using-third-party-installers"></a>包使用第三方安装程序的桌面应用程序

下面是常用的第三方产品和支持的功能打包桌面应用程序的安装程序的列表。 只需单击几下即可使用它们来生成 MSI 安装程序或应用包。 我们不提供有关如何使用这些工具的文档，不过你可以访问它们的网站以了解详细信息。

## <a name="advanced-installer"></a>Advanced Installer

Caphyon 提供基于 GUI 的免费桌面应用打包工具。使用此工具，你只需单击几下即可为应用程序生成 Windows 应用包。 它可以使用任何安装程序;甚至是那些在静默模式下运行，并执行验证检查以确定应用程序是否适用于打包。 Desktop App Converter 还与 Hyper-V 和 [VMware](https://www.vmware.com/) 集成。 这意味着你可以使用自己的虚拟机，而无需下载大小可能超过 3GB 的匹配的 [Docker](https://docs.docker.com/) 映像。

<img width="20%" src="images/Advanced_Installer_Vertical.png">

可以使用 [Advanced Installer](https://www.advancedinstaller.com/) 从现有项目中生成 MSI 和 [Windows 应用包](https://www.advancedinstaller.com/uwp-app-package.html)。 还可以使用 Advanced Installer 导入使用 Microsoft Desktop App Converter 生成的 Windows 应用包。 导入后，可以使用专为 UWP 应用设计的可视化工具对它们进行维护。

Advanced Installer 还提供用于 Visual Studio 2017 和 2015 的扩展，可用于[生成和调试桌面桥应用](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html)。

请观看此[视频](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be)进行简要了解。

> [!TIP]
> 请务必检查最近发布的 [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html)。

## <a name="cloudhouse-compatibility-containers"></a>Cloudhouse 兼容性容器

对于其业务线应用程序与 Windows 10 和 10 S 不兼容的企业客户，Cloudhouse 的兼容性容器支持在 Window 10 上运行 Windows XP 和 7 应用，然后转换为在通用 Windows 平台 (UWP) 上运行，以便无需更改源代码就可通过适用于企业的 Microsoft Store 或 Microsoft InTune 交付。 注册即可[免费试用](https://www.cloudhouse.com/free-trial)。

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse 打包线业务应用程序提供自动 Packager[兼容性容器](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications)应用立即运行的操作系统上 (例如：Windows XP)，然后[准备用于转换](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905)到 UWP。 该容器随后将通过与 Microsoft 的 Desktop App Converter 工具集成转换为新的 Windows 应用包格式。

该自动打包工具使用安装/捕获和运行时分析来为应用程序创建一个容器，其中包括应用程序文件、注册表、运行时、依赖项以及允许该应用程序在 Windows 10 上运行的兼容性和重定向引擎。 该容器为应用程序及其运行时提供隔离，从而使其影响用户设备上运行的其他应用程序或与之冲突。

有关如何通过适用于企业的 Microsoft Store 提供企业应用程序的详细信息，请阅读[版本博客](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp)。

## <a name="firegiant"></a>FireGiant

[FireGiant MSIX 扩展](https://www.firegiant.com/products/wix-expansion-pack/msix)，可以从相同的 WiX 源代码的同时创建 Windows 应用包和 MSI 包。 每次生成时，可以针对 Windows 10 的 Windows 应用包和早期版本的 Windows 使用 MSI。

<img width="20%" src="images/FG3rdPartyLogo.png">

FireGiant MSIX 扩展使用静态分析和智能仿真的 WiX 项目来创建 Windows 应用程序包，而无需容器或虚拟机的磁盘空间和运行时开销。

因为 FireGiant MSIX 扩展不会将您的安装程序转换通过运行它，可以维护 WiX 安装程序，而无需反复将其转换为 Windows 应用包。 你的使用不同版本 Windows 的所有用户都可获取最新改进功能，你无需担心 MSI 和 Windows 应用包不同步。

请查看此[视频](https://www.youtube.com/watch?v=AFBpdBiAYQE)并查看如何使用几行代码 FireGiant 首席执行官 Rob Mensching 创建 Appx （Windows 应用包） 的版本受欢迎的开源 7 Zip 压缩工具，然后他如何改进这两个 Windows 应用程序和使用相同的 WiX 源代码中的更改的 MSI 包。

## <a name="installaware"></a>InstallAware

InstallAware，与[跟踪记录](https://www.installaware.com/press-room.htm)快速支持 Microsoft 的创新，构建[Windows 应用包 （桌面桥）](https://www.installaware.com/appx-builder.htm)，APP-V （应用程序虚拟化），MSI （Windows 安装程序） 和从单个源 EXE （本机代码） 包。

<img width="20%" src="images/installaware.png">

InstallAware 提供 Visual Studio 版本 2012年至 2017年免费 InstallAware 扩展。 你可以直接从 [Visual Studio 工具栏](https://www.installaware.com/visual-studio-installer-2015.htm)单击一下，就可使用它们来创建 Windows 应用包。

您还可以导入任何设置，即使你没有设置过程中，通过使用的 PackageAware （捕获快照免费安装程序） 的源代码或数据库导入向导 （适用于所有安装程序 MSI 和 MSM 合并模块）。 你可以使用 [GUI 工具](https://www.installaware.com/scripting-two-way-integrated-ide.htm) 来以可视形式或脚本形式维护和增强你的导入。

[高级 APPX 创建选项](https://www.installaware.com/mhtml5/desktop/appx.htm)可帮助你应对 Microsoft Store 提交，或生成签名 Windows 应用包二进制文件以旁加载形式分发给最终用户。 您甚至可以创造确定部署到目标的 WSA （Windows Server 应用程序） 安装程序包**Nano Server**全部从单一源，并完全支持[命令行自动化](https://www.installaware.com/scripting-automation-interface.htm)、 除了到 GUI。

InstallAware 还[开源](https://www.installaware.com/gnu.asp) **APPX 生成器库**一起使用的示例命令行小程序，根据 GNU Affero GPL 许可。 这些都是专为用于开源平台（如 WiX）而设计。

## <a name="installshield"></a>InstallShield

InstallShield 提供单一解决方案来开发 MSI 与 EXE 安装程序、创建通用 Windows 平台 (UWP) 和 Windows Server App (WSA) 包，以及虚拟化应用程序，而所需的脚本编写、编码和改编工作极少。

<img width="20%" src="images/InstallShield-logo.jpg">

在数秒内扫描你的 InstallShield 项目，通过自动识别你的应用程序与 UWP 及 WSA 包之间潜在的兼容性问题，省去数小时的调查工作。

通过基于现有 InstallShield 项目生成 UWP 应用包，为 Microsoft Store 做准备，并简化软件在 Windows 10 上的安装体验。 生成 Windows Installer 和 UWP 应用包，以支持你的所有客户所需的部署方案。 通过基于现有 InstallShield 项目生成 WSA 包，支持 Nano Server 和 Windows Server 2016 部署。

以模块形式开发你的安装程序，以便更轻松地进行部署和维护，然后在生成时将各个组件和依赖项合并到单个 UWP 应用包中，通过 Microsoft Store 分发。 在存储外的直接分发，捆绑包 UWP 应用包和套件/高级 UI 安装程序以及其他依赖项。

在此[电子书](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0)中了解详细信息。

## <a name="pace-suite"></a>PACE 套件

[PACE 套件](https://pacesuite.com/)是用于将你的桌面应用引入通用 Windows 平台的应用程序打包工具。

<img width="20%" src="images/PACE.png">

有了 PACE 套件，你无需再准备特殊的打包环境或安装额外的 Windows SDK 组件。 PACE 套件可以在 Windows 10 或 Windows Server 2016 下的标准包装环境中独立构建 Windows 应用包。 查看此[带图示例](https://pacesuite.com/convert-exe-to-appx/)以了解 PACE 套件如何将安装程序重新打包到 Windows 应用包。

除创建 Windows 应用包外，你还可以使用 PACE 套件创建 Windows Installer 包 (MSI)、修补程序 (MSP)、转换程序 (MST) 和 APP-V 包。 针对 MSI 创作，PACE 套件可帮助管理升级、权限设置、自定义操作和脚本等。 你可以将你的应用程序直接发布到 System Center Configuration Manager。

若要查看所有应用程序打包功能，请参阅 [PACE 套件功能](https://pacesuite.com/features/)。

## <a name="rad-studio"></a>RAD Studio

查看 [Embarcadero 的 RAD Studio](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="raypack-studio"></a>RayPack Studio

Raynet 的打包解决方案[RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio)、 作为一个高效且易于配置转换的多个可能的结果对于桌面应用程序支持的包的创建和重新打包框架。

<img width="20%" src="images/RaynetLogo_v3.png">

现有的虚拟环境（VMware 工作站、Hyper-V）可在不进行长时间环境设置的情形下用于执行自动/批量转换。 RayPack Studio 的组件，[RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)，可利用预转换屏蔽和兼容性测试来验证可用于转换的合格软件。 此外，用户现在可以对多种版本的 Windows 10（包括周年和创意者更新）进行全面的冲突性和兼容性检查。

除创建 Windows 10 APPX/UWP 格式软件包外，RayPack Studio 还可用于创建经典 Windows Installer 包 (MSI)、修补程序 (MSP)、转换程序 (MST) 和 APP-V 包。 此外，此解决方案附带一组软件产品和专业企业软件包组件。 除了软件打包和虚拟化，RayPack Studio 还可以完成所有打包相关任务：软件应用程序及软件包的冲突性和兼容性检查 ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad))、软件评估 ([RayEval](https://raynet.de/Raynet-Products/RayEval)) 和质量保证 ([RayQC](https://raynet.de/Raynet-Products/RayQC))。

通过与 [RayFlow](https://raynet.de/Raynet-Products/RayFlow)（Raynet 的企业工作流系统）结合使用，用户可以通过完整的企业应用程序生命周期（从程序包订购到评估、分析、打包、质量保证、用户接受度测试和部署）高效地开发软件。 所有的程序包和格式可被直接储存和部署到 SCCM 或其他解决方案中。 RayFlow 会对整个应用程序生命周期进程进行追踪和管理。 除此之外，RayFlow 还可以对 ServiceNow 等所有订单系统进行集成。 Raynet 利用面向服务提供者的工具在世界各地构建软件打包工厂。

说服自己获取 Raynet 提供的 RayPack Studio 和 RayFlow [免费试用版许可](https://raynet.de/contact?init=license)。 有关详细信息，请访问 [www.raynet.de](https://raynet.de/home)。

相关的链接：

* Raynet：[https://raynet.de/home](https://raynet.de/home)
* RayPack Studio：[https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow：[https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval：[https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC：[https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced：[https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* 免费试用版许可：[https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)
