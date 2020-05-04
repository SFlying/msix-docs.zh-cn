---
title: 更新现有 MSIX 以支持 MSIX Core
description: 本文概述了 .MSIX Core，其中提供了对 Windows 7 SP1、Windows 8.1、当前支持的 Windows Server （带有桌面体验）和 Windows 10 版本1709（秋季周年更新）的 .MSIX 支持。
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: a916cdf10b6fcf97f4e96f029bf7a2ad883615b9
ms.sourcegitcommit: e650c86433c731d62557b31248c7e36fd90b381d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/02/2020
ms.locfileid: "82726428"
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

## <a name="update-manifest-using-the-msix-packaging-tool-package-editor"></a>使用 .MSIX 打包工具包编辑器更新清单
如果你有 .MSIX 包，则可以使用 .MSIX 包工具更新现有包，以支持 .MSIX Core，而无需重新打包。 可以通过包编辑器两种方式执行此操作：

1. 打开 **.Msix 打包工具**应用
2. 选择**包编辑器** 
3. 单击 "**浏览 ...** " 找到你的包
4. 单击**打开包**

### <a name="option-1-use-the-checkbox-and-dropdown-to-add-support"></a>[选项 1]使用复选框和下拉列表添加支持
5. 在 " **.Msix Core 支持**" 下，选中相应的复选框以**将对 .msix Core 的支持添加到此包**中
6. 选择希望此包支持的 Windows 版本


### <a name="option-2-manually-add-in-the-manifest-file"></a>[选项 2]在清单文件中手动添加
5. 在 "**清单文件**" 下，单击 "**打开文件**"
6. 你正在查看包的清单。 在 "**依赖项**" 下，将 .msix Core 添加为目标设备系列（参见上文）
7. 保存并关闭清单 

8. 对包进行重新签名 
9. 单击 "**保存**" 并选择是否希望包递增 

## <a name="add-msix-core-support-using-the-msix-packaging-tool-during-conversion"></a>在转换过程中使用 .MSIX 打包工具添加 .MSIX 核心支持
从版本1.2020.402.0 开始，可以将 .MSIX Core 支持添加到用 .MSIX 打包工具生成的每个 .MSIX 包。 

### <a name="add-msix-core-support-to-all-msix-packages"></a>将 .MSIX Core 支持添加到所有 .MSIX 包
1. 打开 **.Msix 打包工具**应用
2. 选择右上方的齿轮以访问**设置**
3. 在 "**工具默认值**" 下，选中相应的复选框，以在**生成包时添加对 .msix Core 的支持。**
4. 选择默认情况下要支持的 Windows 版本
5. 保存设置

### <a name="add-msix-core-support-to-a-single-package-during-workflow"></a>在工作流过程中将 .MSIX Core 支持添加到单个包
在现有安装程序的转换过程中，如果未将 .MSIX 核心支持指定为默认设置，则可以选择将其添加到所生成的包中。 还可以覆盖在设置中指定的默认设置。 

1. 在 "转换包信息" 步骤中，选中相应的复选框以**添加对此包的 .Msix Core 支持**
2. 选择希望此包支持的 Windows 版本
3. 继续执行转换过程

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
