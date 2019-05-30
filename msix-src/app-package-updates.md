---
title: 应用包更新
description: 了解如何应用差异会更新。
author: mcleanbyron
ms.author: mcleans
ms.date: 09/10/2018
ms.topic: article
keywords: windows 10、 uwp、 应用程序包、 应用更新，msix、 appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 1d7fc6a2d899638a8d38c47a477653173f2c5c9d
ms.sourcegitcommit: b6887e8f66e1d4a49870b933efa25d539eabfcaf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2019
ms.locfileid: "65882608"
---
# <a name="app-package-updates"></a>应用包更新

正在更新现代 Windows 应用包进行了优化以确保仅应用的重要更改的 bits 下载来更新现有的 Windows 应用。

## <a name="metadata-in-the-appxblockmapxml-file"></a>AppxBlockMap.xml 文件中的元数据

在高级别，包创建期间的元数据创建并存储在应用包文件 （.appx 或.msix） 允许唯一可以被 Windows 识别的包部分。 当更新应用程序包时，Windows 使用元数据文件比较旧到新的包的包，并确定需要将下载到设备的内容。

由于元数据，可以唯一标识包的部分，这意味着差异更新机制完全从任何版本的包 （假设源包的版本比目标包的任何其他版本的函数包）。 

元数据包含在 AppxBlockMap.xml 文件 （前面提到的元数据）。 AppxBlockMap.xml 文件是一个 XML 文件包含两个包中的文件有关的信息的维度列表。 第一个维度的布局文件 （例如，名称和大小） 的高级别详细信息和第二个维度提供的该文件 （"块"） 的每个 64 KB 扇区 SHA2-256 哈希表示形式。

下面是 AppxBlockMap.xml 文件的一个示例。

```xml
<!--?xml version="1.0" encoding="UTF-8"?-->
<blockmap hashmethod="http://www.w3.org/2001/04/xmlenc#sha256" 
          xmlns="http://schemas.microsoft.com/appx/2010/blockmap">
  <file lfhsize="66" size="101188" name="asset1.jpg">
    <block hash="2bidNE0JyaO+FjaTpRe0g8HzUCblUf/cfBcTXiZR74c="/>
    <block hash="+jeFwKrGk5gw9wSICWsWRtEQXwcLC7af4EWS7DgrAkY="/>
  </file>
  <file lfhsize="61" size="108823" name="asset2.jpg">
    <block hash="u0+5S0GOzwyAfYx54tKycZyHRBYm2ybvq27dkIKqDsQ="/>
    <block hash="F9h0FRMetL6BNCszAYB0bgyx2KWN+dO1bls4Q9m267c="/>
  </file>
  ...
</blockmap>
```

第一个文件 (asset1.jpg) 具有两个块哈希。 第一个哈希值表示文件的第一个 64 KB 的块，并且为文件指定的第二个哈希表示剩余 35 知识库-101188 字节。

在更新期间，如果修改该文件的第二个块，则哈希也会更新以反映这种状态。 下载组件提取第二个块，并重新使用旧的包中的第一个未更改的块。

更大的规模，如果整个文件不会更改 （由一套完整的块不更改），该文件可以重复使用从现有包，从而节省时间和资源。

## <a name="app-update-constraints"></a>应用更新约束

#### <a name="updates-are-performed-within-the-same-package-family"></a>相同的包系列内执行更新
包系列组成的包名称和发布服务器。 为了能够更新，新的包元数据将需要以前安装的包相同。 

#### <a name="app-updates-must-increment-to-a-higher-version"></a>应用更新必须递增到更高版本
应用更新是常规需要比当前高的新包的版本。 常规应用更新过程将不允许使用较低版本，默认情况下安装的包。 正在启动 Windows 10 1809年更新 *'回滚'* 引入了。 它允许更低版本包时重写开关提供更新 arguments 的一部分进行安装。 目前在 PowerShell 中使用 ForceUpdateFromAnyVersion 开关并在已[应用安装文件，](https://docs.microsoft.com/en-us/windows/msix/app-installer/update-settings)。  

#### <a name="app-update-package-can-have-a-different-architecture"></a>应用更新包可以有不同的体系结构
只要新的体系结构支持在正在部署到在 OS 上更新包的当前安装的应用程序包可以是不同的体系结构。 例如：如果具有 x86 Windows 10 设备在 x64 上安装的 MyFavApp(v1.0.0.0) 版本和更新 package(v2.0.0.0) 是 x64 版本：MyFavApp(1.0.0.0) 将已成功更新到 MyFavApp(v2.0.0.0)。 

#### <a name="packages-can-update-from-an-msix-to-an-msixbundle"></a>包可以从 MSIX 更新到 MSIXbundle
更新包可以从 MSIX 包转到 MSIXbundle 程序包，但反之则不然。 安装 MSIXbundle 时，包更新将需要保持一个捆绑包。 

## <a name="optimize-differential-update-technology"></a>优化差异更新技术
    
有几种方法以确保，差异更新技术已经过优化，最大值。

- 如果需要更改，将会影响完整文件，更新仍很小，将确保小-执行此操作对包中的保留文件。
- 对文件的更改应该是累加性尽可能的累加性的更改将确保最终用户设备仅下载那些已更改的块。
- 如果您的应用程序具有较大的文件，并且需要包含到块的一组更改的文件的中间部分的更改将起到显著作用，应到 64 KB 的块在可能的情况-包含对文件的更改。
 

