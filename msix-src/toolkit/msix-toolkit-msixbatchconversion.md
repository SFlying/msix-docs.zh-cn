---
title: .MSIX 批处理转换脚本
description: 本文提供了有关 .MSIX 工具包中的批处理转换脚本的详细信息。
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0b0329df39d55fff04b6bdf33a5ec3865a823b99
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073563"
---
# <a name="msix-batch-conversion-scripts"></a>.MSIX 批处理转换脚本

.MSIX 工具包中的[批处理转换脚本](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion)可用于自动将 Windows 应用转换为 .msix 包格式。 **条目. ps1**脚本中提供了应用及其详细信息的列表。

## <a name="syntax"></a>语法

```powershell
entry.ps1
```

## <a name="description"></a>说明

这是一组 PowerShell 脚本，提供将应用程序批量打包为 .MSIX 包格式的功能。 这些脚本将连接到用于打包每个应用程序的本地虚拟机或远程计算机。

打包为 .MSIX 应用程序格式的应用程序将按照它们在**项 ps1**脚本中的输入顺序进行转换。 **条目**中列出的远程计算机将用于将应用程序打包为 .msix 格式。 可以多次使用虚拟机将不同的应用程序打包为 .MSIX 应用程序格式。

在运行该脚本之前，必须先将要转换的应用添加到脚本中的 `conversionsParameters` 变量。 可以将多个应用添加到变量。 此脚本利用应用和远程/虚拟机创建一个 XML 文件，该文件已设置格式以满足[.Msix 打包工具](..\packaging-tool\mpt-overview.md)（MsixPackagingTool）的要求。 创建 XML 文件后，将在新的 PowerShell 进程中执行**run_job ps1**脚本，该过程会在目标设备上执行 MsixPackagingTool，以转换应用并将其放在位于脚本执行文件夹中的 **.\Out**文件夹中。

## <a name="example"></a>示例

```powershell
PS C:\> entry.ps1
```

数列示例执行**项 ps1**脚本。 此脚本将 `conversionsParameters` 变量中指定的应用转换为 .MSIX 包。 使用*virtualMachines*和*remoteMachines*变量中指示的虚拟机或远程计算机来转换应用。

## <a name="parameters"></a>参数

### <a name="virtualmachines"></a>virtualMachines

`virtualMachines` 参数是一个数组，其中包含要连接到的虚拟机的名称和凭据，并在将应用打包为 .MSIX 格式时进行访问。

* **键入：** 组成
* **必需：** 不

```

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

将使用指定的虚拟机将应用打包为 .MSIX 格式。 此虚拟机将使用出现提示时输入的凭据进行连接（提示在脚本**条目**后直接显示）。

### <a name="remotemachines"></a>remoteMachines

`remoteMachines` 参数是一个数组，其中包含要连接到的远程计算机的名称和凭据，并在将应用打包为 .MSIX 格式时进行访问。 指定的远程计算机将是用于打包单个应用程序的单一使用设备。

远程计算机必须可在网络上访问和发现。

* **键入：** 组成
* **必需：** 不

```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

指定的远程计算机将用于将单个应用打包为 .MSIX 格式。 当出现提示时，此远程计算机将使用输入的凭据进行连接（提示在脚本**输入**后直接显示）。

请确保在执行**entry**脚本之前，该设备的完全限定的域名或面向外部的别名都是可解析的。

### <a name="conversionsparameters"></a>conversionsParameters

`conversionsParameters` 参数是一个数组，其中包含要转换为 .MSIX 格式的应用的相关信息。 数组中的每个应用都将单独进行分析，并在远程计算机或虚拟机上通过 .MSIX 包转换运行。 应用将按照其在脚本中出现的顺序进行转换。 如果转换到 .MSIX 格式失败，该脚本将不会重新尝试转换其他远程计算机或虚拟机上的应用程序。

* **键入：** 组成
* **必需：** 是的

```powershell
$conversionsParameters = @(
    @{
        InstallerPath = "Path\To\Your\Installer\YourInstaller.msi"; # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                                    # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                            # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                           # Certificate Publisher information
        PublisherDisplayName = "YourCompany";                       # Application Publisher name
        PackageVersion = "1.0.0.0"                                  # MSIX Application version (must contain 4 octets).
    }
)
```

`conversionsParameters` 变量中提供的应用信息将用于生成包含所有必需的应用程序详细信息的 XML 文件。 创建 XML 文件后，该脚本会将 XML 文件传递到要打包的[.Msix 打包工具](..\packaging-tool\mpt-overview.md)（MsixPackagingTool）。
