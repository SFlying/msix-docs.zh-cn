---
title: 如何捆绑 MSIX 包
description: 本文介绍如何使用 .MSIX 打包工具在转换应用程序安装程序的 x86 和 x64 版本后创建捆绑包。
ms.date: 10/25/2018
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: b8dfc909cd962e23970397bf673f9a3041cbcb4f
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328741"
---
# <a name="bundle-msix-packages"></a>捆绑 MSIX 包

本文介绍在使用 MSIX 打包工具转换 Windows 安装程序的 x86 和 x64 版本之后，创建捆绑包的过程。 

将安装程序的多个体系结构版本捆绑成一个实体后，只需将该捆绑包上传到 Store 或另一个分发位置。 Windows 10 部署平台可以识别 .msixbundle 包类型，只会下载适用于你设备的体系结构的文件。 请记住，如果你决定分发特定应用的 .msixbundle，则再也不能像以前一样只是分发某个 MSIX 包。 

以下部分提供了生成 .msixbundle 的每个步骤。 其中假设已将 Windows 安装程序的[现有 x86 和 x64 版本转换为](https://docs.microsoft.com/windows/msix/mpt-best-practices) MSIX 包。 

### <a name="setup"></a>“安装程序”

需要完成以下设置才能成功生成 MSIX 捆绑包：

- [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)（1809 或更高版本）
- 已转换的 x64 和 x86 MSIX 包

## <a name="step-1-find-makeappxexe"></a>步骤1：查找 Makeappx.exe

[MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) 是 Windows 10 SDK 中提供的一个工具，可用于打包和捆绑 MSIX 包。 你将使用此工具将两个 MSIX 包捆绑到一起。

MakeAppx.exe 可用于提取 Windows 10 应用包或捆绑包的文件内容。 它还可以加密和解密应用包与捆绑包。

安装 Windows 10 SDK 后，通常可在以下位置找到 MakeAppx.exe：

- [x86] - C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64] - C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>步骤2：捆绑包

使用 MakeApp.exe 捆绑包的最简单方法是将要捆绑在一起的所有包添加到一个文件夹中。 该目录只能包含要捆绑的包，而不能包含其他任何内容。

将要捆绑的应用包移到一个目录中，如以下屏幕截图所示。

![在目录中捆绑包](images/bundle-pic1.png)

>[!NOTE]
> MakeAppx.exe 只会捆绑具有相同标识的包，这意味着，AppID、发布者和版本需要相同。 只有应用程序包的包处理器体系结构可以不同。

MakeAppx.exe 使用以下命令行语法。

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d input_directorypath 
/p <filepath>.msixbundle
```

下面是一个示例命令。

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d c:\AppPackages\ 
/p c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

运行该命令后，将在指定的路径中创建一个未签名的 .msixbundle。 绑定之前，不需要对包进行签名。  

## <a name="step-3-sign-the-bundle"></a>步骤3：为捆绑签名

创建捆绑包后，必须先对包进行签名，然后才能将应用分发给用户或进行安装。 

若要对包进行签名，需要具有常规代码签名证书并使用 Windows 10 SDK 中的 SignTool.exe。 

我们强烈建议你使用证书颁发机构提供的受信任证书，因为这样可以使包在最终用户设备上无缝分发和部署。 一旦有权访问私有证书（.pfx 文件），就可以对包进行签名，如下所示。

>[!NOTE]
> SignTool.exe 在 Windows 10 SDK 中 MakeAppx.exe 所在的目录中提供。 

SignTool.exe 使用以下命令行语法。

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd <Hash Algorithm> /a 
/f <Path to Certificate>.pfx /p <Your Password> <File path>.msixbundle
```

下面是一个示例命令。

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

有关使用 SignTool.exe 为应用包签名的详细信息，请参阅[此文](../package/sign-app-package-using-signtool.md)。 

成功为捆绑包签名后，可将其放在网络共享或任何内容分发网络上，以将其分发给用户。 

