---
Description: 本文列出了打包桌面应用程序之前需要了解的一些知识。 你可能不需要执行很多操作即可使应用为打包过程做好准备。
title: 准备打包桌面应用程序 (Desktop Bridge)
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: b46dfe767f84987e6f4930b8a263812904274e3d
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68685388"
---
# <a name="prepare-to-package-a-desktop-application"></a>准备打包桌面应用程序

本文列出了打包桌面应用程序之前需要了解的知识。 你可能无需执行任何操作即可为打包过程准备好应用程序, 但如果以下任何项适用于你的应用程序, 则需要在打包之前解决。 请记住，Microsoft Store 会为你处理授权和自动更新，使你能够从基本代码中删除与这些任务相关的任何功能。

+ __你的 .net 应用程序需要 .NET Framework 早于4.6.2 的版本__。 如果正在打包 .NET 应用程序, 我们建议将应用程序目标 .NET Framework 4.6.2 或更高版本。 Windows 10 版本 1607 (也称为周年更新) 中首次引入了安装和运行打包桌面应用程序的功能, 默认情况下, 此 OS 版本包含 .NET Framework 4.6.2。 更高版本的操作系统包括 .NET Framework 的更高版本。 有关更高版本的 Windows 10 中包含的 .NET 版本的完整列表, 请参阅[此文](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies)。

  在大多数情况下, 适用于打包桌面应用程序中4.6.2 之前的 .NET Framework 的目标版本应能正常工作。 但是, 如果面向的是比4.6.2 更早的版本, 则应在将打包的桌面应用程序分发给用户之前对其进行完整的测试。

  + 4.0-4.6.1:面向这些版本的 .NET Framework 的应用程序应在4.6.2 或更高版本上运行, 而不会出现问题。 因此, 这些应用程序应在 Windows 10 版本1607或更高版本上安装和运行, 并附带操作系统附带的 .NET Framework 版本。

  + 2.0 和 3.5:在我们的测试中, 以这些版本的 .NET Framework 为目标的打包桌面应用程序通常可以运行, 但在某些情况下可能会出现性能问题。 为了安装和运行这些打包的应用程序, 必须在目标计算机上安装[.NET Framework 3.5 功能](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10)(此功能还包括 .NET Framework 2.0 和 3.0)。 打包后, 还应彻底测试这些应用程序。

+ __应用程序始终以提升的安全特权运行__。 在以交互式用户身份运行时, 应用程序需要工作。 从 Microsoft Store 安装你的应用程序的用户可能不是系统管理员, 因此要求你的应用程序运行提升意味着它不会对标准用户正常运行。 Microsoft Store 中不接受任何功能部分需要提升的应用。

+ __应用程序需要内核模式驱动程序或 Windows 服务__。 桌面桥适用于应用程序, 但它不支持需要在系统帐户下运行的内核模式驱动程序或 Windows 服务。 使用[后台任务](/windows/uwp/launch-resume/create-and-register-a-background-task)，而不是 Windows 服务。

+ __在进程内将应用的模块加载到不在 Windows 应用包中的进程__。 不允许此操作，这意味着不支持进程中扩展，如 [shell 扩展](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果你在同一个程序包中有两个应用，则可以在它们之间执行进程间通信。

+ __应用程序使用自定义应用程序用户模型 ID (AUMID)__ 。 如果你的过程调用[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx)来设置其自己的 AUMID, 则它可能仅使用应用程序模型环境/Windows 应用包为其生成的 AUMID。 无法定义自定义 AUMID。

+ __应用程序将修改 HKEY_LOCAL_MACHINE (HKLM) 注册表配置单元__。 应用程序创建 HKLM 密钥或打开一个密钥以进行修改的任何尝试都会导致拒绝访问失败。 请记住, 应用程序具有其自己的专用注册表视图, 因此, 用户和计算机范围的注册表配置单元 (即 HKLM) 的概念不适用。 你将需要找到另一种方法来实现 HKLM 的用途，如改为写入 HKEY_CURRENT_USER (HKCU)。

+ __应用程序使用 ddeexec 注册表子项作为启动其他应用程序的一种方法__。 改为使用[应用程序包清单](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中的各种可激活*扩展配置的 DelegateExecute 谓词处理程序之一。

+ __应用程序将数据写入 AppData 文件夹或注册表, 目的是与另一个应用程序共享数据__。 转换后，AppData 将重定向到本地应用数据存储，该存储是每个 UWP 应用的专用应用商店。

  应用程序写入 HKEY_LOCAL_MACHINE 注册表配置单元的所有项都将重定向到一个独立的二进制文件, 并且应用程序写入 HKEY_CURRENT_USER 注册表配置单元的任何项都将放置到专用的每个用户、每个应用的位置。 有关文件和注册表重定向的更多详细信息，请参阅[在桌面桥幕后](desktop-to-uwp-behind-the-scenes.md)。  

  使用不同的进程间数据共享方式。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __您的应用程序将写入您的应用程序的安装目录__。 例如, 你的应用程序将写入日志文件, 并将其放入你的 exe 所在的同一目录中。 此操作不受支持，因此你需要找到另一个位置，如本地应用数据存储。

+ __应用程序安装需要用户交互__。 应用程序安装程序必须能够以无提示方式运行, 并且它必须安装在干净的操作系统映像上默认不启用的所有必备组件。

+ __应用程序使用当前工作目录__。 在运行时, 打包的桌面应用程序将不会获得你先前在桌面中指定的工作目录.LNK 快捷方式。 如果要使应用程序正常运行, 则需要在运行时更改 CWD。

+ __应用程序需要 UIAccess__。 如果应用程序在 UAC 清单的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，则当前不支持转换为 UWP。 有关详细信息，请参阅 [UI 自动化安全概述](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __应用程序公开 COM 对象__。 来自程序包内的进程和扩展可以注册并使用 COM 和 OLE 服务器，进程内和进程外 (OOP) 皆可。  创意者更新添加了打包的 COM 支持，它提供注册 OOP COM 和 OLE 服务器（现在这些服务器在包外部可见）的功能。  请参阅[对桌面桥的 COM 服务器和 OLE 文档支持](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge)。

   打包的 COM 支持适用于现有的 COM API，但不适用于依赖直接读取注册表的应用程序扩展，因为打包的 COM 位于一个专用位置。

+ __应用程序公开 GAC 程序集以供其他进程使用__。 在当前版本中, 你的应用程序不能公开 GAC 程序集供源自 Windows 应用包外部的可执行进程使用。 来自程序包内的进程可以照常注册和使用 GAC 程序集，但它们在外部将不可见。 这意味着，OLE 等互操作方案在被外部进程调用时不起作用。

+ __应用程序以不受支持的方式链接 C 运行时库 (CRT)__ 。 Microsoft C/C++ 运行时库提供用于为 Microsoft Windows 操作系统编程的例程。 这些例程自动执行许多不采用 C 和 C++ 语言提供的常见编程任务。 如果你的应用程序使用C++ C/运行时库, 则需要确保它以受支持的方式进行链接。

    对于最新版本的 CRT，Visual Studio 2017 即支持静态和动态链接，以允许你的代码使用常见 DLL 文件，也支持静态链接，以将库直接链接到你的代码。 如果可能, 建议应用程序使用 VS 2017 的动态链接。

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

    注意:在所有情况下, 必须链接到最新的公开可用 CRT。

+ __应用程序从 Windows 并行文件夹中安装和加载程序集__。 例如, 你的应用程序使用 C 运行时库 VC8 或 VC9, 并从 Windows 并行文件夹动态链接它们, 这意味着你的代码使用共享文件夹中的常用 DLL 文件。 这不受支持。 你将需要静态链接它们，方法是将可再发行库文件直接链接到你的代码中。

+ __应用程序使用 System32/SysWOW64 文件夹中的依赖项__。 若要使这些 DLL 有效，必须将其包含在 Windows 应用包的虚拟文件系统部分中。 这可确保应用程序的行为就像在**System32**/**SysWOW64**文件夹中安装了 dll。 在程序包的根目录中，创建一个名为 **VFS** 的文件夹。 在该文件夹内创建 **SystemX64** 和 **SystemX86** 文件夹。 然后，将 DLL 的 32 位版本放置在 **SystemX86** 文件夹，并将 64 位版本放置在 **SystemX64** 文件夹。

+ __你的应用使用 VCLibs 框架程序包__。 如果 VCLibs 库被定义为 Windows 应用包中的依赖项，则可以直接从 Microsoft Store 中安装。 例如, 如果应用程序使用 Dev11 VCLibs 包, 请对应用程序包清单进行以下更改:`<Dependencies>`在节点下, 添加:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
在从 Microsoft Store 安装期间，将在安装应用之前先安装 VCLibs 框架的适当版本（x86 或 x64）。  
如果应用程序是通过旁加载安装的, 则不会安装依赖项。 若要在计算机上手动安装这些依赖项，必须下载并安装桌面桥的相应 VCLibs 框架包。 有关这些方案的详细信息，请参阅 [在 Centennial 项目中使用 Visual C++ 运行时](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。

  **框架包**：

  * [桌面桥的 VC 14.0 framework 包](https://www.microsoft.com/download/details.aspx?id=53175)
  * [桌面桥的 VC 12.0 framework 包](https://www.microsoft.com/download/details.aspx?id=53176)
  * [桌面桥的 VC 11.0 framework 包](https://www.microsoft.com/download/details.aspx?id=53340)


+ __您的应用程序包含一个自定义跳转列表__。 使用跳转列表时需要注意几个问题和注意事项。

    - __应用程序的体系结构与操作系统不匹配。__  如果应用程序和操作系统体系结构不匹配 (例如, 在 x64 Windows 上运行的 x86 应用程序), 跳转列表当前不能正常工作。 目前, 除了将您的应用程序重新编译为匹配的体系结构以外, 没有其他解决方法。

    - __应用程序将创建跳转列表项并调用[ICustomDestinationList:: SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx)或[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__ 。 不要在代码中以编程方式设置你的 AppID。 否则，将会导致不显示你的跳转列表条目。 如果应用程序需要自定义 Id, 请使用清单文件指定它。 有关说明, 请参阅[手动打包桌面应用程序](desktop-to-uwp-manual-conversion.md)。 应用程序的 AppID 在 *YOUR_PRAID_HERE* 部分中指定。

    - __应用程序会添加一个跳转列表 shell 链接, 该链接引用包中的可执行文件__。 你不能通过跳转列表直接启动程序包中的可执行文件（应用自身 .exe 的绝对路径除外）。 相反, 注册应用执行别名 (允许打包的桌面应用程序通过关键字, 就像它位于路径上一样), 并改为将链接目标路径设置为别名。 有关如何使用 appExecutionAlias 扩展的详细信息, 请参阅将[桌面应用程序与 Windows 10 集成](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions)。 注意，如果要求跳转列表中的链接资源与原始 .exe 匹配，你将需要使用 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) 和 PKEY_Title 的显示名称设置图标等资源，就像设置其他自定义条目那样。

    - __应用程序会添加按绝对路径引用应用包中的资产的跳转列表项__。 应用程序的安装路径在更新其包时可能会更改, 更改资产的位置 (如图标、文档、可执行文件, 等等)。 如果跳转列表项按绝对路径引用此类资产, 则应用程序应定期刷新其跳转列表 (例如, 在应用程序启动时) 以确保路径正确解析。 或者，改用 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API，使你可以使用程序包相对的 ms-resource URI 方案（也与语言、DPI 和高对比度相关）来引用字符串和图像资源。

+ __您的应用程序将启动一个实用工具来执行任务__。 避免启动 PowerShell 和 Cmd.exe 等命令实用工具。 事实上, 如果用户将您的应用程序安装到运行 Windows 10 S 的系统上, 则您的应用程序将无法启动。 这可能会阻止应用程序提交到 Microsoft Store, 因为提交到 Microsoft Store 的所有应用都必须与 Windows 10 S 兼容。

启动实用工具通常可以提供一种方便的方法，用于从操作系统获取信息、访问注册表或访问系统功能。 但是，你可以改为使用 UWP API 完成这些类型的任务。 这些 Api 的性能更高, 因为它们不需要单独的可执行文件来运行, 但更重要的是, 它们会使应用程序不会到达包外。 应用的设计与你已打包的应用程序附带的隔离、信任和安全保持一致, 并且你的应用程序将在运行 Windows 10 S 的系统上按预期方式运行。

+ __应用程序承载外接程序、插件或扩展__。   在许多情况下，只要尚未对扩展打包并且该扩展以完全信任方式安装，COM 样式的扩展则可能会继续工作。 这是因为, 这些安装程序可以使用其完全信任的功能来修改注册表, 并在宿主应用程序需要查找它们的位置放置扩展文件。

   但是, 如果将这些扩展打包, 然后将其安装为 Windows 应用包, 则它们将不起作用, 因为每个包 (主机应用程序和扩展) 将彼此隔离。 若要了解有关如何将应用程序与系统隔离的详细信息, 请参阅[桌面桥的幕后](desktop-to-uwp-behind-the-scenes.md)。

 用户安装到运行 Windows 10 S 的系统上的所有应用程序和扩展都必须作为 Windows 应用包安装。 因此, 如果你想要打包扩展, 或者计划鼓励你的参与者将它们打包, 则请考虑你可以如何促进主机应用程序包和任何扩展包之间的通信。 能够做到这点的一种方法是使用[应用服务](https://docs.microsoft.com/windows/uwp/launch-resume/app-services)。

+ __应用程序生成代码__。 您的应用程序可以生成它在内存中使用的代码, 但避免将生成的代码写入磁盘, 因为在提交应用程序之前, Windows 应用程序认证过程无法验证该代码。 此外, 将代码写入磁盘的应用程序不会在运行 Windows 10 S 的系统上正常运行。这可能会阻止应用程序提交到 Microsoft Store, 因为提交到 Microsoft Store 的所有应用都必须与 Windows 10 S 兼容。

>[!IMPORTANT]
> 创建 Windows 应用包后, 请对应用程序进行测试, 以确保其在运行 Windows 10 S 的系统上正常运行。提交到 Microsoft Store 的所有应用都必须与 Windows 10 S 兼容。应用商店中不会接受不兼容的应用。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](desktop-to-uwp-test-windows-s.md)。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**为桌面应用创建 Windows 应用包**

请参阅[创建 Windows 应用包](desktop-to-uwp-root.md#convert)。
