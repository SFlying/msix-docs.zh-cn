---
title: 使用 SignTool 对应用包进行签名
description: 本文介绍如何使用 SignTool 手动对应用包进行签名或捆绑证书。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: 484f2ba6df7044b628154fd73990089652cede20
ms.sourcegitcommit: e703ffe4c635d9b127ecf8c02e087370b676aa9c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108170"
---
# <a name="sign-an-app-package-using-signtool"></a>使用 SignTool 对应用包进行签名

**SignTool** 是命令行工具，用于对应用包或带有证书的捆绑包进行数字签名。 证书可以由用户创建（用于测试目的）或由公司颁发（用于分发）。 登录应用包可验证用户登录后应用数据未经更改，并且可确认登录用户或公司的身份。 **SignTool** 可对已加密或未加密的应用包和捆绑包进行签名。

> [!IMPORTANT] 
> 如果你使用 Visual Studio 开发你的应用，建议使用 Visual Studio 向导创建应用包并对其进行签名。 有关详细信息，请参阅使用[Visual Studio 打包 UWP 应用](packaging-uwp-apps.md)和[使用 visual Studio 从源代码打包桌面应用](../desktop/desktop-to-uwp-packaging-dot-net.md)。

有关代码签名和证书的一般详细信息，请参阅[代码签名简介](https://docs.microsoft.com/windows/desktop/SecCrypto/cryptography-tools)。

## <a name="prerequisites"></a>先决条件

- **打包应用程序**  
    若要了解手动创建应用包的详细信息，请参阅[使用 MakeAppx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md)。

- **有效的签名证书**  
    有关创建或导入有效签名证书的详细信息，请参阅[创建或导入应用包签名证书](create-certificate-package-signing.md)。

- **SignTool**  
    根据 SDK 的安装路径，以下是 **SignTool** 在 Windows 10 电脑上的位置：
    - x86： C:\Program Files （x86） \Windows Kits\10\bin arch>\\&lt;sdk 版本&gt;\x86\SignTool.exe
    - x64： C:\Program Files （x86） \Windows Kits\10\bin arch>\\&lt;sdk 版本&gt;\x64\SignTool.exe

## <a name="using-signtool"></a>使用 SignTool

**SignTool** 具有对文件进行签名，验证签名或时间戳，删除签名等功能。 为了实现对应用包进行签名的目的，我们将重点关注**签名**命令。 有关 **SignTool** 的完整信息，请参阅 [SignTool](https://docs.microsoft.com/windows/desktop/SecCrypto/signtool) 参考页面。

### <a name="determine-the-hash-algorithm"></a>确定哈希算法

在使用 **SignTool** 对你的应用包或捆绑包进行签名时，**SignTool** 中所用的哈希算法必须与你用于打包应用的算法相同。 例如，如果你使用了采用默认设置的 **MakeAppx.exe** 创建应用包，则在使用 **SignTool** 时必须指定 SHA256，因为这是 **MakeAppx.exe** 所用的默认算法。

要找出打包应用时用到了哪种哈希算法，则需要提取应用包的内容并检查 AppxBlockMap.xml 文件。 要了解如何解压缩/提取应用包，请参阅[从应用包或捆绑包中解压缩文件](create-app-package-with-makeappx-tool.md)。 哈希方法存在于 BlockMap 元素中，且具有以下格式：

```xml
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap"
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

下表显示了每个 HashMethod 值及其对应的哈希算法：


| HashMethod 值                              | 哈希算法 |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> 由于 **SignTool** 的默认算法是 SHA1（在 **MakeAppx.exe** 中不可用），所以在使用 **SignTool** 时必须始终指定哈希算法。

### <a name="sign-the-app-package"></a>对应用包进行签名

在具备所有先决条件并且已确定使用哪种哈希算法打包应用后即可登录。 

**SignTool** 程序包签名相关的常规命令行语法为：

```syntax
SignTool sign [options] <filename(s)>
```

用于对你的应用进行签名的证书必须为.pfx 文件或安装在证书存储中。

若要对带有 .pfx 文件格式证书的应用包进行签名，请使用以下语法：

```syntax
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```

请注意，借助 `/a` 选项可让 **SignTool** 自动选择最佳的证书。

如果你的证书不是.pfx 文件，请使用以下语法：

```syntax
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

或者，你可以使用以下语法指定所需证书的 SHA1 哈希算法，而不是&lt;证书名称&gt;：

```syntax
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```

```syntax
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

请注意，某些证书不使用密码。 如果你的证书没有密码，可忽略示例命令中的“/p &lt;你的密码&gt;”。

使用有效证书对应用包进行签名后即可将你的应用包上传至应用商店。 有关将应用上传和提交至应用商店的详细指南，请参阅[应用提交](https://docs.microsoft.com/windows/uwp/publish/app-submissions)。
