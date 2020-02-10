---
description: 本文介绍如何在 Visual Studio 中对 .MSIX 包进行签名。
title: 在 Visual Studio 中对 .MSIX 包进行签名
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 7904f6de962804a9776338860485c98d04360f02
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073673"
---
# <a name="sign-an-msix-package-in-visual-studio"></a>在 Visual Studio 中对 .MSIX 包进行签名

Visual Studio 中的 " **Windows 应用程序打包项目**" 项目包括默认密码保护的个人信息交换（PFX）格式文件，你可能希望将其替换为你自己的文件。 如果企业未向你提供代码签名证书，则可以从受信任的颁发机构购买证书，也可以创建自签名证书。 Visual Studio 中有 "创建测试证书" 选项和导入向导，你可以在默认的应用程序清单设计器中打开 appxmanifest.xml 文件，并在 "打包" 选项卡下查看。如果不是在向导和对话框中，可以使用 New-selfsignedcertificate PowerShell cmdlet 来创建证书：

```
 > New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=MyCompany,
  O=MyCompany, L=Stockholm, S=N/A, C=Sweden" -KeyUsage DigitalSignature
    -FriendlyName MyCertificate -CertStoreLocation "Cert:\LocalMachine\My"
      -TextExtension @('2.5.29.37={text}1.3.6.1.5.5.7.3.3',
        '2.5.29.19={text}Subject Type:End Entity')
```

Cmdlet 将输出指纹（如 A27）。此处为 D9F），可以将证书传递到另一个 cmdlet、移动项，将证书移动到受信任的根证书存储中：

```
>Move-Item Cert:\LocalMachine\My\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F
  -Destination Cert:\LocalMachine\Root
```
同样，你需要将证书安装到你打算在其中安装和运行打包应用程序的所有计算机上的此存储中。 还需要在这些设备上启用应用旁加载。 在非托管计算机上，此操作可在 "更新 & 安全" |适用于 "设置" 应用中的开发人员。 在组织管理的设备上，可以通过将策略推送到移动设备管理（MDM）提供程序来启用旁加载。

指纹还可用于使用 Get-pfxcertificate cmdlet 将证书导出到新的 PFX 文件：

```
>$pwd = ConvertTo-SecureString -String secret -Force -AsPlainText
>Export-PfxCertificate -cert
  "Cert:\LocalMachine\Root\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F"
    -FilePath "c:/<SolutionFolder>/Msix/certificate.pfx" -Password $pwd
```
请记住，通过在设计器的 "打包" 选项卡中选择 .MSIX 包，或者手动编辑 wapproj 项目文件并替换 <PackageCertificateKeyFile> 和 <PackageCertificateThumbprint> 元素的值，来告诉 Visual Studio 使用生成的 PFX 文件来签署包。
