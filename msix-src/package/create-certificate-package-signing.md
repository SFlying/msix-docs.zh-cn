---
title: 为程序包签名创建证书
description: 使用 PowerShell 工具为应用包签名创建和导出证书。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: d4ee3cae57fc5e344edd8a7daf48a24b49baeb80
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689716"
---
# <a name="create-a-certificate-for-package-signing"></a>为程序包签名创建证书

本文介绍了如何使用 PowerShell 工具为应用包签名创建和导出证书。 建议使用 Visual Studio[打包 UWP 应用](packaging-uwp-apps.md)并[打包桌面应用](../desktop/desktop-to-uwp-packaging-dot-net.md), 但如果未使用 visual studio 开发应用, 仍可手动打包应用。

## <a name="prerequisites"></a>先决条件

- **打包或未打包的应用**  
包含 AppxManifest.xml 文件的应用。 在创建用于给最终应用包签名的证书时，你将需要参考清单文件。 有关如何手动打包应用的详细信息，请参阅[使用 MakeAppx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md)。

- **公钥基础结构 (PKI) Cmdlet**  
你需要 PKI cmdlet 创建和导出你的签名证书。 有关详细信息，请参阅[公钥基础结构 Cmdlet](https://docs.microsoft.com/powershell/module/pkiclient/)。

## <a name="create-a-self-signed-certificate"></a>创建自签名证书

自签名证书可用于测试你的应用程序, 然后才能将其发布到应用商店。 按照此部分中所述的步骤创建自签名证书。

> [!NOTE]
> 自签名证书严格用于测试。 准备好将应用发布到应用商店或其他会场时, 请将证书切换到可信来源。 如果不这样做, 您的客户将无法安装您的应用程序。

### <a name="determine-the-subject-of-your-packaged-app"></a>确定你的打包应用的主体  

若要使用证书给你的应用包签名，证书中的“主体”**必须**匹配应用清单中的“发布者”部分。

例如，你应用的 AppxManifest.xml 文件中的“身份”部分应如下所示：

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

在这种情况下，“发布者”为“CN = Contoso 软件，O = Contoso Corporation，C = 美国”，创建你的证书时将需要这些。

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>使用 **New-SelfSignedCertificate** 创建证书

使用 **New-SelfSignedCertificate** PowerShell cmdlet 创建自签名证书。 **New-SelfSignedCertificate** 包含用于自定义的几个参数，但是鉴于本文目的，我们将侧重于创建可使用 **SignTool** 的简单证书。 有关更多示例和此 cmdlet 的使用，请参阅 [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate)。

基于上一示例中的 AppxManifest.xml 文件，你应该使用下面的语法创建证书。 在提升的 PowerShell 提示符中：

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

请注意以下有关某些参数的详细信息:

- **密钥用法**:此参数定义证书的用途。 对于自签名证书, 应将此参数设置为**DigitalSignature**。

- **TextExtension**:此参数包括以下扩展的设置:

  - 扩展密钥用法 (EKU):此扩展指示可使用已认证的公钥的其他目的。 对于自签名证书, 此参数应包含扩展字符串 **"2.5.29.37 = {text} 1.3.6.1.5.5.7.3.3"** , 这表示证书将用于代码签名。

  - 基本约束:此扩展指示证书是否为证书颁发机构 (CA)。 对于自签名证书, 此参数应包含扩展字符串 **"2.5.29.19 = {text}"** , 这表示该证书是一个最终实体 (而不是一个 CA)。

运行此命令后，证书将被添加到本地证书存储中，如“-CertStoreLocation”参数中指定。 此命令的结果还将生成证书的指纹。  

你可以使用以下命令在 PowerShell 窗口中查看你的证书：

```powershell
Set-Location Cert:\CurrentUser\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

这将显示本地存储中的所有证书。

## <a name="export-a-certificate"></a>导出证书 

若要将本地存储中的证书导出到个人信息交换 (PFX) 文件中，请使用 **Export-PfxCertificate** cmdlet。

使用 **Export-PfxCertificate** 时，你必须创建并使用密码或使用“-ProtectTo”参数指定哪些用户或组可以不使用密码访问该文件。 注意，如果不使用“-Password”或“-ProtectTo”参数，将显示错误。

### <a name="password-usage"></a>密码使用

```powershell
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

### <a name="protectto-usage"></a>ProtectTo 使用

```powershell
Export-PfxCertificate -cert Cert:\CurrentUser\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

创建并导出你的证书后，准备好使用 **SignTool** 给你的应用包签名。 有关手动打包过程中的后续步骤，请参阅[使用 SignTool 给应用包签名](sign-app-package-using-signtool.md)。

## <a name="security-considerations"></a>安全注意事项

通过将证书添加到[本地计算机证书存储](https://docs.microsoft.com/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores)，可以影响计算机上所有用户的证书信任。 建议当这些证书对于阻止其被用来破坏系统信任而言不再必要时删除这些证书。
