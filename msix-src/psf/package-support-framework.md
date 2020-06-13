---
Description: 解决阻止桌面应用程序在 .MSIX 容器中运行的问题
title: 解决阻止桌面应用程序在 .MSIX 容器中运行的问题
ms.date: 05/14/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c878ccb97ea3b77635344a36feb1d63c3f42f9ed
ms.sourcegitcommit: e3a06eccd3322053b8b498cb6343fb6f711a7a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724601"
---
# <a name="get-started-with-package-support-framework"></a>包支持框架入门 

[包支持框架](package-support-framework-overview.md)是一个开源工具包，可帮助你将修补程序应用于现有的桌面应用程序（无需修改代码），使其能够在 .msix 容器中运行。 包支持框架可帮助应用程序遵循新式运行时环境的最佳做法。

本文提供了有关包支持框架的每个组件的各异次，并逐步介绍了如何使用该组件。

## <a name="understand-what-is-inside-a-package-support-framework"></a>了解包支持框架内的内容

包支持框架包含一个可执行文件、一个运行时管理器 DLL 和一组运行时修复程序。

![包支持框架](images/package-support-framework.png)

过程如下： 
1. 创建一个配置文件，用于指定要应用于应用程序的修补程序。 
1. 将包修改为指向包支持框架（PSF）启动器可执行文件。

当用户启动应用程序时，包支持框架启动器是运行的第一个可执行文件。 该启动器将读取配置文件，并将运行时修复程序和运行时管理器 DLL 注入应用程序进程。 如果应用程序需要使用该修复程序才能在 MSIX 容器中运行，则运行时管理器会应用该修复程序。

![包支持框架 DLL 注入](images/package-support-framework-2.png)

## <a name="step-1-identify-packaged-application-compatibility-issues"></a>步骤1：识别打包的应用程序兼容性问题

首先，为应用程序创建包。 然后，安装它，运行它并观察它的行为。 收到的错误消息可能会帮助你识别兼容性问题。 也可以使用[进程监视器](https://docs.microsoft.com/sysinternals/downloads/procmon)来识别问题。  常见问题与有关工作目录和程序路径权限的应用程序假设相关。

### <a name="using-process-monitor-to-identify-an-issue"></a>使用进程监视器识别问题

[进程监视器](https://docs.microsoft.com/sysinternals/downloads/procmon)是一个功能强大的实用工具，用于观察应用的文件和注册表操作及其结果。  这可以帮助你了解应用程序的兼容性问题。  打开进程监视器后，添加筛选器（筛选 > 筛选器 ...），以仅包含应用程序可执行文件中的事件。

![Procmon 配合应用筛选器](images/procmon_app_filter.png)

将显示事件列表。 对于其中的许多事件，"**结果**" 列中将显示 "**成功**" 字样。

![Procmon 配合事件](images/procmon_events.png)

或者，您可以将事件筛选为仅显示失败。

![Procmon 配合排除成功](images/procmon_exclude_success.png)

如果你怀疑文件系统访问失败，请搜索 System32/SysWOW64 或包文件路径下的失败事件。 筛选器也可以在此处提供帮助。 从列表底部开始，向上滚动。 最近出现在此列表底部的失败。 请注意包含字符串（如 "拒绝访问" 和 "找不到路径/名称"）的错误，并忽略看起来不可疑的问题。 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)有两个问题。 在下图中显示的列表中可以看到这些问题。

![Procmon 配合 Config.txt](images/procmon_config_txt.png)

在此图中出现的第一个问题中，应用程序无法从位于 "C:\Windows\SysWOW64" 路径中的 "Config.txt" 文件读取。 应用程序不太可能尝试直接引用该路径。 大多数情况下，它会尝试使用相对路径从该文件中读取，默认情况下，"System32/SysWOW64" 是应用程序的工作目录。 这表明，应用程序需要将当前工作目录设置为包中的某个位置。 查看 appx 内部，可以看到该文件与可执行文件位于同一目录中。

![应用 Config.txt](images/psfsampleapp_config_txt.png)

下图显示了第二个问题。

![Procmon 配合日志文件](images/procmon_logfile.png)

在此问题中，应用程序无法将 .log 文件写入其包路径。 这会建议使用文件重定向修复方法。

<a id="find"></a>

## <a name="step-2-find-a-runtime-fix"></a>步骤2：查找运行时修补程序

PSF 包含可立即使用的运行时修补程序，如文件重定向修正。

### <a name="file-redirection-fixup"></a>文件重定向修正

你可以使用[文件重定向修正](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup)功能重定向尝试写入或读取目录中的数据，该目录无法从在 .msix 容器中运行的应用程序访问。

例如，如果你的应用程序写入到与你的应用程序可执行文件位于同一目录中的日志文件，则可以使用[文件重定向修正](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup)功能在其他位置（例如本地应用数据存储）中创建该日志文件。

### <a name="runtime-fixes-from-the-community"></a>社区中的运行时修补程序

请确保查看[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework)页面的社区贡献。 其他开发人员可能会解决类似于你的问题并已共享运行时修复。

## <a name="step-3-apply-a-runtime-fix"></a>步骤3：应用运行时修补程序

您可以使用 Windows SDK 中的一些简单工具来应用现有运行时修补程序，并通过执行以下步骤。

> [!div class="checklist"]
> * 创建包布局文件夹
> * 获取包支持框架文件
> * 将它们添加到包
> * 修改包清单
> * 创建配置文件

让我们完成每个任务。

### <a name="create-the-package-layout-folder"></a>创建包布局文件夹

如果已经有 .msix （或 .appx）文件，可以将其内容解压缩到一个布局文件夹中，该文件夹将用作包的暂存区域。 可以在命令提示符下使用 Makeappx.exe 工具执行此操作，具体取决于 SDK 的安装路径，你可以在 Windows 10 电脑上找到 makeappx.exe 工具： x86： C:\Program Files （x86） \Windows Kits\10\bin\x86\makeappx.exe x64： C:\Program Files （x86） \Windows Kits\10\bin\x64\makeappx.exe

```powershell
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

这会显示如下所示的内容。

![包布局](images/package_contents.png)

如果没有要开始使用的 .msix （或 .appx）文件，可以从头开始创建包文件夹和文件。

### <a name="get-the-package-support-framework-files"></a>获取包支持框架文件

可以通过使用独立的 Nuget 命令行工具或通过 Visual Studio 获取 PSF Nuget 包。

#### <a name="get-the-package-by-using-the-command-line-tool"></a>使用命令行工具获取包

从以下位置安装 Nuget 命令行工具： https://www.nuget.org/downloads 。 然后，在 Nuget 命令行中运行以下命令： 

```powershell
nuget install Microsoft.PackageSupportFramework
```

或者，可以将包扩展重命名为 .zip 并将其解压缩。 所需的所有文件都将位于/bin 文件夹下。

#### <a name="get-the-package-by-using-visual-studio"></a>使用 Visual Studio 获取包

在 Visual Studio 中，右键单击解决方案或项目节点，然后选择 "管理 Nuget 包" 命令之一。  搜索 " **PackageSupportFramework** " 或 " **PSF** " 以查找 Nuget.org 上的包。然后，安装它。

### <a name="add-the-package-support-framework-files-to-your-package"></a>将包支持框架文件添加到包

将必需的32位和64位 PSF Dll 和可执行文件添加到包目录中。 使用下表作为指南。 还需要包含所需的任何运行时修补程序。 在我们的示例中，我们需要文件重定向运行时修复。

| 应用程序可执行文件为 x64 | 应用程序可执行文件为 x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

包内容现在应如下所示。

![包二进制文件](images/package_binaries.png)

### <a name="modify-the-package-manifest"></a>修改包清单

在文本编辑器中打开包清单，并将 `Executable` 元素的属性设置 `Application` 为 PSF 启动程序可执行文件的名称。  如果你知道目标应用程序的体系结构，请选择适当的版本，PSFLauncher32.exe 或 PSFLauncher64.exe。  否则，PSFLauncher32.exe 将适用于所有情况。  下面是一个示例。

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>创建配置文件

创建文件名 ``config.json`` ，并将该文件保存到包的根文件夹。 修改文件上 config.js的已声明应用 ID，使其指向刚替换的可执行文件。 使用通过处理监视器获得的知识，还可以设置工作目录，并使用文件重定向修正将读取/写入操作重定向到包相对 "PSFSampleApp" 目录下的 .log 文件。

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```

下面是针对架构的 config.js指南：

| Array | 键 | Value |
|-------|-----------|-------|
| applications | id |  使用 `Id` `Application` 包清单中元素的属性的值。 |
| applications | 可执行文件 | 要启动的可执行文件的包相对路径。 在大多数情况下，你可以在修改之前从包清单文件中获取此值。 它是元素的属性的值 `Executable` `Application` 。 |
| applications | workingDirectory | 可有可无要用作启动的应用程序的工作目录的包相对路径。 如果未设置此值，操作系统将使用 `System32` 目录作为应用程序的工作目录。 |
| 进程 | 可执行文件 | 在大多数情况下，这将是上面配置的名称， `executable` 其中包含已删除的路径和文件扩展名。 |
| 修正 | dll | 要加载的链接地址的包相对路径，.msix/.appx。 |
| 修正 | config | 可有可无控制修正 dll 的行为方式。 此值的准确格式因修正链接而异，因为每个修正都可以根据需要解释此 "blob"。 |

`applications`、 `processes` 和 `fixups` 键是数组。 这意味着，可以使用文件 config.js来指定多个应用程序、进程和修复 DLL。

### <a name="package-and-test-the-app"></a>打包并测试应用程序

接下来，创建一个包。

```powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

然后，对其进行签名。

```powershell
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

有关详细信息，请参阅[如何创建包签名证书](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)和[如何使用 signtool 为包签名](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

使用 PowerShell 安装包。

>[!NOTE]
> 请记得首先卸载包。

```powershell
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

运行应用程序，并在应用运行时修补程序时观察行为。  根据需要重复诊断和打包步骤。

### <a name="check-whether-the-package-support-framework-is-running"></a>检查包支持框架是否正在运行 

您可以检查运行时修复程序是否正在运行。 执行此操作的一种方法是打开**任务管理器**，然后单击 "**更多详细信息**"。 找到应用包支持框架的应用，并展开应用详细信息以视图更多详细信息。 你应能够查看包支持框架是否正在运行。 

### <a name="use-the-trace-fixup"></a>使用跟踪修正

诊断打包应用程序兼容性问题的一种替代方法是使用跟踪修正。 此 DLL 包含在 PSF 中，并提供应用程序行为的详细诊断视图，类似于进程监视器。  它专门设计用于显示应用程序兼容性问题。  若要使用跟踪修正，请将 DLL 添加到包，将以下片段添加到 config.js上的，然后打包并安装应用程序。

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

默认情况下，跟踪修正将筛选出可能被视为 "预期" 的失败。  例如，应用程序可能会尝试无条件删除文件，而不检查其是否已存在，并忽略结果。 这会导致某些意外的失败被筛选掉，因此，在上述示例中，我们选择接收来自文件系统功能的所有失败。 我们这样做的原因是，在尝试从 Config.txt 文件中读取消息失败，并显示消息 "找不到文件"。 这是一种通常会出现的错误，通常不会被认为是意外情况。 在实践中，可能最好仅开始筛选意外故障，然后在出现仍无法确定的问题时回退到所有失败。

默认情况下，跟踪修正的输出将发送到附加的调试器。 在此示例中，我们不会附加调试器，而是使用 SysInternals 中的[DebugView](https://docs.microsoft.com/sysinternals/downloads/debugview)程序来查看其输出。 运行应用程序后，可以看到与之前相同的故障，这会使我们向相同的运行时修复。

![找不到 TraceShim 文件](images/traceshim_filenotfound.png)

![拒绝访问 TraceShim](images/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>调试、扩展或创建运行时修补程序

你可以使用 Visual Studio 来调试运行时修补程序，扩展运行时修补程序，或者从头开始创建一个修补程序。 你需要执行这些操作才能成功。

> [!div class="checklist"]
> * 添加打包项目
> * 为运行时修复添加项目
> * 添加启动 PSF 启动器可执行文件的项目
> * 配置打包项目

完成后，解决方案将如下所示。

![已完成解决方案](images/runtime-fix-project-structure.png)

让我们看看此示例中的每个项目。

| 项目 | 目标 |
|-------|-----------|
| DesktopApplicationPackage | 此项目基于[Windows 应用程序打包项目](../desktop/desktop-to-uwp-packaging-dot-net.md)，并输出 .msix 包。 |
| Runtimefix | 这是一个 c + + 动态链接库项目，其中包含一个或多个用于运行时修复的替换函数。 |
| PSFLauncher | 这是 c + + 空项目。 此项目是用于收集包支持框架的运行时可分发文件的位置。 它输出可执行文件。 该可执行文件是在启动解决方案时运行的第一件事。 |
| WinFormsDesktopApplication | 此项目包含桌面应用程序的源代码。 |

若要查看包含所有这些类型项目的完整示例，请参阅[PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)。

让我们逐步完成在解决方案中创建和配置这些项目的步骤。

### <a name="create-a-package-solution"></a>创建包解决方案

如果还没有适用于桌面应用程序的解决方案，请在 Visual Studio 中创建一个新的**空白解决方案**。

![空白解决方案](images/blank-solution.png)

你可能还需要添加任何应用程序项目。

### <a name="add-a-packaging-project"></a>添加打包项目

如果还没有**Windows 应用程序打包项目**，请创建一个项目并将其添加到解决方案中。

![包项目模板](images/package-project-template.png)

有关 Windows 应用程序打包项目的详细信息，请参阅[使用 Visual Studio 打包你的应用程序](../desktop/desktop-to-uwp-packaging-dot-net.md)。

在**解决方案资源管理器**中，右键单击打包项目，选择 "**编辑**"，然后将其添加到项目文件的底部：

```xml
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>为运行时修复添加项目

将 c + +**动态链接库（DLL）** 项目添加到解决方案。

![运行时修复库](images/runtime-fix-library.png)

右键单击该项目，然后选择 "**属性**"。

在属性页中，找到 " **c + + 语言标准**" 字段，然后在该字段旁边的下拉列表中，选择 " **ISO c + + 17 标准（/std： c + + 17）** " 选项。

![ISO 17 选项](images/iso-option.png)

右键单击该项目，然后在上下文菜单中，选择 "**管理 Nuget 包**" 选项。 确保 "**包源**" 选项设置为 "**全部**" 或 " **nuget.org**"。

单击 "设置" 图标旁边的字段。

搜索*PSF** Nuget 包，并将其安装到此项目中。

![NuGet 包](images/psf-package.png)

如果你想要调试或扩展现有的运行时修补程序，请添加你使用本指南的[查找运行时修补程序](#find)部分中所述的指导来获取的运行时修补文件。

如果你打算创建全新的修补程序，请不要将任何内容添加到此项目。 稍后在本指南中，我们将帮助你向此项目添加正确的文件。 现在，我们将继续设置您的解决方案。

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>添加启动 PSF 启动器可执行文件的项目

向解决方案中添加一个 c + +**空项目**项目。

![空项目](images/blank-app.png)

使用上一节中所述的相同指南，将**PSF** Nuget 包添加到此项目中。

打开该项目的属性页，然后在 "**常规**设置" 页中，将 "**目标名称**" 属性设置为 ``PSFLauncher32`` 或， ``PSFLauncher64`` 具体取决于应用程序的体系结构。

![PSF 启动器引用](images/shim-exe-reference.png)

将项目引用添加到解决方案中的运行时修复项目。

![运行时修复引用](images/reference-fix.png)

右键单击该引用，然后在 "**属性**" 窗口中应用这些值。

| 属性 | Value |
|-------|-----------|
| 复制本地 | True |
| 复制本地附属程序集 | True |
| 引用程序集输出 | True |
| 链接库依赖项 | False |
| 链接库依赖项输入 | False |

### <a name="configure-the-packaging-project"></a>配置打包项目

在打包项目中，右键单击**应用程序**文件夹，然后选择**添加引用**。

![添加项目引用](images/add-reference-packaging-project.png)

选择 PSF 启动器项目和桌面应用程序项目，然后选择 **"确定"** 按钮。

![桌面项目](images/package-project-references.png)

>[!NOTE]
> 如果你的应用程序没有源代码，只需选择 PSF 启动器项目即可。 当你创建配置文件时，我们将向你展示如何引用可执行文件。

在 "**应用程序**" 节点中，右键单击 PSF 启动器应用程序，然后选择 "**设置为入口点**"。

![设置入口点](images/set-startup-project.png)

向打包项目添加一个名为的文件 ``config.json`` ，然后将以下 json 文本复制并粘贴到该文件中。 将 "**包操作**" 属性设置为 "**内容**"。

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "fixups": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```

为每个键提供一个值。 使用此表作为指南。

| Array | 键 | Value |
|-------|-----------|-------|
| applications | id |  使用 `Id` `Application` 包清单中元素的属性的值。 |
| applications | 可执行文件 | 要启动的可执行文件的包相对路径。 在大多数情况下，你可以在修改之前从包清单文件中获取此值。 它是元素的属性的值 `Executable` `Application` 。 |
| applications | workingDirectory | 可有可无要用作启动的应用程序的工作目录的包相对路径。 如果未设置此值，操作系统将使用 `System32` 目录作为应用程序的工作目录。 |
| 进程 | 可执行文件 | 在大多数情况下，这将是上面配置的名称， `executable` 其中包含已删除的路径和文件扩展名。 |
| 修正 | dll | 要加载的链接地址 DLL 的包相对路径。 |
| 修正 | config | 可有可无控制修正 DLL 的行为方式。 此值的准确格式因修正链接而异，因为每个修正都可以根据需要解释此 "blob"。 |

完成后， ``config.json`` 文件将如下所示。

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`、 `processes` 和 `fixups` 键是数组。 这意味着，可以使用文件 config.js来指定多个应用程序、进程和修复 DLL。

### <a name="debug-a-runtime-fix"></a>调试运行时修补程序

在 Visual Studio 中，按 F5 启动调试器。  第一件事是 PSF 启动器应用程序，该应用程序随后会启动目标桌面应用程序。  若要调试目标桌面应用程序，必须通过选择 "**调试"->"附加到进程**"，然后选择应用程序进程，手动附加到桌面应用程序进程。 若要允许使用本机运行时修复 DLL 调试 .NET 应用程序，请选择 "托管和本机代码类型（混合模式调试）"。  

完成此设置后，可以在桌面应用程序代码和运行时修复项目中的代码行旁边设置断点。 如果你的应用程序没有源代码，则只能在运行时修复项目中的代码行旁边设置断点。

由于 F5 调试通过从包布局文件夹路径部署松散文件来运行应用程序，而不是从 .msix/.appx 包进行安装，因此布局文件夹通常与已安装的包文件夹具有相同的安全限制。 因此，在应用运行时修补程序之前，可能无法重现包路径访问拒绝错误。

若要解决此问题，请使用 .msix/.appx 包部署，而不是按 F5 松散文件部署。  如前文所述，若要创建 .msix/.appx 包文件，请使用 Windows SDK 中的[makeappx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)实用程序。 或者，在 Visual Studio 中，右键单击应用程序项目节点，然后选择 "**存储-> 创建应用包**"。

Visual Studio 的另一个问题是，它没有为附加到调试器启动的任何子进程提供内置支持。   这使得难以在目标应用程序的启动路径中调试逻辑，必须在启动后由 Visual Studio 手动附加。

若要解决此问题，请使用支持子进程附加的调试程序。  请注意，通常不能将实时（JIT）调试器附加到目标应用程序。  这是因为大多数 JIT 技术都涉及到通过 ImageFileExecutionOptions 注册表项启动调试器来代替目标应用。  这会使 PSFLauncher.exe 将 FixupRuntime.dll 注入到目标应用中，这种机制将违背 detouring 机制。  WinDbg 包含在[Windows 调试工具](https://docs.microsoft.com/windows-hardware/drivers/debugger/index)中，并从[Windows SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)获取，它支持子进程附加。  它现在还支持直接[启动和调试 UWP 应用](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)。

若要将目标应用程序启动调试为子进程，请启动 ``WinDbg`` 。

```powershell
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

在 ``WinDbg`` 提示符下，启用子调试并设置相应的断点。

```powershell
.childdbg 1
g
```

（在目标应用程序启动并中断调试器之前执行）

```powershell
sxe ld fixup.dll
g
```

（在加载修复 DLL 之前执行）

```powershell
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/windows-hardware/drivers/debugger/plmdebug)也可用于在启动时将调试程序附加到应用程序，并且也包含在[Windows 调试工具](https://docs.microsoft.com/windows-hardware/drivers/debugger/index)中。  不过，使用的直接支持比 WinDbg 提供的直接支持更复杂。

## <a name="support"></a>支持

有问题？ 请在 .MSIX 技术社区网站上的 "[包支持框架](https://techcommunity.microsoft.com/t5/Package-Support-Framework/bd-p/Package-Support)" 会话空间询问我们。
