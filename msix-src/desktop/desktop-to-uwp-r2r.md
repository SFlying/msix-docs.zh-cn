---
Description: 本指南介绍如何配置 Visual Studio 解决方案，以使用本机映像优化应用程序二进制文件。
title: 使用本机映像优化 NET 桌面应用
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix, 本机映像编译器
ms.localizationpriority: medium
ms.openlocfilehash: ebc2c7351fef0856d83529e55ba4ebf012aaaeed
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77073087"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>使用本机映像优化 NET 桌面应用

可以通过预编译二进制文件来改善 .NET Framework 应用程序的启动时间。 可以针对通过 Microsoft Store 打包和分发的大型应用程序使用此技术。 我们已观察到，在某些情况下，这样可以将性能提升 20%。 可以在[技术概述](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)中详细了解此技术。

我们以 [NuGet 包](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)的形式发布了本机映像编译器。 可将此包应用到面向 .NET Framework 4.6.2 或更高版本的任何 .NET Framework 应用程序。 此包会将一个包含本机有效负载的生成后步骤添加到应用程序使用的所有二进制文件。 当应用程序在 .NET 4.7.2 和更高版本中运行时，将加载这个优化的有效负载，而旧版本仍然加载 MSIL 代码。

[Windows 10 2018 年 4 月更新](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)中包含了 [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/)。 还可以在运行 Windows 7+ 和 Windows Server 2008 R2+ 的电脑上安装此版本的 .NET Framework。

> [!IMPORTANT]
> 若要为 Windows 应用程序打包项目打包的应用程序生成本机映像，请确保将项目的“目标平台最低版本”设置为“Windows 周年更新”。

## <a name="how-to-produce-native-images"></a>如何生成本机映像

按照以下说明配置项目。

1. 将目标框架配置为 4.6.2 或以上

2. 将目标平台配置为 x86 或 x64 

3. 添加 NuGet 包。

4. 创建发布版本。

## <a name="configure-the-target-framework-as-462-or-above"></a>将目标框架配置为 4.6.2 或以上

若要将项目配置为面向 .NET Framework 4.6.2，需要安装 .NET Framework 4.6.2 或更高版本的开发工具。 在 .NET 桌面开发工作负荷中，这些工具以可选组件的形式通过 Visual Studio 安装程序提供：

![安装 .NET 4.6.2 开发工具](images/install-4.6.2-devpack.png)

或者，可以从 [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks) 获取 .NET 开发人员包

## <a name="configure-the-target-platform-as-x86-or-x64"></a>将目标平台配置为 x86 或 x64

本机映像编译器优化给定平台的代码。 若要使用此工具，需将应用程序配置为面向某个特定平台，例如 x86 或 x64。

如果解决方案中包含多个项目，只能将入口点项目（很有可能是生成可执行文件的项目）编译为 x86 或 x64。 从主项目引用的其他二进制文件将使用主项目中指定的体系结构进行处理，即使这些二进制文件编译为 AnyCPU。

若要配置项目：

1. 右键单击解决方案并选择“配置管理器”。 

2. 在生成可执行文件的项目名称旁边的“平台”下拉菜单中，选择“<新建...>”。  

3. 在“新建项目平台”对话框中，确保“从此处复制设置”下拉列表设置为“任何 CPU”。   

![配置 x86](images/configure-x86.png)

若要生成 x64 二进制文件，请对 `Release/x64` 重复此步骤。

>[!IMPORTANT]
> 本机映像编译器不支持“任何 CPU”配置。

## <a name="add-the-nuget-packages"></a>添加 NuGet 包

本机映像编译器以 NuGet 包的形式提供，需将其添加到生成可执行文件的 Visual Studio 项目中。 此项目通常是 Windows 窗体或 WPF 项目。 使用以下 PowerShell 命令执行此操作。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 1.0.0
```

## <a name="create-a-release-build"></a>创建发布版本

NuGet 包将项目配置为针对发布版本运行一个附加的工具。 此工具将本机代码添加到相同的二进制文件。
若要验证该工具是否已处理二进制文件，可以查看生成输出，确保其中包含如下所示的消息：

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

可以通过在项目文件中将属性 `NgenR2R` 设置为 `true`，对非发布版本触发本机映像编译。

## <a name="faq"></a>FAQ

**问：在没有 .NET Framework 4.7.2 的情况下，新二进制文件是否可在计算机上正常运行？**

A. 在装有 .NET Framework 4.7.2 的情况下运行时，优化的二进制文件可以从改进的功能中受益。 运行旧版 .NET Framework 的客户端将从二进制文件中加载未优化的 MSIL 代码。

**问：如何提供反馈或报告问题？**

A. 请使用 Visual Studio 2017 中的“反馈”工具报告问题。 [详细信息](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**问：将本机映像添加到现有二进制文件会造成什么影响？**

A. 优化的二进制文件包含托管代码和本机代码，使最终文件变得更大。

**问：是否可以使用此技术发布二进制文件？**

A. 此版本包含立即可供使用的“上线”许可证。
