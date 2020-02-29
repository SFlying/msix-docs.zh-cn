---
Description: 本文列出了在打包桌面应用程序之前需要知道的事项。 可能不需要执行很多操作即可使应用为打包过程做好准备。
title: 准备打包桌面应用程序 (MSIX)
ms.date: 08/22/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 432017a083ae3f9553bea88902378ca3e6556bcb
ms.sourcegitcommit: a7f677e024e415168f8bf0080f4e115307833a1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2020
ms.locfileid: "77576907"
---
# <a name="prepare-to-package-a-desktop-application"></a>准备打包桌面应用程序

本文列出了在打包桌面应用程序之前需要知道的事项。 可能不需要执行很多操作即可使应用为打包过程做好准备，但是如果以下任意一项适用于你的应用程序，则在打包之前需要解决该问题。

+ __.NET 应用程序需要低于 4.6.2 的 .NET Framework 版本__。 如果你正在打包 .NET 应用程序，我们建议使应用程序以 .NET Framework 4.6.2 或更高版本为目标。 Windows 10 版本 1607（也称为周年更新）中首次引入了安装和运行打包桌面应用程序的功能，此 OS 版本默认包含 .NET Framework 4.6.2。 更高版本的 OS 包含更高版本的 .NET Framework。 有关更高版本的 Windows 10 中包含的 .NET 版本完整列表，请参阅[此文](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies)。

  在大多数情况下，预期都可以在打包的桌面应用程序中以低于 4.6.2 的 .NET Framework 版本为目标。 但是，如果以低于 4.6.2 的版本为目标，则在将打包的桌面应用程序分发给用户之前，应先对其进行全面测试。

  + 4.0 - 4.6.1：以这些 .NET Framework 版本为目标的应用程序在 4.6.2 或更高版本中预期也可以正常运行。 因此，在 Windows 10 版本 1607 或更高版本上使用 OS 原装 .NET Framework 版本的情况下，这些应用程序应该无需进行任何更改即可安装和运行。

  + 2.0 和 3.5：根据我们的测试，以这些 .NET Framework 版本为目标的打包桌面应用程序通常可以正常运行，但某些情况下可能会出现性能问题。 要使这些打包的应用程序能够安装和运行，必须在目标计算机上安装 [.NET Framework 3.5 功能](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10)（此功能还包括 .NET Framework 2.0 和 3.0）。 打包这些应用程序后，还应该对其进行全面的测试。

+ __应用程序始终使用提升的安全特权运行__。 应用程序需要在以交互用户身份运行时工作。 安装应用程序的用户可能不是系统管理员，因此需要应用程序以提升的权限运行意味着它无法对标准用户正确运行。 如果打算将应用发布到 Mcirosoft Store，请注意，Store 不接受任何功能部分需要提升权限的应用。

+ __应用程序需要内核模式驱动程序或 Windows 服务__。 MSIX 不支持内核模式驱动程序或需要在系统帐户下运行的 Windows 服务。 使用[后台任务](/windows/uwp/launch-resume/create-and-register-a-background-task)，而不是 Windows 服务。

+ __在进程内将应用的模块加载到不在 Windows 应用包中的进程__。 不允许此操作，这意味着不支持进程中扩展，如 [shell 扩展](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果你在同一个程序包中有两个应用，则可以在它们之间执行进程间通信。

+ __确保应用程序安装的任何扩展安装在应用程序所安装到的位置__。 Windows 允许用户和 IT 管理者更改包的默认安装位置。  查看“设置”->“系统”>“存储”->“更多存储设置”->“更改新内容的保存位置”->“新应用将保存到”。  如果在应用程序中安装某个扩展，请确保该扩展在安装文件夹方面没有其他限制。  例如，某些扩展可能会禁止将其扩展安装到非系统驱动器。  如果默认位置已更改，这会导致错误 0x80073D01 (ERROR_DEPLOYMENT_BLOCKED_BY_POLICY)。 

+ __应用程序使用自定义应用程序用户模型 ID (AUMID)__ 。 如果进程调用 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) 以设置其自己的 AUMID，则它可能仅使用应用程序模型环境/Windows 应用包为其生成的 AUMID。 无法定义自定义 AUMID。

+ __应用程序修改 HKEY_LOCAL_MACHINE (HKLM) 注册表配置单元__。 应用程序的任何创建 HKLM 键或打开一个键以进行修改的尝试都会导致拒绝访问失败。 请记住，应用程序具有自身的注册表专用虚拟化视图，因此用户范围和计算机范围的注册表配置单元的概念（HKLM 的定义）不适用。 你将需要找到另一种方法来实现 HKLM 的用途，如改为写入 HKEY_CURRENT_USER (HKCU)。

+ __应用程序使用 ddeexec 注册表子项作为启动另一个应用的方式__。 改为使用[应用程序包清单](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中的各种可激活*扩展配置的 DelegateExecute 谓词处理程序之一。

+ __应用程序会写入 AppData 文件夹或注册表，目的是与其他应用共享数据__。 转换后，AppData 将重定向到本地应用数据存储，该存储是每个应用的专用应用商店。

  应用程序将写入 HKEY_LOCAL_MACHINE 注册表配置单元的所有条目都将重定向到隔离的二进制文件中，应用程序写入 HKEY_CURRENT_USER 注册表配置单元的任何条目都将按用户、按应用放入专用位置。 有关文件和注册表重定向的更多详细信息，请参阅[在桌面桥幕后](desktop-to-uwp-behind-the-scenes.md)。  

  使用不同的进程间数据共享方式。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __应用程序写入应用的安装目录__。 例如，应用程序写入与你的 exe 放置在同一个目录中的日志文件。 此操作不受支持，因此你需要找到另一个位置，如本地应用数据存储。

+ __应用程序使用当前工作目录__。 在运行时，打包的桌面应用程序不会获得先前在桌面 .LNK 快捷方式中指定的相同工作目录。 如果具有正确的目录对应用程序正常运行很重要，需要在运行时更改 CWD。

  > [!NOTE]
  > 如果应用需要写入安装目录或使用当前工作目录，则你还可以考虑将一个使用[包支持框架](https://github.com/microsoft/MSIX-PackageSupportFramework)的运行时修复程序添加到包中。 有关更多详细信息，请参阅[此文](../psf/package-support-framework.md)。 

+ __应用程序安装需要用户交互__。 应用程序安装程序必须能够在无提示的情况下运行，并且必须安装默认不在干净 OS 映像中的所有必备组件。

+ __应用程序需要 UIAccess__。 如果应用程序在 UAC 清单的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，则当前不支持转换为 MSIX。 有关详细信息，请参阅 [UI 自动化安全概述](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __应用程序公开 COM 对象__。 来自程序包内的进程和扩展可以注册并使用 COM 和 OLE 服务器，进程内和进程外 (OOP) 皆可。  创意者更新添加了打包的 COM 支持，它提供注册 OOP COM 和 OLE 服务器（现在这些服务器在包外部可见）的功能。  请参阅[对桌面桥的 COM 服务器和 OLE 文档支持](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge)。

   打包的 COM 支持适用于现有的 COM API，但不适用于依赖直接读取注册表的应用程序扩展，因为打包的 COM 位于一个专用位置。

+ __应用程序公开 GAC 程序集以供其他进程使用__。 应用程序无法公开 GAC 程序集以供来自 Windows 应用包外部可执行文件的进程使用。 来自包内部的进程可以照常注册和使用 GAC 程序集，但它们在外部将不可见。 这意味着，OLE 等互操作方案在被外部进程调用时不起作用。

+ __应用程序正以不受支持的方式链接 C 运行时库 (CRT)__ 。 Microsoft C/C++ 运行时库提供用于为 Microsoft Windows 操作系统编程的例程。 这些例程自动执行许多不采用 C 和 C++ 语言提供的常见编程任务。 如果应用程序利用 C/C++ 运行时库，则你需要确保它以受支持的方式链接。

    对于最新版本的 CRT，Visual Studio 2017 即支持静态和动态链接，以允许代码使用常见 DLL 文件，也支持静态链接，以将库直接链接到代码。 如果可能，我们建议让应用程序将动态链接与 VS 2017 一起使用。

    以前版本的 Visual Studio 中的支持各有不同。 有关详细信息，请参阅下表：

    <table>
    <th>Visual Studio 版本</td><th>动态链接</th><th>静态链接</th></th>
    <tr><td>2005 (VC 8)</td><td>不支持</td><td>支持</td>
    <tr><td>2008 (VC 9)</td><td>不支持</td><td>支持</td>
    <tr><td>2010 (VC 10)</td><td>支持</td><td>支持</td>
    <tr><td>2012 (VC 11)</td><td>支持</td><td>不支持</td>
    <tr><td>2013 (VC 12)</td><td>支持</td><td>不支持</td>
    <tr><td>2015 和 2017 (VC 14)</td><td>支持</td><td>支持</td>
    </table>

    注意：在所有情况下，都必须链接到最新公开发布的 CRT。

+ __应用程序从 Windows 并行文件夹安装和加载程序集__。 例如，应用程序使用 C 运行时库 VC8 或 VC9，并且正在从 Windows 并行文件夹动态链接它们，这意味着代码正在使用来自共享文件夹的常见 DLL 文件。 这不受支持。 你将需要静态链接它们，方法是将可再发行库文件直接链接到你的代码中。

+ __应用程序使用 System32/SysWOW64 文件夹中的依赖项__。 若要使这些 DLL 有效，必须将其包含在 Windows 应用包的虚拟文件系统部分中。 这可确保应用程序的行为就像 DLL 已安装在 **System32**/**SysWOW64** 文件夹中一样。 在程序包的根目录中，创建一个名为 **VFS** 的文件夹。 在该文件夹内创建 **SystemX64** 和 **SystemX86** 文件夹。 然后，将 DLL 的 32 位版本放置在 **SystemX86** 文件夹，并将 64 位版本放置在 **SystemX64** 文件夹。

+ __应用程序使用 VCLibs 框架包__。 如果转换 C++ Win32 应用，必须处理 Visual C++ Runtime 的部署。 Visual Studio 2019 和 Windows SDK 在以下文件夹中包含 Visual C++ Runtime 版本 11.0、12.0 和14.0 的最新框架包：

    * **VC 14.0 框架包**：C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop\14.0

    * **VC 12.0 框架包**：C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop.120\14.0

    * **VC 11.0 框架包**：C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop.110\14.0

    若要使用其中的一个包，必须在包清单中以依赖项的形式引用该包。 当客户从 Microsoft Store 安装零售版应用时，将连同该应用一起从 Store 安装该包。 如果旁边加载应用，则不会安装依赖项。 若要手动安装依赖项，必须使用位于上面所列安装文件夹中的、适用于 x86、x64 或 ARM 的相应 .appx 包来安装相应的框架包。

    若要在应用中引用 C++ Visual Runtime 框架包：

    1. 转到上面所列的、适用于应用所用 Visual C++ Runtime 版本的框架包安装文件夹。

    2. 打开该文件夹中的 SDKManifest.xml 文件，找到 `FrameworkIdentity-Debug` 或 `FrameworkIdentity-Retail` 属性（取决于使用的是运行时调试版还是零售版），然后复制该属性中的 `Name` 和 `MinVersion` 值。 例如，下面是当前 VC 14.0 框架包的 `FrameworkIdentity-Retail` 属性。
        ```xml
        FrameworkIdentity-Retail = "Name = Microsoft.VCLibs.140.00.UWPDesktop, MinVersion = 14.0.27323.0, Publisher = 'CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US'"
        ```

    3. 在应用包清单中的 `<Dependencies>` 节点下，添加以下 `<PackageDependency>` 元素。 确保将 `Name` 和 `MinVersion` 值替换为在上一步骤中复制的值。 以下示例指定当前版本的 VC 14.0 框架包的依赖项。
        ```xml
        <PackageDependency Name="Microsoft.VCLibs.140.00.UWPDesktop" MinVersion="14.0.27323.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
        ```

+ __应用程序包含自定义跳转列表__。 使用跳转列表时需要注意几个问题和事项。

    - __应用的体系结构与 OS 不匹配__。  如果应用程序与 OS 体系结构不匹配（例如在 x64 Windows 上运行 x86 应用程序），则跳转列表目前无法正常工作。 当前没有其他解决方法，只能重新编译应用程序，使其匹配体系结构。

    - __应用程序创建跳转列表条目并调用 [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) 或 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__ 。 不要在代码中以编程方式设置你的 AppID。 否则，将会导致不显示你的跳转列表条目。 如果应用程序需要自定义的 ID，请使用清单文件指定。 有关说明，请参阅[手动打包桌面应用程序](desktop-to-uwp-manual-conversion.md)。 应用程序的 AppID 在 *YOUR_PRAID_HERE* 部分中指定。

    - __应用程序添加跳转列表 shell 链接，该链接引用包中的可执行文件__。 你不能通过跳转列表直接启动程序包中的可执行文件（应用自身 .exe 的绝对路径除外）。 改为注册应用执行别名（此别名允许通过关键字启动打包的桌面应用程序，就像它在 PATH 环境变量中一样），并设置指向别名的链接目标路径。 有关如何使用 appExecutionAlias 扩展的详细信息，请参阅[将桌面应用程序与 Windows 10 集成](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions)。 注意，如果要求跳转列表中的链接资源与原始 .exe 匹配，你将需要使用 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) 和 PKEY_Title 的显示名称设置图标等资源，就像设置其他自定义条目那样。

    - __应用程序添加跳转列表条目，该条目通过绝对路径引用应用包中的资源__。 更新应用程序的包时，应用程序的安装路径可能会更改，从而更改资源的位置（例如图标、文档、可执行文件等）。 如果跳转列表条目通过绝对路径引用此类资源，则应用程序应定期刷新其跳转列表（例如应用程序启动时）以确保正确解析路径。 或者，改用 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API，使你可以使用程序包相对的 ms-resource URI 方案（也与语言、DPI 和高对比度相关）来引用字符串和图像资源。

+ __应用程序启动一个实用工具来执行任务__。 避免启动 PowerShell 和 Cmd.exe 等命令实用工具。 实际上，如果用户将应用程序安装在运行 Windows 10 S 的系统上，则应用程序根本无法启动这些实用工具。 这可能会使应用程序无法提交到 Microsoft Store，因为提交到 Microsoft Store 的所有应用都必须与 Windows 10 S 兼容。

     启动实用工具通常可以提供一种方便的方法，用于从操作系统获取信息、访问注册表或访问系统功能。 但是，可以改用 UWP API 来完成此类任务。 这些 API 的性能更强，因为它们无需单独的可执行文件即可运行，但更重要的是，它们会阻止应用程序访问包的外部。 应用的设计与打包的应用随附的隔离、信任和安全性保持一致，并且应用程序将按预期在运行 Windows 10 S 的系统上运行。

+ __应用程序托管加载项、插件或扩展__。   在许多情况下，只要尚未对扩展打包并且该扩展以完全信任方式安装，COM 样式的扩展可能会继续工作。 这是因为那些安装程序可以使用其完全信任的功能修改注册表，并将扩展文件放置在主机应用程序应找到这些文件的任意位置。

   但是，如果这些扩展已打包，然后作为 Windows 应用包安装，则它们将无法正常工作，因为每个包（主机应用程序和扩展）将彼此隔离。 有关如何从系统隔离应用程序的详细信息，请参阅[在桌面桥幕后](desktop-to-uwp-behind-the-scenes.md)。

     用户安装到运行 Windows 10 S 的系统上的所有应用程序和扩展都必须作为 Windows 应用包安装。 因此，如果想要对扩展打包，或者打算鼓励你的参与者对其打包，请考虑如何能够促进主机应用程序包和任意扩展包之间的通信。 使用[应用服务](https://docs.microsoft.com/windows/uwp/launch-resume/app-services)即可实现此目的。

+ __应用程序生成代码__。 应用程序可以生成其在内存中使用的代码，但请避免将生成的代码写入磁盘，因为 Windows 应用认证过程在应用提交之前无法验证该代码。 此外，向磁盘写入代码的应用将无法在运行 Windows 10 S 的系统中正常运行。这可能会使应用程序无法提交到 Microsoft Store，因为提交到 Microsoft Store 的所有应用都必须与 Windows 10 S 兼容。

>[!IMPORTANT]
> 创建 Windows 应用包后，请测试应用程序，确保它可以在运行 Windows 10 S 的系统上正常工作。所有提交到 Microsoft Store 的应用必须与 Windows 10 S 兼容。Microsoft Store 不接受不兼容的应用。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](desktop-to-uwp-test-windows-s.md)。