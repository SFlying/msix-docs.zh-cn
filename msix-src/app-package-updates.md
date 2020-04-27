---
title: 应用包更新
description: 介绍如何优化 MSIX 包，确保仅下载最重要的已更改应用位来更新现有的 Windows 应用。
ms.date: 09/10/2018
ms.topic: article
keywords: windows 10, uwp, 应用包, 应用更新, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 56db11754c5f9db0480abf0ec249b97486b3e614
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "73328282"
---
# <a name="app-package-updates"></a>应用包更新

新式 Windows 应用包的更新方式经过优化，可确保仅下载最重要的已更改应用位来更新现有的 Windows 应用。

## <a name="metadata-in-the-appxblockmapxml-file"></a>AppxBlockMap.xml 文件中的元数据

从较高层面看，在创建包的过程中，将会创建元数据片段并将其存储在应用包文件（.appx 或 .msix）中，使 Windows 能够唯一标识包的组成部分。 更新应用包时，Windows 将使用元数据文件来比较旧包与新包，并确定需要将哪些内容下载到设备。

由于使用元数据可以唯一标识包的组成部分，因此这意味着，差异更新机制完全可以在不同版本的包中正常运行（假设源包的版本低于目标包）。 

元数据包含在 AppxBlockMap.xml 文件中（前面所述的元数据）。 AppxBlockMap.xml 文件是一个 XML 文件，其中包含有关包中文件的信息的二维列表。 第一维列出有关文件的高级别详细信息（例如名称和大小），第二维提供该文件的每个 64KB 切片的 SHA2-256 哈希表示形式（“块”）。

下面是 AppxBlockMap.xml 文件的示例。

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

第一个文件 (asset1.jpg) 包含两个块哈希。 第一个哈希表示该文件的第一个 64KB 块，第二个哈希表示剩余的 35KB - 假设文件大小为 101188 字节。

在更新期间，如果该文件的第二个块已修改，则哈希也会更新以反映此操作。 下载组件提取第二个块，并重复使用旧包中第一个未经更改的块。

操作规模更大时，如果整个文件未更改（根据整组不变化的块确定），则可以从现有包中重复使用该文件，从而节省时间和资源。

## <a name="app-update-constraints"></a>应用更新约束

#### <a name="updates-are-performed-within-the-same-package-family"></a>更新是在相同的包系列内部执行的
包系列由包名称和发布者构成。 为了能够更新，新包元数据需要与以前安装的包相同。 

#### <a name="app-updates-must-increment-to-a-higher-version"></a>应用更新必须递增到更高版本
一般情况下，应用更新要求新包的版本高于当前版本。 默认情况下，应用更新进程不允许安装版本更低的包。 自 Windows 10 版本 1809 起，当提供重写开关作为更新参数的一部分时，可以使用 ForceUpdateToAnyVersion 允许安装更低版本的包。 目前可以在 PowerShell 中使用 [ForceUpdateFromAnyVersion](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 选项、通过 [PackageManager API](https://docs.microsoft.com/uwp/api/windows.management.deployment.deploymentoptions)、[EnterpriseModernAppManagement CSP](https://docs.microsoft.com/windows/client-management/mdm/enterprisemodernappmanagement-csp)，以及在 [AppInstaller 文件](https://docs.microsoft.com/windows/msix/app-installer/update-settings)中执行此操作。  

> [!NOTE]
> 如果对来自 Windows Store 的应用使用 ForceUpdateToAnyVersion，Windows 更新会自动将该应用更新到最新的适用版本。

#### <a name="app-update-package-can-have-a-different-architecture"></a>应用更新包可以采用不同的体系结构
只要部署到的 OS 支持新体系结构，则更新包与当前安装的应用包可以采用不同的体系结构。 例如：如果在 x64 Windows 10 设备上安装了 x86 版本的 MyFavApp (v1.0.0.0)，并且更新包 (v2.0.0.0) 采用 x64 版本：MyFavApp (1.0.0.0) 将成功更新到 MyFavApp (v2.0.0.0)。 

#### <a name="packages-can-update-from-an-msix-to-an-msixbundle"></a>可以从 MSIX 将包更新到 MSIXbundle
可以从 MSIX 包更新到 MSIXbundle 包，但反之不然。 安装 MSIXbundle 时，包更新需要保留一个捆绑包。 

## <a name="optimize-differential-update-technology"></a>优化差异更新技术
    
可通过多种方式最大程度地优化差异更新技术。

- 在包中保留小文件 - 这可以确保在需要执行会影响整个文件的更改时，更新规模仍然很小。
- 应该尽可能地对文件进行加法式更改 - 加法式更改可确保最终用户设备仅下载已更改的块。
- 应该尽可能地将文件更改包含在 64KB 块中 - 如果应用包含大文件并且需要对文件的中间部分进行更改，则在一组块中包含更改可以提供显著的帮助。
 

