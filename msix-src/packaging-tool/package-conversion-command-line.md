---
title: 使用命令行接口创建包
description: 本文介绍如何使用 .MSIX 打包工具的命令行接口创建 .MSIX 包。
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 726d743106932f7febac35ad70256b867146101d
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073713"
---
# <a name="conversion-with-the-command-line"></a>用命令行转换

## <a name="automate-conversion-of-windows-installers-to-msix-packages-using-scripts"></a>使用脚本自动将 Windows 安装程序转换为 .MSIX 包

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

.MSIX 打包工具支持用于创建 .MSIX 应用程序包的命令行界面。 这样，你便可以自动执行将应用安装程序重新打包的过程并执行批量转换。

有关演示如何自动执行打包、签名、管理和分发 MSIX 包的过程的示例 PowerShell 和 Bash 脚本，请参阅 MSIX 工具包的 [scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) 文件夹。

## <a name="use-the-command-line-with-the-command-prompt"></a>在命令提示符下使用命令行

若要为应用程序创建新的 .MSIX 包，请在管理员命令提示符窗口中运行 `MsixPackagingTool.exe create-package` 命令。

下面是可以作为命令行自变量传递的参数：

|**参数** |    **描述**|
|---------|---------|
|-? --help  |显示帮助信息|
|--template | [必需] 转换模板 XML 文件的路径，该文件包含用于此次转换的包信息和设置|
|--virtualMachinePassword   | [可选] 转换环境使用的虚拟机的密码。 注意：模板文件必须包含 VirtualMachine 元素，并且 Settings：： AllowPromptForPassword 特性不得设置为 true。|
|-v --verbose   |[可选] 在控制台中输出详细日志。|

例如：

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> App-V 5.x 目前受支持，可以通过命令行转换。 其中包括功能。

通过使用应用程序进行转换，可以通过 .MSIX 打包工具[生成命令行模板文件](generate-template-file.md)，也可以从示例模板生成一个。
