---
title: .MSIX 大容量转换脚本
description: 本文提供了有关 .MSIX 工具包中的批量转换脚本的详细信息。
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10，.msix，msixtoolkit，.msix 工具包，工具包，批处理，转换，批量，大容量转换
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b2af803cc540d393703e68569d8da1a78d478f2d
ms.sourcegitcommit: 6243b7aca6f52f007f4571c835f580f433c31769
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/16/2020
ms.locfileid: "84812792"
---
# <a name="msix-bulk-conversion-scripts"></a>.MSIX 大容量转换脚本

.MSIX 工具包中的[批量转换脚本](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion)可用于自动将 Windows 应用转换为 .msix 包格式。 **entry.ps1**脚本中提供了应用及其详细信息的列表。

## <a name="prepare-machines-for-conversion"></a>为转换准备计算机
在运行 .MSIX 工具包的批量转换脚本之前，若要自动将你的应用程序转换为 .MSIX 打包格式，你将使用的设备（虚拟或远程）必须配置为允许远程通信，并安装 .MSIX 打包工具。

| 术语            | 说明                                                        |
|-----------------|--------------------------------------------------------------------|
| 主机    | 这是执行大容量转换脚本的设备。          |
| 虚拟机 | 这是 Hyper-v 中的现有设备，托管在主机上。  |
| 远程计算机  | 这是可通过网络访问的物理或虚拟机。 |

### <a name="host-machine"></a>主机
主计算机必须满足以下要求：
* 必须安装[.Msix 打包工具](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab)。
* 如果正在使用虚拟机，则必须安装 Hyper-v。
* 如果正在使用远程计算机：
    * 设备与远程计算机位于同一域中：
        * 启用 PowerShell 远程处理 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            ```
    * 设备存在于工作组中或作为远程计算机的备用域：
        * 启用 PowerShell 远程处理 
        * WinRM 受信任的主机必须包含远程计算机的设备名称或 IP 地址 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            Set-Item WSMan:\localhost\Client\TrustedHosts -Value <RemoteMachineName>,[<RemoteMachineName>,...]
            ```

### <a name="remote-machine"></a>远程计算机
远程计算机必须满足以下要求：
* 必须安装[.Msix 打包工具](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab)。
* 如果设备与主机位于同一域中：
    * 启用 PowerShell 远程处理 
    * 必须启用 WinRM 
    * 允许通过客户端防火墙执行 ICMPv4
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        ```

* 如果设备在工作组内或备用域中存在，则为主机：
    * 启用 PowerShell 远程处理 
    * WinRM 受信任的主机必须包含主机计算机的设备名称或 IP 地址
    * 允许通过客户端防火墙执行 ICMPv4
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        Set-Item WSMan:\localhost\Client\TrustedHosts -Value <HostMachineName>
        
        ```

### <a name="virtual-machine"></a>虚拟机
建议使用 Hyper-v 快速创建 ".MSIX 打包工具环境" 映像，因为它已预先配置为满足所有要求。 虚拟机必须在主机上托管，并在 Microsoft Hyper-V 中运行。

虚拟机必须满足以下要求：
* 必须安装[.Msix 打包工具](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab)。

## <a name="syntax"></a>语法

```powershell
entry.ps1
```

## <a name="description"></a>说明

这是一组 PowerShell 脚本，提供将应用程序批量打包为 .MSIX 包格式的功能。 这些脚本将连接到用于打包每个应用程序的本地虚拟机或远程计算机。

将按照在**entry.ps1**脚本中输入的顺序来转换打包为 .msix 应用程序格式的应用。 将使用**entry.ps1**脚本中列出的远程计算机将应用程序打包为 .msix 格式。 可以多次使用虚拟机将不同的应用程序打包为 .MSIX 应用程序格式。

在运行该脚本之前，必须先将要转换的应用添加到 `conversionsParameters` 脚本中的变量。 可以将多个应用添加到变量。 该脚本利用应用和远程/虚拟机来创建一个 XML 文件，并将其设置为符合[.Msix 打包工具](..\packaging-tool\mpt-overview.md)（MsixPackagingTool.exe）的要求。 创建 XML 文件后，在目标设备上执行 MsixPackagingTool.exe 来转换应用并将其放在位于脚本执行文件夹中的 **.\Out**文件夹中的新 PowerShell 进程中执行**run_job.ps1**脚本。

## <a name="example"></a>示例

```powershell
PS C:\> entry.ps1
```

数列示例执行**entry.ps1**脚本。 此脚本将变量中指定的应用转换 `conversionsParameters` 为 .msix 包。 使用*virtualMachines*和*remoteMachines*变量中指示的虚拟机或远程计算机来转换应用。

## <a name="parameters"></a>参数

### <a name="virtualmachines"></a>virtualMachines

`virtualMachines`参数是一个数组，其中包含要连接到的虚拟机的名称和凭据，并在将应用打包为 .msix 格式时进行访问。

* **键入：** 组成
* **是否必需：** 否

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

将使用指定的虚拟机将应用打包为 .MSIX 格式。 此虚拟机将使用出现提示时输入的凭据进行连接（提示在脚本**entry.ps1**执行后直接显示）。 在将应用程序打包到 .MSIX 打包格式之前，该脚本会创建 Hyper-v VM 的快照，并在打包应用程序后将其还原到此快照。

### <a name="remotemachines"></a>remoteMachines

`remoteMachines`参数是一个数组，其中包含要连接到的远程计算机的名称和凭据，并在将应用打包为 .msix 格式时进行访问。 指定的远程计算机将是用于打包单个应用程序的单一使用设备。

远程计算机必须可在网络上访问和发现。

* **键入：** 组成
* **是否必需：** 否

```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

指定的远程计算机将用于将单个应用打包为 .MSIX 格式。 当出现提示时，此远程计算机将使用所输入的凭据进行连接（提示在脚本**entry.ps1**执行后直接显示）。

请确保在执行**entry.ps1**脚本之前，该设备的完全限定的域名或面向外部的别名都是可解析的。

### <a name="signingcertificate"></a>signingCertificate
`signingCertificate`参数是一个数组，其中包含的信息与将用于对 .msix 打包应用程序进行签名的代码签名证书相关。 此证书的加密级别必须至少为 SHA256。

* **键入：** 组成
* **是否必需：** 否

```powershell
$SigningCertificate = @{
    Password = "Password"; 
    Path = "C:\Temp\ContosoLab.pfx"
}
```
### <a name="conversionsparameters"></a>conversionsParameters

`conversionsParameters`参数是一个数组，其中包含要转换为 .msix 格式的应用的相关信息。 数组中的每个应用都将单独进行分析，并在远程计算机或虚拟机上通过 .MSIX 包转换运行。 应用将按照其在脚本中出现的顺序进行转换。 如果转换到 .MSIX 格式失败，该脚本将不会重新尝试转换其他远程计算机或虚拟机上的应用程序。

* **键入：** 组成
* **是否必需：** 是

```powershell
$conversionsParameters = @(
    ## Use for MSI applications:
    @{
        InstallerPath = "C:\Path\To\YourInstaller.msi";    # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0"                         # MSIX Application version (must contain 4 octets).
    },
    ## Use for EXE or other applications:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement"   # Arguements required by the installer to provide a silent installation of the application.
    },
    ## Creating the Packaged app and Template file in a specific folder path:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement";  # Arguements required by the installer to provide a silent installation of the application.
        SavePackagePath = "Custom\folder\Path";            # Specifies a custom folder path where the MSIX app will be created.
        SaveTemplatePath = "Custom\folder\Path"            # Specifies a custom folder path where the MSIX Template XML will be created.
    }
)
```

变量中提供的应用信息 `conversionsParameters` 将用于生成包含所有必需的应用程序详细信息的 XML 文件。 创建 XML 文件后，该脚本会将 XML 文件传递到要打包的[.Msix 打包工具](..\packaging-tool\mpt-overview.md)（MsixPackagingTool.exe）。

## <a name="logging"></a>日志记录

此脚本将生成一个日志文件，其中概述了在脚本执行过程中早于当前的内容。 日志文件将提供有关将应用程序打包到 .MSIX 打包格式的详细信息，以及与脚本进展相关的信息。 日志可以从任何文本实用工具读取，但已配置为使用 Trace32 日志读取器进行读取。 脚本执行中的错误将突出显示为红色，警告显示为黄色。 有关跟踪32日志读取器的详细信息，请访问 Microsoft Docs 上的[CMTrace](https://docs.microsoft.com/mem/configmgr/core/support/cmtrace) 。

在脚本的目录中创建日志文件 `.\logs\BulkConversion.log` 。
