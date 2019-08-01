---
Description: 本指南说明如何配置 Visual Studio 解决方案, 以便通过本机映像优化应用程序二进制文件。
title: 通过本机映像优化 .NET 桌面应用
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10、uwp、.msix、本机映像编译器
ms.localizationpriority: medium
ms.openlocfilehash: 6ce41bafb2debfb8c2d99135634ba30c91f4400a
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68685402"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>通过本机映像优化 .NET 桌面应用

你可以通过预编译二进制文件来改善 .NET Framework 应用程序的启动时间。 可以在通过 Microsoft Store 打包和分发的大型应用程序上使用此技术。 在某些情况下, 我们观察到性能提高了 20%。 有关此技术的详细信息, 请参阅[技术概述](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)。

我们已将本机映像编译器作为[NuGet 包](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)发布。 可以将此包应用于面向 .NET Framework 4.6.2 或更高版本的任何 .NET Framework 应用程序。 此包将向应用程序使用的所有二进制文件中添加一个包含本机有效负载的 post 生成步骤。 当应用程序在 .NET 4.7.2 和更高版本中运行时, 将加载此优化的负载, 而以前版本仍将加载 MSIL 代码。

[Windows 10 2018 年4月更新](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)中包含[.net framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) 。 你还可以在运行 Windows 7 + 和 Windows Server 2008 R2 + 的电脑上安装此版本的 .NET Framework。

> [!IMPORTANT]
> 如果要生成由 Windows 应用程序打包项目打包的应用程序的本机映像, 请确保将项目的目标平台最低版本设置为 Windows 周年周年更新。

## <a name="how-to-produce-native-images"></a>如何生成本机映像

按照这些说明进行操作以配置项目。

1. 将目标框架配置为4.6.2 或更高版本

2. 将目标平台配置为 x86 或 x64 

3. 添加 NuGet 包。

4. 创建发布版本。

## <a name="configure-the-target-framework-as-462-or-above"></a>将目标框架配置为4.6.2 或更高版本

若要将项目配置为面向 .NET Framework 4.6.2, 需要 .NET Framework 4.6.2 开发工具或更高版本。 这些工具可通过 Visual Studio 安装程序作为 .NET 桌面开发工作负载下的可选组件提供:

![安装 .NET 4.6.2 开发工具](images/install-4.6.2-devpack.png)

或者, 你可以从以下内容获取 .NET 开发人员包:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>将目标平台配置为 x86 或 x64

本机映像编译器优化给定平台的代码。 若要使用它, 需要将应用程序配置为面向某个特定平台, 如 x86 或 x64。

如果你的解决方案中有多个项目, 则必须将入口点项目 (最有可能是生成可执行文件的项目) 编译为 x86 或 x64。 从主项目引用的其他二进制文件将用主项目中指定的体系结构进行处理, 即使它们编译为 AnyCPU 也是如此。

配置项目:

1. 右键单击解决方案, 然后选择 " **Configuration Manager**"。

2. 选择 **< 新建 ... "。** 在**平台**下拉菜单中, 在生成可执行文件的项目的名称旁 >。

3. 在 "**新建项目平台**" 对话框中, 确保 "从下拉列表**复制设置**" 设置为 "**任何 CPU**"。

![配置 x86](images/configure-x86.png)

如果要生成 x64 `Release/x64`二进制文件, 请重复此步骤。

>[!IMPORTANT]
> 本机映像编译器不支持 AnyCPU 配置。

## <a name="add-the-nuget-packages"></a>添加 NuGet 包

本机映像编译器以 NuGet 包的形式提供, 你需要将其添加到生成可执行文件的 Visual Studio 项目中。 这通常是 Windows 窗体或 WPF 项目。 使用此 PowerShell 命令来执行此操作。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 1.0.0
```

## <a name="create-a-release-build"></a>创建发布版本

NuGet 包将项目配置为针对发布版本运行其他工具。 此工具将本机代码添加到相同的二进制文件。
若要验证该工具是否已处理二进制文件, 您可以查看生成输出, 以确保它包含如下所示的消息:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>常见问题

**:.在没有 .NET Framework 4.7.2 的情况下, 新的二进制文件在计算机上是否正常工作？**

A. 优化的二进制文件在 .NET Framework 4.7.2 运行时可从改进中受益。 运行以前的 .NET framework 版本的客户端将从二进制文件加载非优化的 MSIL 代码。

**:.如何提供反馈或报告问题？**

A. 使用 Visual Studio 2017 中的反馈工具报告问题。 [详细信息](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**:.将本机映像添加到现有二进制文件中有什么影响？**

A. 优化的二进制文件包含托管代码和本机代码, 使最终文件更大。

**:.能否使用这种技术发布二进制文件？**

A. 此版本包含你现在可以使用的 "上线" 许可证。
