---
title: Windows 10 版本 1709 和 1803 上的 MSIX 支持
description: 本文汇总了包含最新更新（截至 2019 年 1 月 22 日）的 MSIX 的支持。
ms.date: 04/04/2019
ms.topic: article
keywords: windows 10, uwp, msix, 1709, 1803
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 700656bb7a6de66a876d8d46a9ac724d01caa11a
ms.sourcegitcommit: 073a228653f004914851c3461b9ad6eef343f915
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2019
ms.locfileid: "74309025"
---
# <a name="msix-support-on-windows-10-version-1709-and-1803"></a>Windows 10 版本 1709 和 1803 上的 MSIX 支持

本文汇总了 MSIX 支持以及最新更新的限制。

根据客户的普遍需求，我们在更多的 Windows 10 版本上添加了对 MSIX 的支持。 最值得注意的是，此次涵盖了版本 1709 和 1803。 此项支持可让用户在早期的 Windows 版本上部署 MSIX 包，同时利用 MSIX 的所有优势，包括容器化和通过证书实现的安全性。 若要利用这些更新，请确保使用 1.0.30311 或更高版本的应用安装程序，并在 Windows 10 1709 更新或更高版本上操作。 

下面介绍早期 OS 版本上的 MSIX 支持的功能和限制。

##  <a name="msix-double-click-support"></a>MSIX 双击支持

在 Windows 10 版本 1809 和更高版本上部署 MSIX 包的优势之一是，用户可以通过单击来安装包。 使用最新版本的应用安装程序 (1.0.30311.0) 时，通过单击安装 MSIX 包的功能也可以在 Windows 10 版本 1709 和 1803 上使用。

单击该包会提供与 1809 中相同的安装支持，即，应用安装程序会引导用户完成应用安装。 应用安装程序是预装的，将从 Microsoft Store 获取更新，因此，你始终可以获得最佳的安装体验。

可以从适用于企业的 Microsoft Store [Web 门户](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1)下载应用安装程序，以便在企业中离线使用。 可在[此处](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)详细了解离线分发。

下面是可在 Windows 10 版本 1709 和 1803 中使用的单击安装式 MSIX 支持的摘要。

### <a name="support-matrix"></a>支持矩阵

| OS 版本|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Windows 10 版本 1709（内部版本 16299） | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Windows 10 版本 1803（内部版本 17134） | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Windows 10 版本 1809（内部版本 17763）和更高版本 | &#x2713; | &#x2713; | &#x2713; | &#x2713; |

> [!NOTE]
> Windows 10 版本 1709 和 1803 不支持 .msixbundle 格式。  想要生成与这些 Windows 版本兼容的包的开发人员可以利用 .appxbundle 格式。

## <a name="mdm-support"></a>MDM 支持

Microsoft Intune 和 System Center Configuration Manger (SCCM) 支持 Windows 10 版本 1709 和 1803 上的 MSIX 安装。 可以像在 Windows 10 版本 1809 和更高版本中一样，在上述版本中安装 MSIX 包。

## <a name="store-support"></a>Store 支持

MSIX 包支持的最低 OS 版本已在包清单文件中的 `TargetDeviceFamily` 元素内以 `MinVersion` 形式列出。 例如，MSIX 包可能会列出 `MinVersion="10.0.17701.0"` 作为支持的最低版本，这表示 MSIX 包可以在此版本和更高版本的 OS 上运行。

Windows 10 版本 1709、1803 和 1809 支持主流的企业部署方案。 这包括通过 SCCM、Microsoft Intune、PowerShell 或双击安装方法进行安装。

目前，通过 Microsoft Store 和适用于企业的 Microsoft Store 安装 MSIX 需要使用 Windows 10 版本 1803。

## <a name="packaging-and-signing"></a>打包和签名

目前，若要打包和签名 MSIX 文件，需要使用 Windows 10 版本 1809 SDK。 可以通过 Windows 10 SDK 命令行工具（MakeAppx.exe 和 SignTool.exe）、MSIX 打包工具或 Visual Studio 完成打包和签名。

## <a name="auto-elevation"></a>自动提升

在罕见的情况下，某些应用需要提升的特权。

在 Windows 10 版本 1809 和更高版本上运行 MSIX 包的用户只需单击应用磁贴即可使用提升的特权。 平台会检测是否需要提升，并在用户具有所需的凭据时自动提供这种提升。

在 Windows 10 版本 1709 和 1803 上，用户仍可以使用提升的特权运行应用。 但是，他们需要显式选择以管理员身份运行应用。 为此，一种方法是右键单击应用的磁贴，并选择“以管理员身份运行”选项。

## <a name="digital-certificate-verification"></a>数字证书验证

如前所述，我们强制要求在 Windows 10 版本 1709 和更高版本上对 MSIX 包进行适当的签名。 但是，在 Windows 10 版本 1709 和 1803 上，IT 专业人员需要遵循几个额外的步骤来验证数字证书，然后为其最终用户发布应用。 对于最终用户而言，MSIX 体验没有任何变化。 最终用户可像以往一样继续安装、启动和使用应用，因为证书验证由 IT 专业人员处理。

在 Windows 10 版本 1809 和更高版本上，IT 专业人员可以通过 PowerShell 或 MSIX 包的属性来验证 MSIX 包的数字证书。

但是，在 Windows 10 版本 1709 和 1803 上，除非安装了 Windows 10 版本 1809 SDK，否则 IT 专业人员无法从 MSIX 包的属性验证证书。 如果 IT 专业人员在包含 Windows 10 版本 1709 至 1803 的设备上安装了 Windows 10 版本 1809 SDK，则可以在 PowerShell 中通过 SDK 工具验证证书的签名。
