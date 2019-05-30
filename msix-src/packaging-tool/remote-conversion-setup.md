---
title: 远程转换中 MSIX 打包工具的安装程序
description: 设置远程转换的说明
author: c-don
ms.author: cdon
ms.date: 02/26/2019
ms.topic: article
keywords: MSIX，MPT，MSIX 打包工具，远程 IP
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 22ae4c4b939ed70c8a915ce1f3718105e2c65f49
ms.sourcegitcommit: b3564e47328d21916cdeb4c84d638ac12be0a461
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66186144"
---
# <a name="setup-instructions-for-remote-machine-conversions"></a>远程计算机的转换的设置说明 

若要连接到远程计算机运行您的转换的功能现已推出。 有几个需要开始使用远程转换之前需要采取的步骤。  

必须安全地访问在远程计算机上启用 PowerShell 远程处理。 此外必须在远程计算机的管理员帐户。  如果你想要使用的 IP 地址进行连接，请按照说明连接到未加入域的远程计算机。 

## <a name="connecting-to-a-remote-machine-in-a-trusted-domain"></a>连接到远程计算机中受信任的域 

若要启用 PowerShell 远程处理，从远程计算机上运行以下**管理员**PowerShell 窗口： 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck 
```

请务必在登录到使用域帐户和非本地帐户，在已加入域的计算机或将需要按照安装非域加入计算机的说明。 

### <a name="port-configuration"></a>端口配置 

如果在远程计算机是安全组 （如 Azure) 的一部分，则必须配置网络安全规则，以访问 MSIX 打包工具服务器。  

#### <a name="azure"></a>Azure 

1. 在 Azure 门户中，转到**联网** > **添加入站的端口** 
2. 单击**基本**
3. 服务字段应仍设置为**自定义**
4. 将端口号设置为**1599年**(MSIX 打包工具默认端口值 – 这可以在该工具的设置中更改) 和指定规则名称 (例如 AllowMPTServerInBound) 

#### <a name="other-infrastructure"></a>其他基础结构 

请确保您的服务器端口配置为对齐 MSIX 打包工具端口值 （MSIX 打包工具默认端口值是 1599年 – 这可以在该工具的设置中更改） 

## <a name="connecting-to-a-non-domain-joined-remote-machineincludes-ip-addresses"></a>连接到非域加入的远程计算机 （包括 IP 地址） 

对于加入非域的计算机，您必须是设置使用的证书通过 HTTPS 连接。 

1. 运行以下命令中的远程计算机上启用 PowerShell 远程处理和相应的防火墙规则**管理员**PowerShell 窗口： 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck  

New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled  True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP 
```
 
2. 生成自签名的证书，设置 WinRM HTTPS 配置，并且导出证书 

``` PowerShell
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My -KeyExportPolicy NonExportable).Thumbprint 

$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername"";CertificateThumbprint=""$thumbprint""}" 

cmd.exe /C $command 

Export-Certificate -Cert Cert:\LocalMachine\My\$thumbprint -FilePath <path_to_cer_file> 
```

3. 在本地计算机上复制导出的证书并将其安装在受信任的根存储区下 

``` PowerShell
Import-Certificate -FilePath <path> -CertStoreLocation Cert:\LocalMachine\Root 
``` 

### <a name="port-configuration"></a>端口配置 

如果在远程计算机是安全组 （如 Azure) 的一部分，则必须配置网络安全规则，以访问 MSIX 打包工具服务器。  

#### <a name="azure"></a>Azure 

按照说明[添加自定义端口](#azure)MSIX 打包工具，以及为 WinRM HTTPS 添加网络安全规则 

1. 在 Azure 门户中，转到**联网** > **添加入站的端口** 
2. 单击**基本** 
3. 将服务字段设置为**WinRM**

#### <a name="other-infrastructure"></a>其他基础结构 

请确保您的服务器端口配置为对齐 MSIX 打包工具端口值 （MSIX 打包工具默认端口值是 1599年 – 这可以在该工具的设置中更改） 
