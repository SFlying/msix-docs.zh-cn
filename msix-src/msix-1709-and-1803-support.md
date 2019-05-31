---
author: c-don
title: 在 Windows 10 版本 1709年及 1803 MSIX 支持
description: 本文概述了使用我们最新的更新 1/22/2019年截至 MSIX 的支持。
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: windows 10、 uwp、 msix，1709年、 1803年
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 37fd4538d7942f92316139f9187a4016997a0048
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400725"
---
# <a name="msix-support-on-windows-10-version-1709-and-1803"></a>在 Windows 10 版本 1709年及 1803 MSIX 支持

本文总结了 MSIX 支持和我们最新的更新的限制。

普遍的需求，我们已添加了对 MSIX 支持多个 Windows 10 版本上。 最值得注意的是，这包括 1709年和 1803年的版本。 此支持允许用户部署 MSIX 包在早期 Windows 版本，同时利用 MSIX，包括容器化并通过证书安全的所有优势。 若要充分利用这些更新，请确保具有 1.0.30311 或更高版本的应用安装程序并将在 Windows 10 1709年更新或更高版本。 

下面，我们讨论 MSIX 支持的功能和限制在较早的操作系统版本上。

##  <a name="msix-double-click-support"></a>MSIX 双击支持

部署 Windows 10 版本 1809年及更高版本上的 MSIX 包的优势之一是用户可以通过单击其上安装包。 应用安装程序 (1.0.30311.0) 的最新版本能够通过单击其上安装 MSIX 包是在 Windows 10 版本 1709年及 1803 也可用。

单击包提供了如 1809年中所示的相同安装支持，即应用程序安装程序会引导用户完成应用安装。 应用安装程序预安装，并从 Microsoft Store 中，获取更新，因此我们就可以始终将您的最佳的安装体验。

可以下载应用安装程序以供脱机使用从 Microsoft Store 企业中的业务[web 门户](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1)。 您可以了解有关脱机分发的详细信息[此处](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)。

下面是可在 Windows 10 版本 1709年及 1803年单击安装 MSIX 支持的摘要。

### <a name="support-matrix"></a>支持矩阵

| OS 版本|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Windows 10 版本 1709年 （内部版本 16299） | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Windows 10，版本 1803年 （内部版本 17134） | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Windows 10，版本 1809年 （内部 17763） 及更高版本 | &#x2713; | &#x2713; | &#x2713; | &#x2713; |

> [!NOTE]
> 在 Windows 10 版本 1709年及 1803年上不支持.msixbundle 格式。  希望生成包与这些 Windows 版本兼容的开发人员可以利用.appxbundle 格式。

## <a name="mdm-support"></a>MDM 支持

Microsoft Intune 和 System Center Configuration Manger (SCCM) 在 Windows 10 版本 1709年及 1803年上支持 MSIX 安装。 MSIX 包可安装在这些版本在相同的方式可使用 Windows 10 1809年和更高版本中。

## <a name="store-support"></a>存储区的支持

作为包的清单文件中列出的 MSIX 包支持的最低 OS 版本`MinVersion`在`TargetDeviceFamily`元素。 例如 MSIX 包可能会列出`MinVersion="10.0.17701.0"`作为支持的最低版本，这意味着对此及更高版本，可以运行 MSIX 包版本的操作系统。

在 Windows 10 版本 1709年、 1803 和 1809年，我们支持的主流企业部署方案。 这些包括安装通过 SCCM、 Microsoft Intune、 PowerShell 或双击安装。

目前，通过 Microsoft Store 和于企业的 Microsoft Store MSIX 安装需要 Windows 10 版本 1809年。

## <a name="packaging-and-signing"></a>打包和签名

目前，若要打包和签名 MSIX 文件，你需要 Windows 10 版本 1809 SDK。 MSIX 打包工具或 Visual Studio，可以通过 Windows 10 SDK 命令行工具 （MakeAppx.exe 和 SignTool.exe） 完成打包和签名。

## <a name="auto-elevation"></a>自动提升

在极少数情况下，某些应用需要提升的权限。

Windows 10 版本上运行 MSIX 包 1809年及更高版本的用户可以使用提升的权限，只需通过单击应用磁贴。 平台检测到提升需要并在用户具有所需的凭据时将自动提供它。

在 Windows 10 版本 1709年和 1803年，用户仍然可以使用提升的权限运行应用。 但是，它们需要显式选择以管理员身份运行应用程序。 若要执行此操作的一种方法是右键单击应用的磁贴并选择"以管理员身份运行"选项。

## <a name="digital-certificate-verification"></a>数字的证书验证

前面曾提到，我们强制实施正确签名的 Windows 10 版本 1709年及更高版本上的 MSIX 包。 但是，在 Windows 10 版本 1709年和 1803年，IT 专业人员必须遵守一些额外步骤来为其最终用户发布应用程序之前验证数字的证书。 为最终用户 MSIX 体验不会以任何方式更改。 最终用户将继续安装、 启动和使用之前，作为应用，因为由 IT 专业人员处理证书验证。

在 Windows 10 版本 1809年及更高版本，IT 专业人员可以验证 MSIX 包从 PowerShell 或通过 MSIX 包的属性的数字证书。

但是，在 Windows 10 版本 1709年和 1803年，IT 专业人员不能验证 MSIX 包属性中的证书除非 Windows 10 版本 1809 SDK 是安装。 如果 IT 专业人员有 1809 SDK 在设备上的与 Windows 10 版本 1709 通过 1803，签名可以是 Windows 10 版本可以通过从 PowerShell SDK 工具验证证书。
