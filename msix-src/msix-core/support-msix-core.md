---
title: 更新现有 .MSIX 以支持 .MSIX 核心
description: 本文概述了 .MSIX Core，其中提供了对 Windows 7 SP1、Windows 8.1、当前支持的 Windows Server （带有桌面体验）和 Windows 10 版本1709（秋季周年更新）的 .MSIX 支持。
ms.date: 04/12/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: a662ac31dce888c7dc373e546c7a70e1c62e4653
ms.sourcegitcommit: 8aafaa9ac4087ef2e95030343add8fe2ee1cccc9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/18/2019
ms.locfileid: "75186783"
---
# <a name="update-your-existing-msix-to-support-msix-core"></a>更新现有 .MSIX 以支持 .MSIX 核心 
若要在 Windows 7 SP1 上使用现有的 .MSIX 包 Windows 8.1、当前支持的 Windows 服务器（具有桌面体验）以及1709之前的 Windows 10 版本（秋季更新），则需要更新 .MSIX 包清单。 打包为 .MSIX 的应用必须与要在其中部署它们的操作系统兼容。  .MSIX 包清单必须包含一个名称为 MSIXCore 的正确**y**和一个与操作系统内部版本号匹配的 MinVersion。  还应确保同时包含相关的 Windows 10 1709 和更高版本的条目，以便应用在本机支持 .MSIX 的操作系统上正确部署。

Windows 7 SP1 的最低版本示例：

```
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

所有 MSIXCore 应用都将部署到 Windows Server，并且具有具有相同内部版本号的基于桌面体验的操作系统。  如果应用仅适用于服务器操作系统，请使用 MSIXCore 的 y。  不支持部署到 Windows Server Core。

## <a name="supported-versions-of-windows-with-msix-core"></a>支持的 Windows 版本（含 .MSIX Core） 

| 名称 | 版本 |
|------|---------|
| Windows 7，Service Pack 1| 6.1.7601.0|
| Windows 8.1 （Update 1） |6.3.9600.0|
| Windows 10 2015 LTSB （1507）|10.0.10240.0|
| Windows 10 2016 LTSB （1607）|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| WIN ENT LTSB 2016 Finnish 64 Bits | 10.0.14393.0|
