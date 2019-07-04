---
Description: 运行 Desktop App Converter 以将 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）打包。
title: 使用 Desktop App Converter 将应用打包（桌面桥）
ms.date: 08/21/2018
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.localizationpriority: medium
ms.openlocfilehash: e52cb34795fbf3bf83c43ea9194a6b0ec8a25c00
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555560"
---
# <a name="package-a-desktop-application-using-the-desktop-app-converter"></a>包使用 Desktop App Converter 的桌面应用程序

> [!NOTE]
> Desktop App Converter 工具已被弃用。 我们建议你使用[MSIX 打包工具](../packaging-tool/create-app-package-msi-vm.md)相反。

![DAC 图标](images/dac.png)

Desktop App Converter (DAC) 创建包的桌面应用程序为与最新的 Windows 功能，包括分发和维护服务通过 Microsoft Store。 这包括 Win32 应用和使用 .NET 4.6.1 创建的应用。

[获取桌面应用转换器](https://aka.ms/converter)

尽管此工具名称中出现了“Converter”一词，但它实际上并不转换你的应用。 你的应用程序保持不变。 但是，此工具会生成一个带有应用包标识并能够调用各种 WinRT API 的 Windows 应用包。

可以通过使用 Add-AppxPackage PowerShell cmdlet 在开发计算机上安装该程序包。

转换器使用作为转换器下载的一部分提供的干净的基础映像在隔离的 Windows 环境中运行桌面安装程序。 它捕获桌面安装程序进行的任何注册表和文件系统 I/O，并将其作为输出的一部分打包。

> [!IMPORTANT]
> 在 Windows 10，版本 1607，及更高版本上支持 Desktop App Converter。 仅可在面向 Windows 10 周年更新 (10.0; 的项目Build 14393) 或更高版本在 Visual Studio 中的。

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>DAC 不仅能为你生成一个包

以下是它可以为你执行的其他一些操作。

**Windows 10 创意者更新**

:heavy_check_mark:自动注册预览控件、缩略图处理程序、属性处理程序、防火墙规则、URL 标志。

:heavy_check_mark:自动注册使用户能够使用“文件资源管理器”中的**种类**列对文件进行分组的文件类型映射。

:heavy_check_mark:注册你的公共 COM 服务器。

**Windows 10 周年更新或更高版本**

:heavy_check_mark:自动对程序包进行签名，使你能够测试你的应用。

:heavy_check_mark:验证针对打包的应用和 Microsoft Store 要求应用程序。

若要查找完整的选项列表，请参阅本指南的[参数](#command-reference)部分。

如果你已准备好创建应用包，那我们开始吧。

## <a name="first-prepare-your-application"></a>首先，准备应用程序

在开始创建你的应用程序的包之前，请查看本指南：[准备将桌面应用程序打包](desktop-to-uwp-prepare.md)。

## <a name="make-sure-that-your-system-can-run-the-converter"></a>请确保你的系统可以运行转换器

请确保你的系统满足以下要求：

* Windows 10 周年更新（10.0.14393.0 和更高版本）专业版或企业版。
* 64 位 (x64) 处理器
* 硬件辅助虚拟化
* 二级地址转换 (SLAT)
* [适用于 Windows 10 的 Windows 软件开发工具包 (SDK)](https://go.microsoft.com/fwlink/?linkid=821375)。

## <a name="start-the-desktop-app-converter"></a>启动 Desktop App Converter

1. 下载并安装 [Desktop App Converter](https://aka.ms/converter)。

2. 以管理员身份运行 Desktop App Converter。

    ![以管理员身份运行 DAC](images/run-converter.png)

   会出现一个控制台窗口。 你需要使用该控制台窗口运行命令。

## <a name="set-a-few-things-up-apps-with-installers-only"></a>进行一些设置（仅限具有安装程序的应用）

你可以跳到下一部分如果你的应用程序不具有安装程序。

1. 识别操作系统的版本号。

   若要执行该操作，请在**运行**对话框中键入 **winver**，然后选择**确定**按钮。

   ![winver](images/winver.png)

   你可以在**关于 Windows** 对话框中找到你的 Windows 版本。

   ![Windows 10 版本](images/win-10-version.png)

2. 下载相应的 [Desktop App Converter 基础映像](https://aka.ms/converterimages)。

   确保文件名中显示的版本号与 Windows 内部版本的版本号相匹配。

   >[!IMPORTANT]
   > 如果使用的内部版本号**15063**，并且该生成的次版本等于或大于 **.483** (例如：**15063.540**)，请确保下载**BaseImage 15063 UPDATE.wim**文件。 如果该版本的次要版本小于 **.483**，请下载 **BaseImage-15063.wim** 文件。 如果已设置此基文件的不兼容版本，则可以解决此问题。 这篇[博客文章](https://blogs.msdn.microsoft.com/appconsult/2017/08/04/desktop-app-converter-fails-on-windows-10-15063-483-and-later-how-to-solve-it/)介绍了如何解决此问题。

3. 将下载的文件放置在你稍后可在计算机上找到它的任意位置。

4. 在启动 Desktop App Converter 时出现的控制台窗口中，运行此命令：```Set-ExecutionPolicy bypass```。
5. 通过运行此命令设置转换器：```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```。
6. 如果系统提示你重启计算机，请执行此操作。

   当转换器展开基础映像时，控制台窗口中会显示状态消息。 如果未看到任何状态消息，请按任意键。 此操作可以刷新控制台窗口的内容。

   ![控制台窗口中的状态消息](images/bas-image-setup.png)

   当基础映像完全展开后，请转至下一部分。

## <a name="package-an-app"></a>将应用打包

若要将应用打包，请在启动 Desktop App Converter 时打开的控制台窗口中运行 ``DesktopAppConverter.exe`` 命令。  

将使用参数来指定应用程序的包名称、 发布者和版本数。

> [!NOTE]
> 如果你已保留应用名称在 Microsoft Store 中的，你可以通过使用获取的包和发布服务器名称[合作伙伴中心](https://partner.microsoft.com/dashboard)。 如果你打算将应用旁加载到其他系统上，只要选择的发布者名称与用于对应用进行签名的证书上的名称相匹配，就可以提供自己的名称。

### <a name="a-quick-look-at-command-parameters"></a>命令参数概览

以下是所需参数。

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
可以在[此处](#command-reference)阅读有关每个参数的信息。

### <a name="examples"></a>示例

以下是将应用打包的一些常见方法。

* [包的应用程序的安装程序 (.msi) 文件](#installer-conversion)
* [包具有一个安装程序可执行文件的应用程序](#setup-conversion)
* [包不具有安装程序的应用程序](#no-installer-conversion)
* [打包应用程序，登录应用程序中，并准备将其提交到应用商店](#optional-parameters)

<a id="installer-conversion" />

#### <a name="package-an-application-that-has-an-installer-msi-file"></a>包的应用程序的安装程序 (.msi) 文件

使用 ``Installer`` 参数指向安装程序文件。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!IMPORTANT]
> 以下是在执行此操作时要记住的两个重要事项。 首先，请确保安装程序位于独立文件夹中，并确保只有与该安装程序相关的文件位于同一文件夹中。 转换器将该文件夹的所有内容复制到隔离的 Windows 环境中。 <br> 其次，如果合作伙伴中心将标识分配给您以数字开头的软件包，请确保还传递中<i>-AppId</i>参数，以及如何使用字符串后缀将 （之后的时间段分隔符） 作为该参数的值。  

如果你的安装程序包括相关库或框架的安装程序，则组织内容的方式可能略有不同。 请参阅[用桌面桥链连多个安装程序](https://blogs.msdn.microsoft.com/appconsult/2017/09/11/chaining-multiple-installers-with-the-desktop-app-converter/)。

<a id="setup-conversion" />

#### <a name="package-an-application-that-has-a-setup-executable-file"></a>包具有一个安装程序可执行文件的应用程序

使用 ``Installer`` 参数指向安装程序可执行文件。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>如果合作伙伴中心将标识分配给您以数字开头的软件包，请确保还传递中<i>-AppId</i>参数，以及如何使用字符串后缀将 （之后的时间段分隔符） 作为该参数的值。

``InstallerArguments`` 参数是可选参数。 但是，由于 Desktop App Converter 需要安装程序，以在无人参与模式下运行，您可能需要使用它，如果应用程序需要无提示要以无提示方式运行的标志。 ``/S`` 标志是十分常见的无提示标志，但你使用的标志可能有所不同，具体取决于用于创建安装程序文件的安装程序技术。

<a id="no-installer-conversion" />

#### <a name="package-an-application-that-doesnt-have-an-installer"></a>包不具有安装程序的应用程序

在此示例中，使用``Installer``参数以指向你的应用程序文件的根文件夹。

使用 `AppExecutable` 参数指向应用可执行文件。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>如果合作伙伴中心将标识分配给您以数字开头的软件包，请确保还传递中<i>-AppId</i>参数，以及如何使用字符串后缀将 （之后的时间段分隔符） 作为该参数的值。

<a id="optional-parameters" />

#### <a name="package-an-app-sign-the-app-and-run-validation-checks-on-the-package"></a>将应用打包、对应用进行签名并对应用包运行验证检查

此示例中是类似于第一个，但它显示的是如何进行本地测试应用程序进行签名，然后验证针对打包的应用和 Microsoft Store 要求应用程序。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```
>[!IMPORTANT]
>如果合作伙伴中心将标识分配给您以数字开头的软件包，请确保还传递中<i>-AppId</i>参数，以及如何使用字符串后缀将 （之后的时间段分隔符） 作为该参数的值。

``Sign``参数会生成一个证书，然后对其应用程序进行签名。 若要运行应用，你需要安装生成的证书。 若要了解如何操作，请参阅本指南的[运行已打包的应用](#run-app)部分。

您可以通过使用验证你应用程序``Verify``参数。

### <a name="a-quick-look-at-optional-parameters"></a>可选参数概览

``Sign`` 和 ``Verify`` 参数是可选参数。 还有更多的可选参数。  以下是一些更常用的可选参数。

```CMD
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]
```
可以在下一部分中阅读有关所有参数的信息。

<a id="command-reference" />

### <a name="parameter-reference"></a>参数引用

以下是运行 Desktop App Converter 时可使用的参数的完整列表（按类别列出）。

* [设置](#setup-params)
* [转换](#conversion-params)
* [包标识](#identity-params)
* [包清单](#manifest-params)
* [清理](#cleanup-params)
* [体系结构](#architecture-params)
* [其他](#other-params)

还可以通过在应用控制台窗口中运行 ``Get-Help`` 命令来查看整个列表。   

||||
|-------------|-----------|-------------|
|<a id="setup-params" /> <strong>安装程序参数</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |必需 |在设置模式下运行 DesktopAppConverter。 设置模式支持扩展所提供的基本映像。|
|-BaseImage &lt;String&gt; | 必需 |未扩展的基本映像的完整路径。 如果指定 -Setup，则需要此参数。|
| -LogFile &lt;String&gt; |可选 |指定日志文件。 如果省略，将创建一个日志文件临时位置。|
|-NatSubnetPrefix &lt;String&gt; |可选 |用于 Nat 实例的前缀值。 通常，仅在主机连接到与转换器的 NetNat 相同的子网范围时，你会希望更改此值。 你可以通过使用 **Get-NetNat** cmdlet 查询当前转换器 NetNat 配置。 |
|-NoRestart [&lt;SwitchParameter&gt;] |必需 |在运行设置时不要提示重启（需要重启才能启用容器功能）。 |
|<a id="conversion-params" /> <strong>转换参数</strong>|||
|-AppInstallPath &lt;String&gt;  |可选 |应用程序针对已安装文件（如果已安装）的根文件夹的完整路径（例如“C:\Program Files (x86)\MyApp”）。|
|-Destination &lt;String&gt; |必需 |如果转换器的 appx 输出的所需目标尚未存在，DesktopAppConverter 可以创建此位置。|
|-Installer &lt;String&gt; |必需 |适用于应用程序的安装程序的路径 - 必须能够在无人参与/静默的情况下运行。 否-安装程序转换，这是为应用程序文件的根目录的路径。 |
|-InstallerArguments &lt;String&gt; |可选 |用于强制安装程序在无人参与/无提示的情况下运行的参数的逗号分隔列表或字符串。 如果安装程序是 msi，则此参数为可选参数。 若要从安装程序中获取日志，请在此处为安装程序提供日志记录参数，并使用路径 &lt;log_folder&gt;，该路径是转换器使用相应路径所替换的标记。 <br><br>**注意**：无人参与/无提示标志和日志参数将在安装程序技术之间变化。 <br><br>此参数的用法示例:-InstallerArguments"/silent /log &lt;log_folder&gt;\install.log"不会生成一个日志文件的另一个示例可能如下所示：```-InstallerArguments "/quiet", "/norestart"``` 同样，必须按原义定向到的令牌的路径的任何日志&lt;log_folder&gt;如果你想要捕获它并将其放在最后一个日志文件夹中的转换器。|
|-InstallerValidExitCodes &lt;Int32&gt; |可选 |已成功运行的退出代码，以指示您的安装程序以逗号分隔列表 (例如：0, 1234, 5678).  默认情况下，对于非 msi，它为 0，对于 msi，它为 0, 1641, 3010。|
|-MakeAppx [&lt;SwitchParameter&gt;]  |可选 |一个告知此脚本对输出调用 MakeAppx 的开关（当存在时）。 |
|-MakeMSIX [&lt;SwitchParameter&gt;]  |可选 |开关指示，当存在时，此脚本以打包形式 MSIX 包的输出。 |
|<a id="identity-params" /><strong>包标识参数</strong>||
|-PackageName &lt;String&gt; |必需 |通用 Windows 应用包的名称。 如果合作伙伴中心将标识分配给您以数字开头的软件包，请确保还传递中<i>-AppId</i>参数，以及如何使用字符串后缀将 （之后的时间段分隔符） 作为该参数的值。 |
|-Publisher &lt;String&gt; |必需 |通用 Windows 应用程序包的发布者 |
|-Version &lt;Version&gt; |必需 |通用 Windows 应用程序包的版本号 |
|<a id="manifest-params" /><strong>包清单参数</strong>||
|-AppExecutable &lt;String&gt; |可选 |你的应用程序的主可执行文件的名称（“MyApp.exe”）。 无安装程序的转换需要使用此参数。 |
|-AppFileTypes &lt;String&gt;|可选 |应用程序将与其关联的文件类型的以逗号分隔的列表。 用法示例：-AppFileTypes "'.md', '.markdown'"。|
|-AppId &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将应用程序 ID 设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。 在许多情况下，使用 *PackageName* 很合适。 但是，如果合作伙伴中心将标识分配给您以数字开头的软件包，请确保还传递中<i>-AppId</i>参数，以及如何使用字符串后缀将 （之后的时间段分隔符） 作为该参数的值。 |
|-AppDisplayName &lt;String&gt;  |可选 |在 Windows 应用程序包清单中指定要将应用程序显示名称设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。 |
|-AppDescription &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将应用程序描述设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。|
|-PackageDisplayName &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将程序包显示名称设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。 |
|-PackagePublisherDisplayName &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将程序包发布者显示名称设置为的值。 如果未指定，则将其设置为 *Publisher* 传入的值。 |
|<a id="cleanup-params" /><strong>清除参数</strong>|||
|-Cleanup [&lt;Option&gt;] |必需 |为 DesktopAppConverter 项目运行清除。 清除模式有 3 个有效的选项。 |
|-Cleanup All | |删除所有已扩展的基本映像、删除任何临时转换器文件、删除容器网络并禁用可选的 Windows 功能，即容器。 |
|-Cleanup WorkDirectory |必需 |删除所有临时转换器文件。 |
|-Cleanup ExpandedImage |必需 |删除安装在主机上的所有已扩展的基本映像。 |
|<a id="architecture-params" /><strong>包体系结构参数</strong>|||
|-PackageArch &lt;String&gt; |必需 |生成指定了体系结构的程序包。 有效选项为“x86”或“x64”，例如 -PackageArch x86。 该参数为可选参数。 如果未指定，DesktopAppConverter 将尝试自动检测程序包体系结构。 如果自动检测失败，它将默认为 x64 程序包。 |
|<a id="other-params" /><strong>其他参数</strong>|||
|-ExpandedBaseImage &lt;String&gt;  |可选 |已扩展的基本映像的完整路径。|
|-LogFile &lt;String&gt;  |可选 |指定日志文件。 如果省略，将创建一个日志文件临时位置。 |
| -Sign [&lt;SwitchParameter&gt;] |可选 |出于测试目的，告知此脚本使用生成的证书对输出 Windows 应用包进行签名。 此开关应该位于开关 ```-MakeAppx``` 旁边。 |
|&lt;通用参数&gt; |必需 |此 cmdlet 支持通用参数：*详细*，*调试*， *ErrorAction*， *ErrorVariable*， *WarningAction*， *WarningVariable*， *outBuffer*， *PipelineVariable*，并且*OutVariable*。 有关详细信息，请参阅 [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)。 |
| -Verify [&lt;SwitchParameter&gt;] |可选 |一个开关，当存在时，指示要验证针对打包的应用和 Microsoft Store 要求的应用包的 DAC。 结果是一个“VerifyReport.xml”验证报告，该报告在浏览器中能够以最佳方式显示。 此开关应该位于开关 `-MakeAppx` 旁边。 |
|-PublishComRegistrations| 可选| 扫描你的安装程序进行的所有公共 COM 注册，并发布你的清单中有效的注册。 仅当想使这些注册可供其他应用程序使用时才使用此标志。 如果这些注册将仅由你的应用程序使用，则无需使用此标志。 <br><br>请查看[本文](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97)以确保在你将应用打包后，COM 注册会按预期工作。

<a id="run-app" />

## <a name="run-the-packaged-app"></a>运行打包的应用

可通过两种方法运行应用。

一种方法是打开 PowerShell 命令提示符，然后键入此命令：```Add-AppxPackage –Register AppxManifest.xml```。 它可能是最简单的方法来运行应用程序，因为无需对其进行签名。

另一种方法是应用程序使用证书进行签名。 如果使用```sign```参数，Desktop App Converter 将生成一个，然后与其应用程序进行签名。 该文件名为 **auto-generated.cer**，你可以在已打包应用的根文件夹中找到它。

请按照以下步骤安装生成的证书，然后运行应用。

1. 双击 **auto-generated.cer** 文件安装证书。

   ![生成的证书文件](images/generated-cert-file.png)

   > [!NOTE]
   > 如果系统提示你输入密码，请使用默认密码“123456”。

2. 在**证书**对话框中，选择**安装证书**按钮。
3. 在**证书导入向导**中，将证书安装到**本地计算机**上，并将证书放在**受信任人**证书存储区中。

   ![“受信任人”存储区](images/trusted-people-store.png)

5. 在已打包应用的根文件夹中，双击 Windows 应用包文件。

   ![Windows 应用包文件](images/windows-app-package.png)

6. 选择**安装**按钮以安装应用。

   ![“安装”按钮](images/install.png)


## <a name="modify-the-packaged-app"></a>修改已打包的应用

您可能会对打包的应用程序处理 bug、 添加视觉对象资产，或增强应用程序的现代化体验，例如动态磁贴进行更改。

进行更改之后，无需再次运行转换器。 在大多数情况下，您可以只使用重新打包你的应用程序 MakeAppx 工具并 appxmanifest.xml 文件 DAC 生成为你的应用。 请参阅[生成 Windows 应用包](desktop-to-uwp-manual-conversion.md#make-appx)。

* 如果修改了任何应用视觉资源，请生成新的包资源索引文件，然后再运行 MakeAppx 工具以生成新的包。 请参阅[生成包资源索引 (PRI) 文件](desktop-to-uwp-manual-conversion.md#make-pri)。

* 如果想要添加显示在 Windows 任务栏、任务视图、ALT+TB、贴靠助手和“开始”磁贴右下角上的图标或磁贴，请参阅[（可选）添加基于目标的未着色资源](desktop-to-uwp-manual-conversion.md#target-based-assets)。

> [!NOTE]
> 如果你对安装程序进行的注册表设置做出了更改，则需要再次运行 Desktop App Converter 以应用这些更改。

以下两个部分介绍几种封装的应用程序可能要考虑到的可选链接地址。

### <a name="delete-unnecessary-files-and-registry-keys"></a>删除不必要的文件和注册表项

Desktop App Converter 采用非常保守的方法来筛选掉容器中的文件和系统干扰。

如有需要，可以查看 VFS 文件夹并删除安装程序不需要的任何文件。  还可以查看 Reg.dat 的内容并删除应用未安装/不需要的任何项。

### <a name="fix-corrupted-pe-headers"></a>修复损坏的 PE 头

在转换过程中，DesktopAppConverter 将自动运行 PEHeaderCertFixTool 以修复任何损坏的 PE 头。 但是，还可以在 UWP Windows 应用包、松散文件或特定的二进制文件上运行 PEHeaderCertFixTool。 下面提供了一个示例。

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="telemetry-from-desktop-app-converter"></a>来自 Desktop App Converter 的遥测

Desktop App Converter 可以收集关于你和你使用该软件的情况的信息，并将此信息发送给 Microsoft。 你可以在产品文档和 [Microsoft 隐私声明](https://go.microsoft.com/fwlink/?LinkId=521839)中了解有关 Microsoft 的数据收集和使用的详细信息。 你同意遵守《Microsoft 隐私声明》的所有适用条款。

默认情况下，将为 Desktop App Converter 启用遥测。 添加以下注册表项以将遥测配置为所需设置：  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 通过使用设置为 1 的 DWORD 来添加或编辑 *DisableTelemetry* 值。
+ 若要启用遥测，请删除该项或将值设置为 0。

### <a name="language-support"></a>语言支持

Desktop App Converter 不支持 Unicode；因此，没有用于该工具的任何中文字符或非 ASCII 字符。

## <a name="known-issues-with-the-desktop-app-converter"></a>Desktop App Converter 的已知的问题

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED 和 E_STARTING_ISOLATED_ENV_FAILED 错误    

如果收到任一错误，请确保你正在使用的是从[下载中心](https://aka.ms/converterimages)下载的有效基础映像。
如果使用的是有效的基础映像，请尝试在命令中使用 ``-Cleanup All``。
如果不起作用，请将日志发送至 converter@microsoft.com，以帮助我们进行调查。

### <a name="new-containernetwork-the-object-already-exists-error"></a>新 ContainerNetwork:该对象已存在错误

设置新的基础映像时，可能会收到此错误。 如果你在以前安装了 Desktop App Converter 的开发人员计算机上安装有 Windows 预览体验计划外部测试版，则可能会出现这种情况。

若要解决此问题，请尝试从提升的命令提示符中运行命令 `Netsh int ipv4 reset`，然后重启计算机。

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>.NET 应用程序使用"AnyCPU"生成选项编译和安装失败

如果将主要可执行文件或任何依赖项放置在 **Program Files** 或 **Windows\System32** 文件夹层次结构中的任意位置，则可能会出现这种情况。

若要解决此问题，请尝试使用你体系结构特定的桌面安装程序（32 位或 64 位）生成 Windows 应用包。

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>发布公共并行的 Fusion 程序集不起作用。

 在安装期间，应用程序可以发布公共并行 Fusion 程序集，这些程序集可供任何其他进程访问。 在进程激活上下文创建期间，这些程序集将由名为 CSRSS.exe 的系统进程检索。 当已转换的进程执行此操作时，这些程序集的激活上下文创建和模块加载将失败。 并行的 Fusion 程序集注册在以下位置中：
  + 注册表： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 文件系统: %windir%\\SideBySide

这是一个已知限制，目前尚无解决方法。 即，内置程序集（如 ComCtl）随操作系统附带，因此依赖它们是安全的行为。

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>XML 中发现错误。 “Executable”特性无效 - 根据其数据类型，值“MyApp.EXE”无效

如果应用程序中的可执行文件具有大写的 **.EXE** 扩展名，则可能会出现这种情况。 虽然此扩展的大小写不应影响是否应用程序在运行，这可能会导致生成此错误的 DAC。

若要解决此问题，请尝试指定 **-AppExecutable**标志时包，并使用小写".exe"作为扩展的主可执行文件 (例如：MYAPP.exe)。    或者可以从小写到大写在应用程序中更改所有可执行文件的大小写 (例如： 从。EXE 为.exe)。

### <a name="corrupted-or-malformed-authenticode-signatures"></a>已损坏或格式不正确的验证码签名

本部分包含有关如何在可能包含已损坏或格式不正确的验证码签名的 Windows 应用包中标识可移植可执行 (PE) 文件的问题。 PE 文件中的无效验证码签名可能采用任何二进制格式（如 .exe、.dll、.chm 等），并且会阻止程序包正确签名，从而使其无法从 Windows 应用包部署。

PE 文件的验证码签名的位置由可选头数据目录中的证书表项和关联的属性证书表指定。 在签名验证期间，这些结构中指定的信息用于找到 PE 文件上的签名。 如果这些值损坏，则文件可能看起来无法有效进行签名。

若要使验证码签名正确，验证码签名必须符合以下情况：

- PE 文件中 **WIN_CERTIFICATE** 项的开头不得超过可执行文件的末尾
- **WIN_CERTIFCATE** 项应位于映像的末尾
- **WIN_CERTIFICATE** 项的大小必须为正数
- 对于 32 位可执行文件，**WIN_CERTIFICATE** 项必须在 **IMAGE_NT_HEADERS32** 结构之后开始，对于 64 位可执行文件，必须在 IMAGE_NT_HEADERS64 结构之后开始

有关更多详细信息，请参考[验证码门户可执行文件规范](https://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx)和 [PE 文件格式规范](https://msdn.microsoft.com/windows/hardware/gg463119.aspx)。

请注意，当尝试对 Windows 应用包进行签名时，SignTool.exe 可能会输出已损坏或格式不正确的二进制文件列表。 若要执行此操作，请通过将环境变量 APPXSIP_LOG 设置为 1（如 ```set APPXSIP_LOG=1```）来启用详细日志记录并重新运行 SignTool.exe。

若要修复这些格式不正确的二进制文件，请确保它们符合上述要求。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**运行应用程序 / 查找并修复问题**

请参阅[运行、 调试和测试打包桌面应用程序](desktop-to-uwp-debug.md)

**将应用分发**

请参阅[分发打包桌面应用程序](/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
