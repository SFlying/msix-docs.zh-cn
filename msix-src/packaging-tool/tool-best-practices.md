---
title: 适用于 MSIX 打包工具的最佳做法
description: 本文介绍适用于将应用重新打包到 MSIX 以及使用 MSIX 打包工具的最佳做法。
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a22f4cbc2f96746fea48cb1bca1199e6006f938b
ms.sourcegitcommit: 536d6969cde057877ecdd8345cfb0dc12c9582f2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2020
ms.locfileid: "78909611"
---
# <a name="best-practices-for-the-msix-packaging-tool"></a>适用于 MSIX 打包工具的最佳做法

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

如果尚未配置环境以进行转换，则可以遵循我们的[环境最佳实践](prepare-your-environment.md)建议，然后回到此处设置 .Msix 打包工具。 在开始任何转换之前，建议在 .MSIX 打包工具中配置设置，以便每次简化工作流。

### <a name="tool-defaults"></a>工具默认值

- **生成包含每个包的命令行**此设置将使你自动生成一个命令行模板文件，以便在稍后通过命令行重新打包同一应用程序（例如新版本）时，你可以为该应用程序预配置的命令行模板文件。 你将需要提供安装程序，以便在工作流期间生成模板文件。
- **默认情况下，选择 "准备计算机" 的所有修补程序**此设置允许你预先选择所有建议的修复程序，以便在 "准备计算机" 阶段，你只需选择禁用所有修复，无需单独选择它们。
- **强制实施 Microsoft Store 版本控制要求**如果打算通过 Microsoft Store 来部署应用程序，则应确保选中此选项，使其符合应用商店要求（这将影响包版本要求和最低操作系统版本支持）。 如果未选中此选项，则包的最低版本设置为 Windows 10 1709，你将对包版本4位数字具有完全控制。 如果选中此选项，则包的最低版本设置为 Windows 10 1809，版本必须以. 0 （例如1.5.6.0）结束。
- **生成包时添加包完整性**如果选择此选项，则包完整性将自动添加到生成的所有包中。 Windows 10 2004 及更高版本支持[包的完整性](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity)。
- **默认保存位置**指定将保存生成的包和关联文件的默认保存位置。
- **默认安装程序浏览位置**指定用于查找要转换的安装程序的默认位置。
- **服务器端口号**指定 .MSIX 打包工具的服务器端口号。 如果你打算使用[远程计算机](remote-conversion-setup.md)进行转换，则这一点很有用。 
- **环境首选项**指定每个转换的默认环境。
- **签名首选项**指定在转换应用程序时进行签名的默认选项。 需要对 .MSIX 包进行签名才能安装它。 你可以从几个选项中进行选择，以用于你的签名首选项。
    - 使用 Device Guard 签名签名-如果企业中没有受信任的证书，则建议使用此选项。 在选择此选项之前，需要执行一些步骤来启用[设备保护签名](../package/signing-package-device-guard-signing.md)。 
    - 使用证书签名（.pfx）-如果已有在企业中使用的受信任证书，建议使用此选项。
    - 指定 .cer 文件（不签名）-如果不想在转换时进行签名，但要确保发布服务器信息在签名时有效，则可以选择此选项。
    - 不对包进行签名。 -如果你想要使用其他方法对包进行签名，或者稍后在生成包后进行签名，则可以选择此选项。
    我们还建议你向签名首选项添加一个时间戳服务器 url （如果适用），以便即使你的证书过期也能安装应用程序。

### <a name="other-settings"></a>其他设置

- **文件和注册表排除**项尽管我们有一组默认的排除项，但我们建议你根据特定需求查看并添加或删除任何排除项。 
- **安装程序退出代码**如果要在转换过程中触发重启的特定安装程序退出代码，可以在此处指定这些代码。 默认情况下，我们已添加了常用文件，但如果你不希望触发重新启动，则可以将其删除。 请注意，如果您使用的是 UI，则打包工具将永远不会自动触发重新启动，但如果您使用的是命令行选项，则会重新启动。 
 
你还可以使用这些[说明](duplicate-tool-settings-across-devices.md)导入或导出你的设置以进行共享。 

## <a name="best-practices-during-repackaging"></a>重新打包过程中的最佳做法

使用 .MSIX 打包工具时，我们还建议你在重新打包过程中执行以下操作：

- 打包 ClickOnce 安装程序时，有必要将快捷方式发送到桌面（如果安装程序尚未执行该操作）。 一般情况下，始终记得为主应用可执行文件将快捷方式发送到桌面是很好的做法。
- 当创建修改包时，需要在工具 UI 中声明父应用程序的包名称（标识名称），以便该工具在修改包的清单中设置正确的包依赖项。
- 在 "**准备计算机**" 页中执行准备步骤是可选的，但强烈建议这样做，因为这将有助于减少包中的任何无关数据。
- 需要对包进行签名以进行安装，但我们也建议您将证书加盖时间戳，以便即使证书过期也能安装应用程序。
- "**包信息**" 页中的 "声明安装位置" 字段是可选的。 请确保此路径与应用程序安装程序的安装位置相匹配。

## <a name="best-practices-for-testing-your-msix-package"></a>测试 .MSIX 包的最佳实践

建议你在转换后测试 .MSIX 包，因为在环境设置过程中指定了。 你应在未安装上一个安装程序的其他计算机上测试 .MSIX 包，以便你可以确保部署 .MSIX 包时，它将具有所需的所有组件，并且不会从以前的安装程序中提取任何内容。 这可以通过新的虚拟机（如[快速创建 VM](Quick-Create-VM.md)）来实现，如果在开始转换之前创建了检查点，则可以通过恢复转换计算机来实现此目的。
