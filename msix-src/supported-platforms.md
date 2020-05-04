---
title: 支持的平台
description: 本文介绍适用于 .MSIX 的支持平台。
author: dianmsft
ms.date: 03/06/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ad03f17ce2a34347959f2723ea83408ae97f623c
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "81614019"
---
# <a name="supported-platforms"></a>支持的平台

以下 Windows 版本目前支持 .MSIX：

* Windows 10 版本1709及更高版本。
* Windows Server 2019 LTSC 及更高版本。
* Windows Enterprise 2019 LTSC 及更高版本。

有关 Windows 生命周期支持的更多详细信息，如服务日期结束，请访问[windows 生命周期事实数据表](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet)。

本文介绍了这些 Windows 版本中的 .MSIX 的主要功能。

> [!NOTE]
> Windows Server 2019 LTSC 和 Windows Enterprise 2019 LTSC 要求**应用安装**程序应用支持双击安装或直接从 .msix、.msixbundle、.appx 或 .appxbundle 网站上安装。 如果没有此应用，可以通过 PowerShell、API 或使用受支持的系统管理产品来安装包。 有关 Windows Server 2019 LTSC 的更多注意事项，请参阅[此文](msix-server-2019.md)。

> [!NOTE]
> 对于早于 Windows 10 版本1709的 Windows 版本，请使用[.Msix Core](msix-core/msixcore.md)安装 .msix 包。

## <a name="msix-feature-support"></a>.MSIX 功能支持

下表显示了在不同版本的 Windows 中受支持的 .MSIX 功能和方案。

> [!div class="mx-tableFixed"]
| 功能 | 1709 | 1803 | 1809 | 1903 | 1909 | 2004| Windows Server 2019 LTSC | Windows Enterprise 2019 LTSC|
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| [允许提升](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| [应用安装程序文件支持](app-installer/installing-windows10-apps-web.md)| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| [延迟注册标志](desktop/managing-your-msix-deployment-update.md) |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |
| [从任何版本降级强制更新](desktop/managing-your-msix-deployment-targetdevices.md) |  :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| 强制预配 |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |
| 打包桌面应用的标识 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| [修改包](modification-packages.md) | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| 本机 .MSIX 安装和卸载 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark: |
| [包支持框架（PSF）](psf/package-support-framework-overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:|  :heavy_check_mark: | :heavy_check_mark:|  
| [Windows 服务](packaging-tool/convert-an-installer-with-services.md) | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x:| :x: | 
| [非存储包的包完整性强制](package/signing-package-overview.md#package-integrity-enforcement) | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x:| :x: | 
## <a name="package-format-support"></a>包格式支持

下表显示了 Windows 10 的不同版本支持的包格式。

| 包格式 | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .msixbundle| :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:|
| .appx | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .appxbundle | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## <a name="microsoft-store"></a>Microsoft Store

下表显示了 Windows 10 的不同版本支持的 Microsoft Store 功能。

| 功能 | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| 发布             | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| 更新通知| :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| 流式安装 | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| 
| 增量更新 | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

> [!NOTE]
> 对于上面列出的所有版本的 Windows 10，.appx 或 .appxbundle 都适用。 该表仅反映 .msix 或 .msixbundle 行为。

### <a name="microsoft-store-submissions"></a>Microsoft Store 提交

MSIX 包支持的最低 OS 版本已在包清单文件中的 `TargetDeviceFamily` 元素内以 `MinVersion` 形式列出。 例如，.MSIX 包可能列出`MinVersion="10.0.17701.0"`为支持的最低版本，这意味着 .msix 包可以在此版本和更高版本的操作系统上运行。

Windows 10 版本 1709、1803 和 1809 支持主流的企业部署方案。 其中包括通过 Microsoft 终结点 Configuration Manager、Microsoft Intune、PowerShell 或双击安装进行安装。

目前，通过 Microsoft Store 和 Microsoft Store for Business 进行 .MSIX 安装需要 Windows 10 版本1809及更高版本。
