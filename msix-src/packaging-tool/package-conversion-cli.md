---
title: 使用命令行接口创建包
description: 了解如何创建 MSIX 包使用命令行接口。
author: mcleanbyron
ms.author: mcleans
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7dcd522a1c1b79fe59a5b6e1fd04c7aec19ed419
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566501"
---
# <a name="conversion-with-command-line-interface-cli"></a>使用命令行接口 (CLI) 转换

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>
      
若要创建新的 MSIX 程序包为您的应用程序，请在管理员命令提示符窗口中运行 MsixPackagingTool.exe 创建包命令。 

下面是可以作为命令行参数传递的参数：

|**参数** |    **说明**|
|---------|---------|
|-? --help  |显示帮助信息|
|--template | [必需] 包含包的信息和设置此转换的转换模板 XML 文件路径|
|--virtualMachinePassword   | [可选]虚拟机要用于转换环境的密码。 注意：模板文件必须包含 VirtualMachine 元素和 Settings::AllowPromptForPassword 属性不能设置为 true。|
|-v --verbose   |[可选]打印到控制台的详细日志。|

示例：

' 命令提示符

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```
> [!NOTE]
> App-V 5.x conversion is currently supported to be converted throught the command line. This includes capabilities. 

**Conversion template file**

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

**转换模板参数引用**

下面是可以在转换模板文件中使用的参数的完整列表。

|**ConversionSettings** |   **说明** |
|---------|---------|
|设置：：AllowTelemetry |        [可选]启用该工具的此调用的遥测日志记录。|
|设置：：ApplyAllPrepareComputerFixes     |  [可选]应用所有建议准备计算机的修补程序。 可以使用其他属性时，不能设置。|
|设置：：GenerateCommandLineFile  |  [可选]将复制到 SaveLocation 目录以供将来使用的模板文件输入。|
|设置：：AllowPromptForPassword |        [可选]指示工具提示用户输入密码为虚拟机和为签名证书 （如果它为所需，且未指定。|
|设置：：EnforceMicrosoftStoreVersioningRequirements |       [可选]指示该工具以强制实施从 Microsoft Store 和于企业的 Microsoft Store 部署所需的包版本控制方案。|
|ExclusionItems |       [可选] 0 个或更多 FileExclusion 或 RegistryExclusion 元素。 所有 FileExclusion 元素必须都出现在任何 RegistryExclusion 元素之前。|
|ExclusionItems::FileExclusion |        [可选]要打包从中排除的文件。|
|ExclusionItems::FileExclusion::ExcludePath |       若要排除的打包的文件路径。|
|ExclusionItems::RegistryExclusion |        [可选]注册表项以排除用于打包。|
|ExclusionItems::RegistryExclusion::ExcludePath |      注册表中为包中排除的路径。|
|PrepareComputer::DisableDefragService |        [可选]在转换应用程序时禁用 Windows 碎片整理程序。 如果设置为 false，将替代 ApplyAllPrepareComputerFixes。|
|PrepareComputer::DisableWindowsSearchService |        [可选]在转换应用程序时禁用 Windows Search。 如果设置为 false，将替代 ApplyAllPrepareComputerFixes。|
|PrepareComputer::DisableSmsHostService     |  [可选]在转换应用程序时禁用 SMS 主机。 如果设置为 false，将替代 ApplyAllPrepareComputerFixes。|
|PrepareComputer::DisableWindowsUpdateService   |  [可选]在转换应用程序时禁用 Windows 更新。 如果设置为 false，将替代 ApplyAllPrepareComputerFixes。|
|SaveLocation     |[可选]元素指定保存工具的位置。 如果未指定，包将保存在桌面文件夹中。         |
|SaveLocation::PackagePath     |[可选]文件或保存生成的 MSIX 包文件夹的路径。         |
|SaveLocation::TemplatePath    |[可选]文件或保存生成的 CLI 模板文件夹的路径。    |
|Installer::Path |      指向应用程序安装程序的路径。|
|Installer::Arguments |     [可选]要传递给安装程序的参数。  该工具将自动运行 MSI 安装程序以无提示方式使用参数"/qn /norestart INSTALLSTARTMENUSHORTCUTS = 1 DISABLEADVTSHORTCUTS = 1"。 注意：你必须传递要强制安装程序，以无提示运行，如果您使用的.exe 安装程序的参数。|
|Installer::InstallLocation |       [可选]如果它已安装 （例如已安装的文件的应用程序的根文件夹的完整路径"C:\Program 文件 (x86) \MyAppInstalllocation")。|
|VirtualMachine |       [可选]要指定将本地虚拟机上运行转换的元素。|
|VrtualMachine::Name     |  要用于转换环境的虚拟机的名称。|
|VirtualMachine::Username |     [可选]要用于转换环境的虚拟机的用户名。|
|PackageInformation::PackageName |      你 MSIX 包的包名称。|
|PackageInformation::PackageDisplayName |       MSIX 包包显示名称。|
|PackageInformation::PublisherName |        MSIX 包的的发布者。|
|PackageInformation::PublisherDisplayName |     MSIX 包发布者显示名称。|
|PackageInformation::Version |      MSIX 包版本的版本号。|
|PackageInformation::MainPackageNameForModificationPackage |       [可选]包标识主要包名称的名称。 创建依赖于主 （父） 应用程序的修改包时，这使用。|
|应用程序 |     [可选] 0 个或更多的应用程序元素在 MSIX 包中配置应用程序条目。|
|Application::Id |      你 MSIX 应用程序的应用程序 ID。 此 ID 将用于为匹配指定的 ExecutableName 检测到的应用程序条目。 您可以对可执行文件的多个应用程序 ID 值在包中。<br/><br/>此值是应用程序的包中的唯一标识符。 此值有时称为包相对应用程序标识符 (PRAID)。 ID 必须是唯一的包 （相同的 ID 不能使用超过一次在同一个包中） 中。 但是，该 ID 不能唯一全局范围内。 使用相同的 id。 在系统上可能有另一个包<br/><br/>此字符串包含用句点分隔的字母数字字段。 每个字段必须以 ASCII 字母字符开头。 不能使用它们作为字段值："CON"、"PRN"、"AUX"、"NUL"、"COM1"、"COM2"、"COM3"、"COM4"、"COM5"、"COM6"、"COM7"、"COM8"、"COM9"、"LPT1"、"LPT2"、"LPT3"、"LPT4"、"LPT5"、"LPT6"、"LPT7"、"LPT8"和"LPT9"。|
|Application::ExecutableName |      将添加到包清单 MSIX 应用程序的可执行文件名称。 如果检测不到具有此名称的任何应用程序，相应的应用程序条目将被忽略。|
|Application::Description |     [可选]MSIX 应用程序应用程序说明。 如果未使用，将使用应用程序显示名称。 此说明将使用为应用程序项检测到的匹配指定的 ExecutableName|
|Application::DisplayName    |  MSIX 包应用显示名称。 此显示名称将用于为应用程序项检测到的匹配指定的 ExecutableName|
|功能 |     [可选] 0 个或更多的功能元素，将自定义功能添加到 MSIX 包。 "runFullTrust"功能默认情况下添加在转换过程。|
|Capability::Name | 添加到 MSIX 包的功能。

