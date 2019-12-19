---
title: 疑难解答提示
description: 本文列出了客户在使用 .MSIX Core 时可能遇到的错误代码和问题
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 1833991c9f052e1379f79fa34332734fab46322b
ms.sourcegitcommit: 0adaf0b61b5d259d3d157bc3255d56ead0655c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/18/2019
ms.locfileid: "75187474"
---
# <a name="troubleshooting-tips-for-msix-core"></a>.MSIX 核心故障排除提示 

在各种版本的 Windows 上安装带有 .MSIX Core 的 .msix 包时，可能会遇到一些问题。 本文列出了要使用的错误代码和故障排除提示。 

## <a name="error-codes"></a>错误代码 
下面是可能会遇到的常见错误消息。 

| 错误代码 |描述 |
|------------|------------|
| 0x8BAD0042 | 这通常意味着未安装用于对应用进行签名的证书。 若要解决此情况，请安装证书，然后重试| 
| 0x80070032 | 包中包含 .MSIX Core 不支持的注释。 例如，不支持包支持框架的某些功能。 这些是包支持框架，可调用在安装结束时运行的脚本，设置为运行的脚本等于 false 或格式不正确的包支持框架。 | 
|0x8BAD0071 | 此错误表示您正在尝试安装捆绑包。 .MSIX Core 当前不支持捆绑。|


当包格式有问题时，会出现以下错误。 

| 错误代码 |描述 |
|------------|:------------|
| 0x8BAD0031 | MissingAppxSignatureP7X|
| 0x8BAD0032 | MissingContentTypesXML|
| 0x8BAD0033 | MissingAppxBlockMapXML|
| 0x8BAD0034 | MissingAppxManifestXML|
| 0x8BAD0035 | DuplicateFootprintFile |
| 0x8BAD0036 | UnknownFileNameEncoding |
| 0x8BAD0037 | DuplicateFile | 

以下错误与文件问题相关

 | 错误代码 |描述 |
|------------|:------------|
| 0x8BAD0001 | FileOpen|
| 0x8BAD0002 | FileSeek|
| 0x8BAD0003 | FileRead|
| 0x8BAD0003 | FileWrite|
| 0x8BAD0004 | FileCreateDirectory  |
| 0x8BAD0005 | FileSeekOutOfRange  |

当包签名时使用的证书出现问题时，会出现以下错误。 

| 错误代码 |描述 |
|------------|:------------|
| 0x8BAD0041 | SignatureInvalid| 
| 0x8BAD0042 | CertNotTrusted|
| 0x8BAD0043 | PublisherMismatch|

你可能会遇到的其他问题

| 错误代码 |描述 |
|------------|:------------|
| 0x8BAD0011 | ZipCentralDirectoryHeader|
| 0x8BAD0012 | ZipLocalFileHeader|
| 0x8BAD0013 | Zip64EOCDRecord|
| 0x8BAD0014 | Zip64EOCDLocator |
| 0x8BAD0015 | ZipEOCDRecord |
| 0x8BAD0016 | ZipHiddenData |
| 0x8BAD0017 | ZipBadExtendedData | 

| 错误代码 |描述 |
|------------|:------------|
| 0x8BAD0051 | BlockMapSemanticError|
| 0x8BAD0052 | BlockMapInvalidData|

| 错误代码 |描述 |
|------------|:------------|
| 0x8BAD0061 | AppxManifestSemanticError |
| 0x8BAD0082 | DeflateInitialize |
| 0x8BAD0081 | DeflateWrite |
| 0x8BAD0083 | DeflateRead  |

| 错误代码 |描述 |
|------------|:------------|
| 0x8BAD1001 | XmlWarning  |
| 0x8BAD1002 | XmlError|
| 0x8BAD1003 | XmlFatal |
| 0x8BAD1004 | XmlInvalidData |

若要[在此处](https://docs.microsoft.com/windows/win32/debug/system-error-codes)搜索其他错误代码，请参阅。 

有关完整列表，请访问[.Msix Core 错误代码](https://github.com/microsoft/msix-packaging/blob/master/src/inc/public/MsixErrors.hpp)页。 

## <a name="msix-tracing-powershell-script"></a>.MSIX 跟踪 PowerShell 脚本
请参阅我们的[发布页面](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release)，并下载**msixtrace**。 这是 .MSIX 跟踪 PowerShell 脚本，它将生成日志，以帮助你在 .MSIX 安装过程中遇到问题。

使用以下命令
```
msixtrace.ps1 -wait
``` 
按照提示显示脚本来生成日志。 或使用以下命令。  
```
msixtrace.ps1 -start
```
安装 .MSIX 包。 完成后，请完成以下命令。 
```
msixtrace.ps1 -stop
```
