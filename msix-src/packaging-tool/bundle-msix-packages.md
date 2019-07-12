---
title: 如何捆绑 MSIX 包
description: 捆绑不同体系结构的 MSIX 包
ms.date: 10/25/2018
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 14045d6bd5d78ba364c82b190d065d8630983fdb
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829295"
---
# <a name="bundle-msix-packages"></a>捆绑 MSIX 包 

本文介绍在使用 MSIX 打包工具转换 Windows 安装程序的 x86 和 x64 版本之后，创建捆绑包的过程。 

将安装程序的多个体系结构版本捆绑成一个实体后，只需将该捆绑包上传到 Store 或另一个分发位置。 Windows 10 部署平台可以识别 .msixbundle 包类型，只会下载适用于你设备的体系结构的文件。 请记住，如果你决定分发特定应用的 .msixbundle，则再也不能像以前一样只是分发某个 MSIX 包。 

以下部分提供了生成 .msixbundle 的每个步骤。 其中假设已将 Windows 安装程序的[现有 x86 和 x64 版本转换为](https://docs.microsoft.com/windows/msix/mpt-best-practices) MSIX 包。 

### <a name="setup"></a>安装
需要完成以下设置才能成功生成 MSIX 捆绑包：
- [Windows 10 SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)（1809 或更高版本）
- 已转换的 x64 和 x86 MSIX 包 

## <a name="step-1-find-makeappxexe"></a>第 1 步：找到 MakeAppx.exe
[MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) 是 Windows 10 SDK 中提供的一个工具，可用于打包和捆绑 MSIX 包。 你将使用此工具将两个 MSIX 包捆绑到一起。 

MakeAppx.exe 可用于提取 Windows 10 应用包或捆绑包的文件内容。 它还可以加密和解密应用包与捆绑包。

安装 Windows 10 SDK 后，通常可在以下位置找到 MakeAppx.exe： 
- [x86] - C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64] - C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>步骤 2：捆绑包

使用 MakeApp.exe 捆绑包的最简单方法是将要捆绑在一起的所有包添加到一个文件夹中。 该目录只能包含要捆绑的包，而不能包含其他任何内容。 

将要捆绑的应用包移到一个目录中，如以下屏幕截图所示。

![在目录中捆绑包](images/bundle-pic1.png)

>[!NOTE] 
> MakeAppx.exe 只会捆绑具有相同标识的包，这意味着，AppID、发布者和版本需要相同。 只有应用程序包的包处理器体系结构可以不同。 

MakeAppx.exe 使用以下命令行语法。

```命令提示符 C:\>"C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d input_directorypath /p <filepath>.msixbundle
```

Here is an example command.

```
C:\>"C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d c:\AppPackages\ /p c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

After running the command, an unsigned .msixbundle will be created in the path specified. Packages do not need to be signed before bundling.  

## Step 3: Sign the bundle

After you create the bundle, you must sign the package before you can distribute the app to your users or install it. 

To sign a package, you will need a general code signing certificate and use SignTool.exe from the Windows 10 SDK. 

We strongly recommend that you use a trusted cert from certificate authority as that allows for the package to be distributed and deployed on your end users devices seamlessly. Once you have access to the private certificate (.pfx file), you can sign the package as shown below.

>[!NOTE]
> SignTool.exe is available in the same directory as MakeAppx.exe in the Windows 10 SDK. 

SignTool.exe has the following command line syntax.

```Command Prompt
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd <Hash Algorithm> /a 
/f <Path to Certificate>.pfx /p <Your Password> <File path>.msixbundle
```

下面是一个示例命令。

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

有关使用 SignTool.exe 为应用包签名的详细信息，请参阅[此文](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool)。 

成功为捆绑包签名后，可将其放在网络共享或任何内容分发网络上，以将其分发给用户。 

