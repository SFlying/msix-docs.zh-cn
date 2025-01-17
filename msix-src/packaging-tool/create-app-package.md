---
title: 通过任何桌面安装程序创建 MSIX 包
description: " (MSI、EXE、ClickOnce 或 App-v) 的任何桌面安装程序创建 .MSIX 包"
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b9aea569ea6c66935c8d5f6fdc1e86e429b18760
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091035"
---
# <a name="create-an-msix-package-from-any-desktop-installer-msi-exe-clickonce-or-app-v"></a> (MSI、EXE、ClickOnce 或 App-v) 的任何桌面安装程序创建 .MSIX 包

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

可以使用 [.Msix 打包工具](tool-overview.md) ，通过以下任意选项创建 .msix 应用程序包：

- MSI
- EXE
- ClickOnce
- App-V
- 脚本
- 手动安装

本文档将引导你了解如何获取现有资产，并将其转换为 .MSIX。

在开始转换之前，建议确保 [了解安装程序](know-your-installer.md)以及是否会转换。

我们还建议遵循最佳做法来 [配置环境](prepare-your-environment.md) 和 [.msix 打包工具](tool-best-practices.md) 以进行转换。

> [!NOTE]
> MSIX 打包工具当前支持 APP-V 5.1。 如果你有包含 App-v 4.x 的包，则建议你使用源安装程序转换为 .MSIX。

首次启动该工具时，系统会提示你同意发送遥测数据。 必须注意，共享的诊断数据仅来自应用，永远不会用于识别你的身份或与你联系。

创建 **应用程序包** 是最常用的选项。 这是你将从安装程序创建 .MSIX 包或手动安装应用程序负载的位置。

![pic1](images/pic1.PNG)

## <a name="packaging-method"></a>打包方法

选择适用于你的转换计算机的选项：

- 如果你已经在干净的环境中工作，请选择 " **在此计算机上创建包"**
- 如果要连接到现有虚拟机或远程计算机，请选择 " **在远程计算机上创建包"**
  - 你将需要 [设置你的远程计算机](remote-conversion-setup.md) ，然后才能对其进行转换
- 如果要转换的计算机上存在本地虚拟机，请选择 "**在本地虚拟机上创建包**"
  - 请注意，我们仅支持 Hyper-v 虚拟机，如果要使用其他虚拟化产品，可以使用远程计算机选项进行连接。

- 点击“下一步”
  
## <a name="prepare-computer"></a>准备计算机

接下来，“准备计算机”页会提供用于准备要打包的计算机的选项。****

需要 **.Msix 打包工具驱动程序** ，并且如果未启用该工具，该工具会自动尝试启用它。 该工具首先会检查 DISM，以确定是否已安装该驱动程序。 如果遇到问题，请尝试检查我们的 [疑难解答文档](tool-known-issues.md)，如果问题仍然存在，请记录 [反馈中心问题](tool-known-issues.md#sending-feedback) 。

> [!NOTE]
> MSIX 打包工具驱动程序会监视系统，以捕获安装程序在系统上所做的更改，使 MSIX 打包工具能够基于这些更改创建包。

**Windows 更新处于活动状态** 我们会暂时禁用打包期间的 Windows 更新，这样我们就不会收集任何无关的数据了。

- “等待重新启动”复选框默认已禁用。**** 如果系统提示等待中的操作需要重新启动，则你需要手动重启计算机，然后再次启动该工具。 这只是建议的操作，而不是必需的。

- [可选] 选中“Windows Search 处于活动状态”对应的复选框，如果你选择禁用搜索服务，请选择“禁用所选项”。********
    - 这只是建议的操作，而不是必需的。
    - 禁用后，该工具会将状态字段更新为“已禁用”。****

- 可有可无选中 " **SMS 主机处于活动状态** " 复选框，如果选择禁用主机服务，则选择 " **禁用** "。
    - 这只是建议的操作，而不是必需的。
    - 禁用后，该工具会将状态字段更新为“已禁用”。****

计算机准备完成后，单击“下一步”。****

## <a name="choose-the-installer-you-want-to-package"></a>选择要打包的安装程序

您需要做的第一件事就是了解要转换的安装程序会发生什么情况。 对于任何这些安装程序，可以在此处指定它们以简化工作流，也可以在稍后在工作流中安装时手动运行。

### <a name="msi-installers"></a>MSI 安装程序

如果要转换 .msi 安装程序，只需浏览到该安装程序，并指定 .msi。 如果有随附的 .mst 或 .msp 文件，则可以在 "安装程序参数" 字段中指定。 在此处指定你的 .msi 的优点之一是，我们可以从中提取所有包信息，从而为你节省了下一个转换步骤。

### <a name="app-v-installers"></a>App-v 安装程序

如果使用 App-v 进行转换，则这是一个非常简单的过程。 只需指定 App-v 文件，即可快速跟踪到 "创建 .MSIX" 页。 这是因为包的清单只需转换为 .MSIX 包，然后就可以像 .MSIX 一样工作。 此处所述的是工具仅支持 app-v 5.1-如果 App-v 为版本4.x，则建议使用源安装程序，然后将其直接转换为 .MSIX。

### <a name="exe-installers"></a>EXE 安装程序

如果要转换 .exe 安装程序，则可以在此时指定安装程序。 由于与 exe 缺少格式一致性，需要手动输入安装程序的包信息。 

### <a name="clickonce-installers"></a>ClickOnce 安装程序

如果要转换 ClickOnce 安装程序，则可以在此时指定安装程序。 与 .exe 类似，你将需要手动输入安装程序的包信息。 

### <a name="scripts"></a>脚本
如果使用脚本来安装应用程序，则可以在此处指定命令行。 或者，你可以将此字段留空，然后在 [安装阶段](#installation)手动运行脚本。

### <a name="manual-installation"></a>手动安装
如果希望手动运行安装程序，或者手动执行安装程序的操作，则可以将安装程序字段留空，并在 [安装阶段](#installation)执行安装程序所需的操作。 

如果你正在尝试生成转换模板文件，则不指定安装程序将无法执行此操作。

如果有任何安装程序参数，则可以在提供的字段中输入所需的参数。 此字段接受任何字符串。

### <a name="signing-preference"></a>签名首选项 

在 " **签名首**选项" 下，选择签名选项。 还可以在设置中将其设置为默认值，这将在每次转换时节省一些步骤。

- **用 Device Guard 签名签名** 此选项允许你登录到已配置为与 Device Guard 签名一起使用的 Microsoft Active Directory 帐户，该帐户是 Microsoft 提供的一种签名服务，你无需提供自己的证书。 [在此处](../package/signing-package-device-guard-signing.md)了解有关如何设置帐户和 Device Guard 签名的详细信息。 
- **使用证书签名 ( .pfx) ** 浏览并选择 .pfx 证书文件。 如果证书受密码保护，请在密码框中键入密码。
- **指定 .cer 文件 (不签署) ** 此选项允许您指定 .cer 文件。 如果你不希望对包进行签名，但你想要确保发布服务器信息与将用于签名的证书的使用者匹配，这会很有用。 
- **不对包进行签名** 如果以后要对包进行签名，请选择此选项。 注意：如果 .MSIX 包未签名，则无法安装该程序包
- 签名时，强烈建议向证书添加 **时间戳** ，以便证书的有效性可 outlast 其到期日期。 接受的格式为 RFC 3161 [时间戳服务器 URL](/windows/win32/seccrypto/signtool)。

> [!NOTE]
> 不支持使用 SHA1 证书对 .MSIX 包格式应用程序进行签名。

单击“下一步”继续。

## <a name="package-information"></a>包信息

选择在现有虚拟机上打包应用程序后，必须提供有关应用的信息。 该工具会尝试根据安装程序提供的信息自动填充这些字段。 如果需要，始终可以选择更新条目。 带星号 * 的字段是必填的。 如果条目无效，系统会提供内联帮助。

- 包名称：
    - 必填字段，对应于清单中的包标识名称，用于描述包的内容。
    - 不会向最终用户显示。
    - 区分大小写，不能包含空格。
    - 可以接受由字母数字、句点和短划线字符构成的，长度为 3 到 50 个字符的字符串。
    - 不能以句点结尾，必须是下列其中一项： "CON"、"PRN"、"AUX"、"NUL"、"COM1"、"COM2"、"COM3"、"COM4"、"COM5"、"COM6"、"COM7"、"COM8"、"COM9"、"LPT1"、"LPT2"、"LPT3"、"LPT4"、"LPT5"、"LPT6"、"LPT7"、"LPT8"、"LPT9" 和 ""。
- 包显示名称：
    - 必填字段，对应于清单中的包，在开始菜单和设置页中向用户显示友好的包名称。
    - 该字段接受长度为 1 到 256 个字符的字符串，并且可本地化。
- 发布者名称：
    - 必填字段，对应于包，用于描述发布者信息。
    - “发布者”属性必须与用于对包签名的证书的发布者使用者信息相匹配。
    - 此字段接受长度为 1 到 8192 个字符且符合可分辨名称正则表达式的字符串："(CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")(, ((CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")))*"。
- 发布者显示名称：
    - 必填字段，对应于清单中的包，在应用安装程序和设置页中向用户显示友好的发布者名称。
    - 该字段接受长度为 1 到 256 个字符的字符串，并且可本地化。
- 版本：
    - 必填字段，对应于清单中的包，用于描述包的版本号。
    - 此字段接受四表示法形式的版本字符串： "主要版本. 次要版本. 内部版本. 修订版本"。
- 说明:
    - 此字段是可选的。
- 安装位置：
    - 这是安装程序要将应用程序有效负载复制到的位置（通常是 Programs Files 文件夹）。
    - 此字段是可选的，但建议在 "Program Files" 文件夹外安装应用负载时使用。
    - 浏览到并选择文件夹路径。
    - 在执行每项应用程序安装操作时，请确保此文件与安装程序的安装位置相匹配。 
- 向此包添加对 .MSIX Core 的支持。 
    - 选中此复选框后，将显示一个下拉框，此下拉框将 aloow 你为正在生成的包选择 Windows 版本 [.Msix 核心](../msix-core/msixcore.md) 支持。

## <a name="installation"></a>安装

- 这是工具监视和捕获应用程序安装操作的安装阶段。
- 该工具将在之前指定的环境中启动安装程序，需要通过安装程序向导来安装该应用程序。
    - 请确保安装路径与前面在包信息页中定义的内容相匹配。
    - 可能需要在桌面中为新安装的应用程序创建快捷方式。
    - 完成应用程序安装向导后，请确保完成或关闭安装向导。
    - 如果需要运行多个安装程序，此时可以手动运行。
    - 如果应用需要其他必备组件，则需要立即安装。
    - 如果应用程序需要 .Net 3.5/20，请将可选功能添加到 Windows。
- 如果以前未指定安装程序，则可以在此处手动运行安装程序或脚本。
- 如果 [安装程序需要重新启动](support-restart.md)，则可以执行手动重新启动，或使用 "重启" 按钮执行重新启动，并且在重启后，你将在转换过程中返回到这一点。
- 安装完应用程序后，单击“下一步”。****

## <a name="manage-first-launch-tasks"></a>管理初始启动任务

此页显示工具捕获到的应用程序可执行文件。 我们建议启动应用程序至少一次，以捕获所有初始启动任务。

可以通过选择可执行文件，然后单击 " **运行**" 来启动该可执行文件。 您还可以通过选择它，然后单击 " **删除**" 来删除任何不必要的入口点。

如果有多个应用程序，请选中对应于主入口点的框。 如果在此处未看到应用程序 .exe，请手动浏览并运行它。 然后 **刷新列表**。

单击“下一步”。此时会出现一个弹出窗口，提示你确认已完成应用程序的安装和初始启动任务的管理。****

- 如果已完成，请单击“是，继续”。****
- 如果尚未完成，请单击“否，未完成”。**** 随后你会返回到上一页，可在其中启动应用程序、安装或复制其他文件和 DLL/可执行文件。

## <a name="services-report"></a>服务报表

从1.2019.1220.0 版本的 .MSIX 打包工具开始，你可以将 [安装程序与服务进行](convert-an-installer-with-services.md)转换，因此我们添加了一个服务报表页。 如果未检测到任何服务，则仍会看到此页，但它将为空，并显示页面顶部未检测到任何服务的消息。 

"服务报表" 页将列出在转换过程中检测到的服务。 所 **含** 的表中将显示具有所需的所有信息并支持的服务。 **排除**的表中将显示需要其他信息、需要修复或不受支持的服务。

若要修复服务或查看有关服务的其他数据，请双击表中的服务条目以查看弹出窗口，其中包含有关服务的详细信息。 如果需要，可以编辑此信息。

- **项名称：** 服务的名称。 这是不可编辑的。
- **说明：** 服务项的说明。
- **显示名称：** 服务的显示名称。
- **映像路径：** 服务可执行文件的位置。 这是不可编辑的。
- **启动帐户：** 服务的启动帐户。
- **启动类型：** 服务的启动类型。 支持 **自动**、 **手动**和 **禁用**。
- **参数：** 要在服务启动时运行的参数。
- **依赖项：** 服务的依赖关系。

修复了某个服务后，可以将其移动到 **包含** 的表中，或者，如果不希望在最终包中使用它，也可以选择将它保留在 **排除** 的表中。 有关其他信息，请参阅 [服务文档](convert-an-installer-with-services.md)。

## <a name="create-package"></a>创建包

- 提供用于保存 MSIX 包的位置。
- 默认情况下，包保存在本地应用数据文件夹中。
- 可以在“设置”菜单中定义默认保存位置。
- 如果您要生成转换模板文件，则还可以为该模板文件指定其他保存位置（如果您不想将其保存在与 .MSIX 包相同的位置。
- 如果要在保存 .MSIX 包之前继续编辑包的内容和属性，可以选择 " [包编辑器](package-editor.md) " 并将其转到 "打包编辑器"。
- 单击“创建”以创建 MSIX 包。****

创建包时，会显示一个弹出窗口。 此弹出窗口将包含保存位置，并链接到新创建包的文件位置。 它还包括指向 .MSIX 打包工具的日志文件位置的链接。 可以关闭此弹出窗口并重定向到欢迎页。 还可以选择 " [包编辑器](package-editor.md) " 来查看和修改包的内容和属性。