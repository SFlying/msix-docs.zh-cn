---
title: 使用命令行接口创建包
description: 本文介绍如何使用 .MSIX 打包工具的命令行接口创建 .MSIX 包。
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5206c4e90ca780c80e1ac6dd4e3ae33aa6c4c0e0
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328450"
---
# <a name="conversion-with-command-line-interface-cli"></a>使用命令行接口 (CLI) 进行转换

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

若要为应用程序创建新的 .MSIX 包，请在管理员命令提示符窗口中运行 `MsixPackagingTool.exe create-package` 命令。

下面是可以作为命令行自变量传递的参数：

|**参数** |    **描述**|
|---------|---------|
|-? --help  |显示帮助信息|
|--template | [必需] 转换模板 XML 文件的路径，该文件包含用于此次转换的包信息和设置|
|--virtualMachinePassword   | [可选] 转换环境使用的虚拟机的密码。 注意：模板文件必须包含 VirtualMachine 元素，并且 Settings：： AllowPromptForPassword 特性不得设置为 true。|
|-v --verbose   |[可选] 在控制台中输出详细日志。|

示例：

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> App-V 5.x 目前受支持，可以通过命令行转换。 其中包括功能。 

**转换模板文件**

``` xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018">

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
    EnforceMicrosoftStoreVersioningRequirements="false">
        
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
        
    </Settings>

    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute and is optional
    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService ="true"/>
    -->

    <SaveLocation
    PackagePath="C:\users\user\Desktop\MyPackage.msix" 
    TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />
    
    
    <!--NOTE: This section specifies that the conversion will be run on a local Virtual Machine.
    <VirtualMachine Name="vmname" Username="vmusername" />
    -->

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">
        
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

**转换模板参数参考**

下面是可以在转换模板文件中使用的参数的完整列表。

|**ConversionSettings** |   **描述** |
|---------|---------|
|Settings：： AllowTelemetry |        [可选] 为此工具调用启用遥测日志记录。|
|Settings：： ApplyAllPrepareComputerFixes     |  [可选] 应用所有建议的“准备计算机”修复程序。 使用其他特性时不能设置此参数。|
|Settings：： GenerateCommandLineFile  |  [可选] 将模板文件输入复制到 SaveLocation 目录，供将来使用。|
|Settings：： AllowPromptForPassword |        [可选] 指示工具提示用户输入虚拟机和签名证书的密码（如果必须提供密码，但未指定密码）。|
|Settings：： EnforceMicrosoftStoreVersioningRequirements |       [可选] 指示工具强制实施从 Microsoft Store 和适用于企业的 Microsoft Store 进行部署所需的包版本控制方案。|
|ExclusionItems |       [可选] 0 个或更多 FileExclusion 或者 RegistryExclusion 元素。 所有 FileExclusion 元素必须出现在任何 RegistryExclusion 元素之前。|
|ExclusionItems::FileExclusion |        [可选] 不要打包的文件。|
|ExclusionItems::FileExclusion::ExcludePath |       不要打包的文件的路径。|
|ExclusionItems::RegistryExclusion |        [可选] 不要打包的注册表项。|
|ExclusionItems::RegistryExclusion:: ExcludePath |      不要打包的注册表的路径。|
|PrepareComputer::DisableDefragService |        [可选] 转换应用时禁用 Windows 碎片整理程序。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|PrepareComputer:: DisableWindowsSearchService |        [可选] 转换应用时禁用 Windows Search。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|PrepareComputer:: DisableSmsHostService     |  [可选] 转换应用时禁用 SMS 主机。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|PrepareComputer:: DisableWindowsUpdateService   |  [可选] 转换应用时禁用 Windows 更新。 如果设置为 false，则会重写 ApplyAllPrepareComputerFixes。|
|SaveLocation     |[可选] 用于指定工具保存位置的元素。 如果未指定，包将保存在“桌面”文件夹中。         |
|SaveLocation::PackagePath     |[可选] 生成的 MSIX 包要保存到的文件或文件夹的路径。         |
|SaveLocation::TemplatePath    |[可选] 生成的 CLI 模板要保存到的文件或文件夹的路径。    |
|Installer::Path |      应用程序安装程序的路径。|
|Installer::Arguments |     [可选] 要传递给安装程序的参数。  该工具将使用参数“/qn /norestart INSTALLSTARTMENUSHORTCUTS=1 DISABLEADVTSHORTCUTS=1”以无提示方式自动运行 MSI 安装程序。 注意：如果你使用的是 .exe 安装程序，则必须传递参数以强制自动运行安装程序。|
|Installer::InstallLocation |       可有可无如果安装的文件已安装（例如 "C:\Program Files （x86） \MyAppInstalllocation"），则为该应用程序的根文件夹的完整路径。|
|VirtualMachine |       [可选] 用于指定要在本地虚拟机上运行转换的元素。|
|VrtualMachine::Name     |  转换环境使用的虚拟机的名称。|
|VirtualMachine::Username |     [可选] 转换环境使用的虚拟机的用户名。|
|PackageInformation::PackageName |      MSIX 包的名称。|
|PackageInformation::PackageDisplayName |       MSIX 包的显示名称。|
|PackageInformation::PublisherName |        MSIX 包的发布者。|
|PackageInformation::PublisherDisplayName |     MSIX 包的发布者显示名称。|
|PackageInformation::Version |      MSIX 包的版本号。|
|PackageInformation:: MainPackageNameForModificationPackage |       [可选] 主包名称的包标识名称。 创建依赖于主（父）应用程序的修改包时，将使用此参数。|
|应用程序 |     [可选] 用于在 MSIX 包中配置应用程序条目的 0 个或多个 Application 元素。|
|Application::Id |      MSIX 应用程序的应用 ID。 此 ID 将用于检测到的、与指定的 ExecutableName 匹配的应用程序条目。 可为包中的可执行文件提供多个应用程序 ID 值。<br/><br/>此值是包中应用程序的唯一标识符。 此值有时称作“包相对应用标识符”(PRAID)。 该 ID 必须在包中唯一（同一个 ID 不能在同一个包中多次使用）。 但是，该 ID 不一定全局唯一。 系统中可以有另一个包使用相同的 ID。<br/><br/>此字符串包含句点分隔的字母数字字段。 每个字段必须以 ASCII 字母字符开头。 不能将它们用作字段值： "CON"、"PRN"、"AUX"、"NUL"、"COM1"、"COM2"、"COM3"、"COM4"、"COM5"、"COM6"、"COM7"、"COM8"、"COM9"、"LPT1"、"LPT2"、"LPT3"、"LPT4"、"LPT5"、"LPT6"、"LPT7"、"LPT8"、"LPT9" 和 ""。|
|Application::ExecutableName |      要添加到包清单的 MSIX 应用程序的可执行文件名称。 如果检测不到具有此名称的应用程序，则会忽略相应的应用程序条目。|
|Application::Description |     [可选] MSIX 应用程序的应用说明。 如果不使用此参数，将使用应用程序显示名称。 此说明将用于检测到的、与指定的 ExecutableName 匹配的应用程序条目|
|Application::DisplayName    |  MSIX 包的应用显示名称。 此显示名称将用于检测到的、与指定的 ExecutableName 匹配的应用程序条目|
|功能 |     [可选] 用于将自定义功能添加到 MSIX 包的 0 个或多个 Capability 元素。 在转换期间，默认会添加“runFullTrust”功能。|
|Capability::Name | 添加到 MSIX 包的功能。

