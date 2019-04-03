---
title: 应用程序包更新
description: 了解如何应用差异会更新。
author: mcleanbyron
ms.author: mcleans
ms.date: 09/10/2018
ms.topic: article
keywords: windows 10、 uwp、 应用程序包、 应用更新，msix、 appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 786dd7236b8d5c8600dd36e5d3e507576df79ae7
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900509"
---
# <a name="app-package-updates"></a>应用程序包更新

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

有两种方法使用此差异更新技术。

- 如果需要更改，将会影响完整文件，更新仍很小，将确保小-执行此操作对包中的保留文件。
- 对文件的修改应该是累加性尽可能的累加性的更改将确保最终用户设备仅下载那些已更改的块。
- 如果您的应用程序具有较大的文件，并且需要包含到块的一组更改的文件的中间部分的更改将起到显著作用，应到 64 KB 的块在可能的情况-包含对文件的修改。
