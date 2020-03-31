---
title: 更新现有 MSIX 以支持 MSIX Core
description: 本文概述了 .MSIX Core，其中提供了对 Windows 7 SP1、Windows 8.1、当前支持的 Windows Server （带有桌面体验）和 Windows 10 版本1709（秋季周年更新）的 .MSIX 支持。
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 3fb4d333c0ed84a4d54522e17b7e4ab54c6e28be
ms.sourcegitcommit: f6cee51b46fc36a57b5cf9c8cb3fd24a40ae858a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391624"
---
# <a name="update-your-existing-msix-package-to-support-msix-core"></a>更新现有的 .MSIX 包以支持 .MSIX 核心

在将 .MSIX 包与[.Msix Core](msixcore.md)一起部署之前，必须先更新 .msix 包清单。

打包为 .MSIX 的应用必须与要在其中部署它们的操作系统兼容。 .MSIX 包清单必须包含一个名称为**MSIXCore**的正确**y**和一个与操作系统内部版本号匹配的**MinVersion** 。 请确保同时包含相关的 Windows 10 版本1709和更高版本的条目，以便应用在本机支持 .MSIX 的操作系统上正确部署。

下面的示例指定 Windows 7 SP1 作为最低版本：

```xml
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

所有**MSIXCore**应用都将部署到 Windows Server，并且具有具有相同内部版本号的基于桌面体验的操作系统。 如果应用仅适用于服务器操作系统，请指定名为**MSIXCore**的**y** 。 不支持部署到 Windows Server Core。

## <a name="update-manifest-using-the-msix-packaging-tool"></a>使用 .MSIX 打包工具更新清单 
如果有 .MSIX 包，可以使用 .MSIX 包工具更新包以支持 .MSIX Core。 步骤如下： 
1. 打开 **.Msix 打包工具**应用
2. 选择**包编辑器** 
3. 单击 "**浏览 ...** " 找到你的包
4. 单击**打开包**
5. 在 "**清单文件**" 下，单击 "**打开文件**"
6. 你正在查看包的清单。 在 "**依赖项**" 下，将 .msix Core 添加为目标设备系列（参见上文）
7. 保存并关闭清单 
8. 对包进行重新签名 
9. 单击 "**保存**" 并选择是否希望包递增 

## <a name="windows-versions-supported-by-msix-core"></a>.MSIX Core 支持的 Windows 版本

| 名称 | 版本 |
|------|---------|
| Windows 7，SP 1| 6.1.7601.0|
| Windows 8.1 （Update 1） |6.3.9600.0|
| Windows 10 2015 LTSB （1507）|10.0.10240.0|
| Windows 10 2016 LTSB （1607）|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
