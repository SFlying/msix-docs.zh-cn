---
author: nonasi
title: 在内部版本 1709年和 1803 MSIX 支持
description: 本文概述了使用我们最新的更新 1/22/2019年截至 MSIX 的支持。
ms.author: nonasir
ms.date: 01/18/2019
ms.topic: article
keywords: windows 10、 uwp、 msix，1709年、 1803年
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 667de11b23e9e5eab75cb60be016adbf100409e8
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900569"
---
# <a name="msix-support-on-windows-10-builds-1709-and-1803"></a>在 Windows 10 版本 1709年和 1803 MSIX 支持

本文总结了 MSIX 支持和我们最新的更新的限制。

普遍的需求，我们已添加了对 MSIX 支持多个 Windows 10 版本上。 最值得注意的是，这包括 1709年和 1803年的版本。 此支持允许用户部署 MSIX 包在早期 Windows 版本，同时利用 MSIX，包括容器化并通过证书安全的所有优势。 若要充分利用这些更新，请确保你已 1.0.30311 或更高版本的应用安装程序和最新服务修补程序在 1709年及 1803年的设备上。

下面，我们讨论 MSIX 支持的功能和限制在较早的操作系统版本上。

##  <a name="msix-double-click-support"></a>MSIX 双击支持
部署 MSIX 1809 和更高版本上的优势之一是用户可以通过单击其上安装包。 应用安装程序-1.0.30311.0-的最新版本能够通过单击其上安装 MSIX 包可以 1709年和 1803 也是可用。 

单击包提供如下所示 1809年相同的安装支持，Microsoft 的应用安装程序应用程序即显示给用户，指导他们完成将应用安装的 UI。 应用安装程序预安装，并从应用商店，获取更新，因此我们就可以始终将您的最佳的安装体验。 

下面是单击安装 MSIX 支持的摘要可 1709年和 1803年上。

### <a name="support-matrix"></a>支持矩阵

| OS 版本|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Win 10 1709(16299) | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Win 10 1803(17134) | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Win 10 1809(17763) 及更高版本 | &#x2713; | &#x2713; | &#x2713; | &#x2713; |


> [!NOTE] 
> 不支持 Windows 10 版本 1709年和 1803 MSIXbundles。  希望生成包与这些 Windows 版本兼容的开发人员可以利用.appxbundle 技术。

## <a name="mdm-support"></a>MDM 支持
Microsoft Intune 和 SCCM 1709 和 1803年支持 MSIX 安装。 MSIX 可以在上安装这些生成相同的方式可使用 1809年及更高版本。 

## <a name="store-support"></a>存储区的支持
MSIX 的受支持的最低操作系统版本被列为在清单文件中的包的 MinVersion TargetDeviceFamily 元素中。 例如 MSIX 包可能会列出 MinVersion ="10.0.17701.0"作为支持的最低版本，这意味着对此及更高版本可以运行 MSIX 版本的操作系统。

在 1709，1803年和 1809年我们支持的主流企业部署方案。 这些包括通过 System Center Configuration Manger (SCCM) Microsoft Intune，通过 PowerShell 编写脚本的安装，或双击安装。

当前通过 Microsoft Store 和于企业的 Microsoft Store MSIX 安装需要 Windows 10 版本 1809年。

## <a name="packaging--signing"></a>打包和签名
当前进行打包和签名 MSIX，您需要 1809 SDK。 可通过 SDK 命令行工具，MSIX 打包工具或 Visual Studio 打包和签名。 

## <a name="auto-elevation"></a>自动提升
在极少数情况下，某些应用需要提升的权限。 

在正在运行 MSIX 比 1809年可以使用更高版本生成的用户只需通过单击应用磁贴上提升的权限。 Plaftorm 检测提升需要并在用户具有所需的凭据时将自动提供它。 

在版本 1709年和 1803年中，用户仍然可以运行应用程序使用提升的权限，但它们需要进行显式选以管理员身份运行该应用程序若要执行此操作的一种方法是右键单击应用的磁贴并选择"以管理员身份运行"选项。

## <a name="signature-verification"></a>签名验证
前面曾提到，我们强制 MSIX 包在所有 Windows 版本 1709年及更高版本上的正确签名。 但是，在早期 Windows 版本 （1709年和 1803年），IT 专业人员必须遵守一些额外步骤来为其最终用户发布应用之前先验证签名。 请注意，为最终用户 MSIX 体验不会以任何方式更改。 最终用户将继续进行安装、 启动和使用之前，作为应用程序，因为由 IT 专业人员处理签名验证。 

在 Windows 10 版本 1809年及更高版本中，IT 专业人员可以验证应用程序的签名从 PowerShell 或通过 MSIX 包的属性。 

但是，版本 1709年和 1803 IT 专业人员无法验证签名从 MSIX 包的属性，除非 1809 SDK 安装。 如果 IT 专业人员通过 1803 Windows 1709 的设备上具有 1809 SDK，可以通过从 PowerShell SDK 工具验证的签名。 
