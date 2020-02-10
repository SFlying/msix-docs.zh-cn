---
title: 如何生成用于命令行转换的模板文件
description: 本文介绍了如何为命令行转换生成模板文件。
ms.date: 01/28/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 9e5fd913de5e1c057e335c8b3102a14a9b75adf3
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073503"
---
# <a name="how-to-generate-a-template-file-for-command-line-conversions"></a>如何生成用于命令行转换的模板文件

使用 .MSIX 打包工具，可以通过两种方式执行转换：通过交互 UI 或通过我们的命令行选项。 使用命令行时，需要提供模板文件，以便该转换可用于特定的设置和需求。 本文将指导你完成生成适用于你的模板文件的过程。

可以通过两种方式获取适用于你的模板文件：

* 可以使用 .MSIX 打包工具的 UI。 在工具设置中，可以指定要使用创建的每个 .MSIX 包生成转换模板文件。
* 您可以获取一个[示例模板](#sample-conversion-template-file)，并手动输入每个转换所需的配置。

## <a name="generate-a-conversion-template-file-from-the-msix-packaging-tool"></a>从 .MSIX 打包工具生成转换模板文件

- 启动 .MSIX 打包工具
- 在应用程序的右上角中转到 "设置"
- 确保 "为每个包生成转换模板文件"
- 对所需的设置进行其他更改或修改（例如排除项、退出代码）
- 保存设置

- 使用安装程序浏览应用程序包工作流
    - 如果未选择安装程序，则将无法生成转换模板文件
    - 如果你使用的是 exe，则需要将无提示标志传递到安装程序以生成转换模板文件
- 转换结束时，你将拥有一个模板文件，该文件是根据你选择的安装程序配置的，以及你现在可以重新用于未来转换的当前设置
    - 默认情况下，转换模板文件将保存在与 .MSIX 包相同的位置，但你可以在 "创建包" 页上为模板文件指定单独的保存位置
    - 你仍需要根据要在每次转换结束时输出的 .MSIX 进行一些修改。


## <a name="manually-edit-the-conversion-template-file"></a>手动编辑转换模板文件

你可以手动编辑转换模板文件的模板参数，以生成适合你的模板文件。 生成转换模板文件时，请注意要添加到模板文件中的功能，因为某些功能可能需要其他架构引用才能正常工作。

### <a name="conversion-template-parameter-reference"></a>转换模板参数引用

下面是可以在转换模板文件中使用的参数的完整列表。

|**ConversionSettings** |   **描述** |
|---------|---------|
|Settings：： AllowTelemetry | [可选] 为此工具调用启用遥测日志记录。|
|Settings：： ApplyAllPrepareComputerFixes | [可选] 应用所有建议的“准备计算机”修复程序。 使用其他特性时不能设置此参数。|
|Settings：： GenerateCommandLineFile | [可选] 将模板文件输入复制到 SaveLocation 目录，供将来使用。|
|Settings：： AllowPromptForPassword | [可选] 指示工具提示用户输入虚拟机和签名证书的密码（如果必须提供密码，但未指定密码）。|
|Settings：： EnforceMicrosoftStoreVersioningRequirements | [可选] 指示工具强制实施从 Microsoft Store 和适用于企业的 Microsoft Store 进行部署所需的包版本控制方案。|
|Settings：： ServerPortNumber| 可有可无连接到远程计算机时使用。 需要模板架构的 v2。|
|Settings：： AddPackageIntegrity| 可有可无将包完整性添加到每个生成的 .MSIX 中。 需要模板架构的 v5。 |
|ValidInstallerExitCodes | [可选] 0 个或多个 ValidInstallerExitCode 元素。 需要模板架构的 v2。 |
|ValidInstallerExitCodes:: ValidInstallerExitCode | 可有可无指定该工具可能不熟悉或需要重新启动的任何安装程序退出代码。 需要模板架构的 v2。 |
|ValidInstallerExitCodes：： ValidInstallerExitCode：： Reboot | 可有可无指定在转换过程中退出代码是否应触发重新启动。 需要模板架构的 v3。 |
|ExclusionItems | [可选] 0 个或更多 FileExclusion 或者 RegistryExclusion 元素。 所有 FileExclusion 元素必须出现在任何 RegistryExclusion 元素之前。|
|ExclusionItems::FileExclusion | [可选] 不要打包的文件。|
|ExclusionItems::FileExclusion::ExcludePath | 不要打包的文件的路径。|
|ExclusionItems::RegistryExclusion | [可选] 不要打包的注册表项。|
|ExclusionItems::RegistryExclusion:: ExcludePath | 不要打包的注册表的路径。|
|PrepareComputer::DisableDefragService | [可选] 转换应用时禁用 Windows 碎片整理程序。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|PrepareComputer:: DisableWindowsSearchService | [可选] 转换应用时禁用 Windows Search。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|PrepareComputer:: DisableSmsHostService | [可选] 转换应用时禁用 SMS 主机。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|PrepareComputer:: DisableWindowsUpdateService | [可选] 转换应用时禁用 Windows 更新。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|SaveLocation | [可选] 用于指定工具保存位置的元素。 如果未指定，包将保存在“桌面”文件夹中。 |
|SaveLocation::PackagePath | [可选] 生成的 MSIX 包要保存到的文件或文件夹的路径。 |
|SaveLocation::TemplatePath | 可有可无保存生成的命令行模板的文件或文件夹的路径。 |
|Installer::Path | 应用程序安装程序的路径。|
|Installer::Arguments | [可选] 要传递给安装程序的参数。  该工具将使用参数“/qn /norestart INSTALLSTARTMENUSHORTCUTS=1 DISABLEADVTSHORTCUTS=1”以无提示方式自动运行 MSI 安装程序。 注意：如果你使用的是 .exe 安装程序，则必须传递参数以强制自动运行安装程序。|
|Installer::InstallLocation | 可有可无如果安装的文件已安装（例如 "C:\Program Files （x86） \MyAppInstalllocation"），则为该应用程序的根文件夹的完整路径。|
|VirtualMachine | [可选] 用于指定要在本地虚拟机上运行转换的元素。|
|VirtualMachine：： Name | 要用于转换环境的虚拟机的名称。|
|VirtualMachine::Username | 用于转换环境的虚拟机的用户名。|
|RemoteMachine | 可有可无一个元素，用于指定将在远程计算机上运行转换。 需要模板架构的 v2。 |
|RemoteMachine：： ComputerName | 用于转换环境的远程计算机的名称。 需要模板架构的 v2。 |
|RemoteMachine：： Username | 用于转换环境的远程计算机的用户名。 需要模板架构的 v2。 |
|RemoteMachine：： EnableAutoLogon | 可有可无当执行需要在远程计算机上重启的转换以便无缝地继续转换时，这将自动登录回。 需要模板架构的 V3。 |
|PackageInformation::PackageName | MSIX 包的名称。|
|PackageInformation::PackageDisplayName | MSIX 包的显示名称。|
|PackageInformation::PublisherName | MSIX 包的发布者。|
|PackageInformation::PublisherDisplayName | MSIX 包的发布者显示名称。|
|PackageInformation::Version | MSIX 包的版本号。|
|PackageInformation：:P ackageDescription | 可有可无.MSIX 包的说明。 需要模板架构的 v4。 |
|PackageInformation:: MainPackageNameForModificationPackage | [可选] 主包名称的包标识名称。 创建依赖于主（父）应用程序的修改包时，将使用此参数。|
|SigningInformation| 可有可无一个元素，用于指定 Device Guard 签名的签名信息。 需要模板架构的 v4。 |
|SigningInformation:: DeviceGuardSigning | 可有可无用于指定设备保护签名信息的元素。 需要模板架构的 v4。 |
|DeviceGuardSigning:: TokenFile | 采用 JSON 格式进行设备保护签名[所需的 Azure AD 访问令牌](../package/signing-package-device-guard-signing.md#get-an-azure-ad-access-token)。 需要 v4 模板架构。 |
|DeviceGuardSigning:: TimestampUrl | 可有可无在使用 Device Guard 进行签名时提供时间戳，以确保你的应用程序将在证书的生存期之外安装。 需要模板架构的 v4。 |
|应用程序 | [可选] 用于在 MSIX 包中配置应用程序条目的 0 个或多个 Application 元素。|
|Application::Id | MSIX 应用程序的应用 ID。 此 ID 将用于检测到的、与指定的 ExecutableName 匹配的应用程序条目。 可为包中的可执行文件提供多个应用程序 ID 值。<br/><br/>此值是包中应用程序的唯一标识符。 此值有时称作“包相对应用标识符”(PRAID)。 该 ID 必须在包中唯一（同一个 ID 不能在同一个包中多次使用）。 但是，该 ID 不一定全局唯一。 系统中可以有另一个包使用相同的 ID。<br/><br/>此字符串包含句点分隔的字母数字字段。 每个字段必须以 ASCII 字母字符开头。 不能将它们用作字段值： "CON"、"PRN"、"AUX"、"NUL"、"COM1"、"COM2"、"COM3"、"COM4"、"COM5"、"COM6"、"COM7"、"COM8"、"COM9"、"LPT1"、"LPT2"、"LPT3"、"LPT4"、"LPT5"、"LPT6"、"LPT7"、"LPT8"、"LPT9" 和 ""。|
|Application::DisplayName | MSIX 包的应用显示名称。 此显示名称将用于检测到的、与指定的 ExecutableName 匹配的应用程序条目|
|Application::ExecutableName | 要添加到包清单的 MSIX 应用程序的可执行文件名称。 如果检测不到具有此名称的应用程序，则会忽略相应的应用程序条目。|
|Application::Description | [可选] MSIX 应用程序的应用说明。 如果不使用此参数，将使用应用程序显示名称。 此说明将用于检测到的、与指定的 ExecutableName 匹配的应用程序条目|
|功能 | [可选] 用于将自定义功能添加到 MSIX 包的 0 个或多个 Capability 元素。 在转换期间，默认会添加“runFullTrust”功能。|
|Capability::Name | 添加到 MSIX 包的功能。 |

### <a name="sample-conversion-template-file"></a>示例转换模板文件

```xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018"
    xmlns:V2="http://schemas.microsoft.com/msix/msixpackagingtool/template/1904"
    xmlns:V3="http://schemas.microsoft.com/msix/msixpackagingtool/template/1907"
    xmlns:V4="http://schemas.microsoft.com/msix/msixpackagingtool/template/1910"
    xmlns:V5="http://schemas.microsoft.com/msix/msixpackagingtool/template/2001">
<!--Note: You only need to include xmlns:v2 - xmlns:v5 if you are using one of the features that use those schemas -->

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
        EnforceMicrosoftStoreVersioningRequirements="false"
        v2:ServerPortNumber="1599"
        v5:AddPackageIntegrity="true">    

    <!--Note: Exclusion items are optional and if declared take precedence over the default tool exclusion items
        <ExclusionItems>
            <FileExclusion ExcludePath="[{CryptoKeys}]" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Crypto" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Search\Data" />
            <FileExclusion ExcludePath="[{Cookies}]" />
            <FileExclusion ExcludePath="[{History}]" />
            <FileExclusion ExcludePath="[{Cache}]" />
            <FileExclusion ExcludePath="[{Personal}]" />
            <FileExclusion ExcludePath="[{Profile}]\Local Settings" />
            <FileExclusion ExcludePath="[{Profile}]\NTUSER.DAT.LOG1" />
            <FileExclusion ExcludePath="[{Profile}]\ NTUSER.DAT.LOG2" />
            <FileExclusion ExcludePath="[{Recent}]" />
            <FileExclusion ExcludePath="[{Windows}]\debug" />
            <FileExclusion ExcludePath="[{Windows}]\Logs\CBS" />
            <FileExclusion ExcludePath="[{Windows}]\Temp" />
            <FileExclusion ExcludePath="[{Windows}]\WinSxS\ManifestCache" />
            <FileExclusion ExcludePath="[{Windows}]\WindowsUpdate.log" />
        <FileExclusion ExcludePath="[{Windows}]\Installer" />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\$Recycle.Bin " />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\System Volume Information" />
        <FileExclusion ExcludePath="[{AppVPackageDrive}]\Config.Msi" />
            <FileExclusion ExcludePath="[{AppData}]\Microsoft\AppV" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Antimalware" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Windows Defender" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Windows Defender" />
        <FileExclusion ExcludePath="[{ProgramFiles}]\WindowsApps" />
            <FileExclusion ExcludePath="[{Local AppData}]\Temp" />
        <FileExclusion ExcludePath="[{Local AppData}]\Microsoft\Windows" />
        <FileExclusion ExcludePath="[{Local AppData}]\Packages" />

            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware Setup" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Security Client" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\AppV" />
        </ExclusionItems>
    -->
    
    <!--Note: Specifying an installer exit code will allow you to automatically trigger a reboot during your conversion
      <v2:ValidInstallerExitCodes>
        <V2:ValidInstallerExitCode ExitCode="3010" V3:Reboot="true"/>
        <V2:ValidInstallerExitCode ExitCode="1641"/>
      </v2:ValidInstallerExitCodes>
    -->
        
    </Settings>

    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute and is optional
    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService="true"/>
    -->

    <SaveLocation
        PackagePath="C:\users\user\Desktop\MyPackage.msix" 
        TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />
    
    <!--NOTE: This section specifies that the conversion will be run on a local Virtual Machine. This is optional if you want to change your conversion environment from the default local machine.
    <VirtualMachine Name="vmname" Username="vmusername"/>
    -->

    <!--NOTE: This section specifies that the conversion will be run on a remote machine.This is optional if you want to change your conversion environment from the default local machine.
    <v2:RemoteMachine ComputerName="vmname" Username="vmusername" v3:EnableAutoLogon="true"/>
    -->

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">

    <!--Note: This is optional, if you want to sign your package with Device Guard signing
        <v4:SigningInformation>
            <v4:DeviceGuardSigning
                Tokenfile="tokenfile.json"
                TimestampUrl="https://mytimestamp.com"/>
        </v4:SigningInformation>
    -->
        
    <!--NOTE: This ID will be used if the Application entry detected matches the specified ExecutableName
        <Applications>
            <Application
                Id="MyApp1"
                Description="MyApp"
                DisplayName="My App"
                ExecutableName="MyApp.exe"/>
        </Applications>
    -->

    <!--NOTE: This is optional as “runFullTrust” capability is added by default during conversion
        <Capabilities>
            <Capability Name="runFullTrust" />
        </Capabilities>
    -->
        
    </PackageInformation>
</MsixPackagingToolTemplate>
```