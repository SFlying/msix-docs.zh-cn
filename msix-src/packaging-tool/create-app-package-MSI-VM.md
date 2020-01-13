---
title: 在 VM 上通过桌面安装程序（MSI、EXE 或 App-V）创建 MSIX 包
description: 在 VM 上通过桌面安装程序（MSI、EXE 或 App-V）创建 MSIX 包
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e9cd7b8967c65f9cfcc4fb11d66650975b6bbda9
ms.sourcegitcommit: 71c49de79d061909fb1ab632ec7550227d2287bd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754887"
---
# <a name="create-an-msix-package-from-a-desktop-installer-msi-execlickonce-or-app-v"></a>从桌面安装程序创建 .MSIX 包（MSI、EXE、ClickOnce 或 App-v）

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

你可以使用[.Msix 打包工具](../mpt-overview.md)，通过 hyper-v 虚拟机（VM）上的现有 MSI、EXE、ClickOnce 或 app-v 安装程序来创建 .msix 应用程序包。 该 VM 必须符合以下要求：

建议遵循[最佳做法](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-best-practices)来配置环境和 .Msix 打包工具进行转换。 

> [!NOTE]
> MSIX 打包工具当前支持 APP-V 5.1。 如果有包含 APP-V 4.x 的包，我们建议你先将其转换为 APP-V 5.1，然后再使用 MSIX 打包工具转换为 MSIX。 

首次启动该工具时，系统会提示你同意发送遥测数据。 必须注意，共享的诊断数据仅来自应用，永远不会用于识别你的身份或与你联系。

创建应用程序包是最常用的选项。 这是你将从安装程序创建 .MSIX 包或[手动安装](https://docs.microsoft.com/windows/msix/packaging-tool/create-other-installer)应用程序负载的位置。

![pic1](images/pic1.PNG)

## <a name="packaging-method"></a>打包方法

选择适用于你的转换计算机的选项：
- 如果你已经在干净的环境中工作，请选择 "**在此计算机上创建包"**
- 如果要连接到现有 VM 或远程计算机，请选择“在远程计算机上创建包”
  - 你将需要[设置你的远程计算机](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup)，然后才能对其进行转换
- 如果要执行转换操作的计算机上有本地 VM，请选择“在本地虚拟机上创建包”
  - 单击**下一步**
  
## <a name="prepare-computer"></a>准备计算机

接下来，“准备计算机”页会提供用于准备要打包的计算机的选项。

需要 **.Msix 打包工具驱动程序**，并且如果未启用该工具，该工具会自动尝试启用它。 该工具首先会检查 DISM，以确定是否已安装该驱动程序。 如果遇到问题，请尝试检查我们的[疑难解答文档](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-known-issues)，如果问题仍然存在，请记录[反馈中心问题](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-known-issues#sending-feedback)。 

> [!NOTE]
> MSIX 打包工具驱动程序会监视系统，以捕获安装程序在系统上所做的更改，使 MSIX 打包工具能够基于这些更改创建包。

**Windows 更新处于活动状态**我们会暂时禁用打包期间的 Windows 更新，这样我们就不会收集任何无关的数据了。 

- “等待重新启动”复选框默认已禁用。 如果系统提示等待中的操作需要重新启动，则你需要手动重启计算机，然后再次启动该工具。 这只是建议的操作，而不是必需的。

- [可选] 选中“Windows Search 处于活动状态”对应的复选框，如果你选择禁用搜索服务，请选择“禁用所选项”。
    - 这只是建议的操作，而不是必需的。
    - 禁用后，该工具会将状态字段更新为“已禁用”。

- 可有可无选中 " **SMS 主机处于活动状态**" 复选框，如果选择禁用主机服务，则选择 "**禁用**"。
    - 这只是建议的操作，而不是必需的。
    - 禁用后，该工具会将状态字段更新为“已禁用”。

计算机准备完成后，单击“下一步”。

## <a name="choose-the-installer-you-want-to-package"></a>选择要打包的安装程序

单击 "**浏览**" 并在文件选取器中选择安装程序，导航到 MSI、app-v 或其他 Win 32 安装程序。

如果有任何安装程序参数，则可以在提供的字段中输入所需的参数。 此字段接受任何字符串。

在 "**签名首**选项" 下，选择签名选项。 还可以在设置中将其设置为默认值，这将在每次转换时节省一些步骤。 
- **用 Device Guard 签名签名**此选项允许你登录到已配置为与 Device Guard 签名一起使用的 Microsoft Active Directory 帐户，该帐户是 Microsoft 提供的一种签名服务，你无需提供自己的证书。 [在此处](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)了解有关如何设置帐户和 Device Guard 签名的详细信息。 
- **使用证书签名（.pfx）** 浏览并选择 .pfx 证书文件。 如果证书受密码保护，请在密码框中键入密码。
- **指定 .cer 文件（进行注释签名）** 此选项允许您指定 .cer 文件。 如果你不希望对包进行签名，但你想要确保发布服务器信息与将用于签名的证书的使用者匹配，这会很有用。 
- **不对包进行签名**如果以后要对包进行签名，请选择此选项。 注意：如果 .MSIX 包未签名，则无法安装该程序包
- 签名时，强烈建议向证书添加**时间戳**，以便证书的有效性可 outlast 其到期日期。 接受的格式为 RFC 3161 [时间戳服务器 URL](https://docs.microsoft.com/windows/win32/seccrypto/signtool)。 

单击“下一步” 以继续。

## <a name="package-information"></a>包信息

选择在现有虚拟机上打包应用程序后，必须提供有关应用的信息。 该工具会尝试根据安装程序提供的信息自动填充这些字段。 如果需要，始终可以选择更新条目。 我们都知道，带星号 * 的字段是必填的。 如果条目无效，系统会提供内联帮助。
- 包名称：
    - 必填字段，对应于清单中的包标识名称，用于描述包的内容。
    - 必须与用于对包签名的证书的使用者名称信息相匹配。
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
- 安装位置：
    - 这是安装程序要将应用程序有效负载复制到的位置（通常是 Programs Files 文件夹）。
    - 此字段是选填的，但建议填写，尤其是在 Program Files 外部安装应用有效负载时。
    - 浏览到并选择文件夹路径。
    - 在执行每项应用程序安装操作时，请确保此文件与安装程序的安装位置相匹配。
- 说明：
    - 此字段为可选字段。 

## <a name="installation"></a>安装

- 现已进入安装阶段，该工具会监视并捕获应用程序安装操作。
- 该工具会在前一阶段中已打开的虚拟机窗口中启动安装程序，你需要完成安装程序向导来安装应用程序。
    - 请确保安装路径与前面在包信息页中定义的内容相匹配。
    - 需要在桌面上为新安装的应用程序创建一个快捷方式。
    - 完成应用程序安装向导后，请确保完成或关闭安装向导。
    - 如果需要运行多个安装程序，此时可以手动运行。
    - 如果应用需要其他必备组件，则你现在需要安装这些组件。
    - 如果应用程序需要 .Net 3.5/20，请将可选功能添加到 Windows。
- 如果安装程序需要重新启动，则可以执行手动重新启动，或使用 "重启" 按钮执行重新启动，并且在重启后，你将在转换过程中返回到这一点。
- 安装完应用程序后，单击“下一步”。

## <a name="manage-first-launch-tasks"></a>管理初始启动任务

此页显示工具捕获到的应用程序可执行文件。 我们建议启动应用程序至少一次，以捕获所有初始启动任务。

如果有多个应用程序，请选中对应于主入口点的框。 如果在此处未看到应用程序 .exe，请手动浏览并运行它。

单击“下一步”。此时会出现一个弹出窗口，提示你确认已完成应用程序的安装和初始启动任务的管理。
- 如果已完成，请单击“是，继续”。
- 如果尚未完成，请单击“否，未完成”。 随后你会返回到上一页，可在其中启动应用程序、安装或复制其他文件和 DLL/可执行文件。

## <a name="create-package"></a>创建程序包

- 提供用于保存 MSIX 包的位置。
- 默认情况下，包保存在本地应用数据文件夹中。
- 可以在“设置”菜单中定义默认保存位置。
- 在保存 MSIX 包之前若要继续编辑包的内容和属性，可以选择“包编辑器”转到“包编辑器”。
- 单击“创建”以创建 MSIX 包。

创建包后，会出现一个弹出窗口。 此弹出窗口中包含新建包的名称、发布者和该保存位置。 可以关闭此弹出窗口并重定向到欢迎页。 还可以选择“包编辑器”[](https://docs.microsoft.com/windows/msix/packaging-tool/package-editor)来查看和修改包的内容与属性。

