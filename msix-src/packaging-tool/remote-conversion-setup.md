---
title: MSIX 打包工具中的远程转换设置
description: 本文介绍如何设置并连接到远程计算机，以便使用 .MSIX 打包工具运行应用转换。
ms.date: 02/26/2019
ms.topic: article
keywords: MSIX, MPT, MSIX 打包工具, 远程 IP
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: a13b2a2725cc8c3750cbd22d4238ae41f5406fdc
ms.sourcegitcommit: 4d912f89e385268757e87bf8fd9ca1828b99e109
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77544771"
---
# <a name="setup-instructions-for-remote-machine-conversions"></a>有关远程计算机转换的设置说明

与远程计算机连接是一种选择，以确保遵循转换环境的[最佳实践](prepare-your-environment.md)建议，因为它可以是比本地计算机更干净的环境。 在开始使用远程转换之前，需要执行几个步骤。  

为确保访问的安全，必须在远程计算机上启用 PowerShell 远程处理。 你还必须具有远程计算机的管理员帐户。  若要使用 IP 地址进行连接，请遵照有关连接到未加入域的远程计算机的说明操作。

## <a name="connecting-to-a-remote-machine-in-a-trusted-domain"></a>连接到受信任域中的远程计算机

若要启用 PowerShell 远程处理，请在远程计算机上以**管理员身份**从 PowerShell 窗口运行以下内容： 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck
```

请务必使用域帐户（而不是本地帐户）登录到已加入域的计算机，或者遵照有关连接到未加入域的计算机的设置说明操作。

### <a name="port-configuration"></a>端口配置

如果远程计算机在安全组（例如 Azure）中，则必须配置网络安全规则来访问 MSIX 打包工具服务器。  

#### <a name="azure"></a>Azure

1. 在 Azure 门户中，转到“网络” **“添加入站端口”**  > 
2. 单击“基本”
3. “服务”字段应保留设置为“自定义”
4. 将端口号设置为 **1599**（MSIX 打包工具默认端口值 – 可以在工具的“设置”中更改），并指定规则的名称（例如 AllowMPTServerInBound）

#### <a name="other-infrastructure"></a>其他基础结构

确保服务器端口配置与 MSIX 打包工具端口值相符（MSIX 打包工具默认端口值为 1599 – 可以在工具的“设置”中更改）

## <a name="connecting-to-a-non-domain-joined-remote-machineincludes-ip-addresses"></a>连接到未加入域的远程计算机（包括 IP 地址）

对于未加入域的计算机，必须设置一个用于通过 HTTPS 建立连接的证书。

1. 通过在 PowerShell 窗口中的远程计算机上以**管理员身份**运行以下内容来启用 PowerShell 远程处理和适当的防火墙规则：

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck  

New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled  True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
```
 
2. 生成自签名证书，设置 WinRM HTTPS 配置，然后导出证书

``` PowerShell
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My -KeyExportPolicy NonExportable).Thumbprint

$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername"";CertificateThumbprint=""$thumbprint""}"

cmd.exe /C $command

Export-Certificate -Cert Cert:\LocalMachine\My\$thumbprint -FilePath <path_to_cer_file>
```

3. 在本地计算机上复制导出的证书，并将其安装在受信任根存储下

``` PowerShell
Import-Certificate -FilePath <path> -CertStoreLocation Cert:\LocalMachine\Root
```

### <a name="port-configuration"></a>端口配置 

如果远程计算机在安全组（例如 Azure）中，则必须配置网络安全规则来访问 MSIX 打包工具服务器。  

#### <a name="azure"></a>Azure

遵照说明为 MSIX 打包工具[添加自定义端口](#azure)，并为 WinRM HTTPS 添加网络安全规则

1. 在 Azure 门户中，转到“网络” **“添加入站端口”**  > 
2. 单击“基本”
3. 将“服务”字段设置为 **WinRM**

#### <a name="other-infrastructure"></a>其他基础结构 

确保服务器端口配置与 MSIX 打包工具端口值相符（MSIX 打包工具默认端口值为 1599 – 可以在工具的“设置”中更改）
