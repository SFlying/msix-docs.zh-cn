---
title: 修改包发布服务器脚本
description: 本文提供了有关 .MSIX Toolit 中修改包发布者脚本的详细信息。
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10、.msix、.msix 工具包
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b54d6d0a6d1f1d971a7ece99104fdac0db3c49d3
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073573"
---
# <a name="modify-package-publisher-script"></a>修改包发布服务器脚本

.MSIX 工具包中的[Modify package publisher 脚本](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/ModifyPackagePublisher)可用于更新清单中的发布服务器，然后再根据新证书对包进行重新签名。 此脚本当前限制为 .MSIX 应用，而不是 .MSIX 捆绑包。

## <a name="syntax"></a>语法

```powershell
.\modify-package-publisher.ps1 -directory <String> -redist <String> -certPath <String> [[-pfxPath] <String>] [[-Password] <String>] [[-forceContinue]<Switch>]
```

## <a name="examples"></a>示例

### <a name="update-the-publisher-based-on-the-certificate"></a>根据证书更新发布服务器

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer"
```

此命令以递归方式搜索所有 .MSIX 包的 C:\MSIX 的内容，并更新 .MSIX 应用发行者，使其与位于 C:\cert\mycert.cer. 的证书发布者匹配。

### <a name="update-the-publisher-and-sign-the-msix-app"></a>更新发布服务器并对 .MSIX 应用进行签名

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx"
```

此命令以递归方式搜索所有 .MSIX 包的 C:\MSIX 的内容，并更新 .MSIX 应用发行者，使其与位于 C:\cert\mycert.cer. 的证书发布者匹配。 然后，该命令使用位于 C:\cert\CertKey.pfx. 的证书重新签名已标识的 .MSIX 包。

### <a name="update-the-publisher-and-sign-the-msix-app-with-a-password-protected-pfx-certificate"></a>更新发布服务器并使用受密码保护的 PFX 证书对 .MSIX 应用进行签名

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -password "aaabbbccc"
```

此命令以递归方式搜索所有 .MSIX 包的 C:\MSIX 的内容，并更新 .MSIX 应用发行者，使其与位于 C:\cert\mycert.cer. 的证书发布者匹配。 然后，该命令使用 C:\cert\CertKey.pfx 上的证书对已标识的 .MSIX 包重新签名，并使用 password *aaabbbccc*解锁受密码保护的证书。

### <a name="update-the-publisher-sign-the-msix-app-and-force-continue-to-next-msix-app"></a>更新发布服务器，对 .MSIX 应用进行签名，并强制继续下一个 .MSIX 应用

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -forceContinue -pfxPath "C:\cert\CertKey.pfx"
```

此命令以递归方式搜索所有 .MSIX 包的 C:\MSIX 的内容，并更新 .MSIX 应用发行者，使其与位于 C:\cert\mycert.cer. 的证书发布者匹配。 然后，该命令使用位于 C:\cert\CertKey.pfx. 的证书重新签名已标识的 .MSIX 包。 如果在处理 .MSIX 包时出现任何错误，脚本将继续更新发布服务器并对标识的 .MSIX 包进行重新签名。

## <a name="parameters"></a>参数

### <a name="-directory"></a>-目录

提供包含 .MSIX 应用程序的根目录。 将以递归方式搜索所有 .MSIX 包的目录。

* **键入：** 类似
* **必需：** 是的
* **位置：** Named
* **默认值：** 内容

### <a name="-certpath"></a>-certPath

提供证书文件（* .cer）的完整路径，该文件用于标识新的或更新的应用程序发布服务器信息。

* **键入：** 类似
* **必需：** 是的
* **位置：** Named
* **默认值：** 内容

### <a name="-redist"></a>-已再发行

从[.Msix 工具包](https://aka.ms/msixtoolkit)中检索到的可再发行文件的路径。 此文件用于将应用重新打包为 .MSIX 包格式。 必须指向32位或64位体系结构的可再发行组件。

* **键入：** 类似
* **必需：** 是的
* **位置：** Named
* **默认值：** 内容

### <a name="-pfxpath"></a>-pfxPath

代码签名证书（* .pfx）的路径，将在更新应用发布者后用于对 .MSIX 包进行签名。

* **键入：** 类似
* **必需：** 不
* **位置：** Named
* **默认值：** 内容

### <a name="-password"></a>-password

代码签名证书（* .pfx）所需的密码。

* **键入：** 类似
* **必需：** 不
* **位置：** Named
* **默认值：** 内容

### <a name="-forcecontinue"></a>-Forcecontinue 只能

如果已指定，则脚本将忽略错误并尝试更新所有应用程序的发布服务器信息。

* **键入：** 类似
* **必需：** 不
* **位置：** Named
* **默认值：** 内容
