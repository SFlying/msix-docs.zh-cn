---
author: c-don
title: 如何捆绑 MSIX 包
description: 捆绑 MSIX 包的不同的体系结构
ms.author: cdon
ms.date: 10/25/2018
ms.topic: article
keywords: windows 10 msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 54e6f063aa05c1a297725e6c1c7a091ebffbb043
ms.sourcegitcommit: 9bbb116d1984082123f694130b4d6cc078fa8510
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983439"
---
# <a name="bundle-msix-packages"></a>捆绑 MSIX 包 

本文介绍了转换您的 Windows 安装程序使用 MSIX 打包工具的 x86 和 x64 版本后创建一个捆绑包的过程。 

通过捆绑到一个实体到安装程序的多个体系结构版本，仅此捆绑包需要上传到应用商店或另一个分发位置。 Windows 10 部署平台已意识到.msixbundle 包类型，并且将仅下载适用于你的设备的体系结构的文件。 请记住，如果您决定要分发的某个特定应用.msixbundle，你不能还原为分发只是一个 MSIX 包。 

以下部分提供循序渐进的方法来生成.msixbundle。 它假定您已有[转换现有的 x86 和 x64 版本](https://docs.microsoft.com/windows/msix/mpt-best-practices)MSIX 包的 Windows 安装程序。 

### <a name="setup"></a>安装
你将需要以下安装程序成功生成 MSIX 捆绑包：
- [Windows 10 SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) (版本 1809年或更高版本)
- 已转换的 x64 和 x86 MSIX 包 

## <a name="step-1-find-makeappxexe"></a>第 1 步：查找 MakeAppx.exe
[MakeAppx.exe](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)是一个工具进行打包和捆绑 MSIX 包的 Windows 10 SDK 中提供。 将使用此工具以两个 MSIX 包捆绑在一起。 

MakeAppx.exe 可用于提取文件内容的 Windows 10 应用包或捆绑包。 它还进行加密和解密应用程序包和捆绑包。

安装 Windows 10 SDK 后，通常是以下位置找到 MakeAppx.exe: 
- [x86] - C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe
- [x64] - C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\MakeAppx.exe

## <a name="step-2-bundle-the-packages"></a>步骤 2：捆绑包

捆绑包 MakeApp.exe 的最简单方法是添加你想要捆绑在一起在一个文件夹中的所有包。 该目录必须是免费的所有其他包除外，是将其捆绑。 

将移动你想要绑定到一个目录，如以下屏幕截图中所示的应用包。

![在目录中的捆绑包](images/bundle-pic1.png)

>[!NOTE] 
> MakeAppx.exe 捆绑包的包具有相同的标识，这意味着 AppID、 发布服务器、 版本需要相同。 只有包处理器体系结构的应用程序包可能会不同。 

MakeAppx.exe 具有以下的命令行语法。

'' C： 的命令提示符\>"C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\MakeAppx.exe" bundle /d input_directorypath /p <filepath>.msixbundle
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

下面是示例命令。

```
C:\> "C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\SignTool.exe" sign /fd SHA256 /a 
/f c:\private-cert.pfx /p aaabbb123 c:\MyLOBApp_10.0.0.0_ph32m9x8skttmg.msixbundle
```

有关对使用 SignTool.exe 应用程序包进行签名的详细信息，请参阅[这篇文章](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool)。 

已成功对捆绑进行签名后, 您就可以将其托管在网络共享上或任何内容分发网络，以将其分配到你的用户。 

