---
title: 在 VM 上创建 MSIX 包从桌面安装程序 （MSI、 EXE 或 APP-V）
description: 在 VM 上创建 MSIX 包从桌面安装程序 （MSI、 EXE 或 APP-V）
author: mcleanbyron
ms.author: mcleans
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 457f9a0358cb8e72abd9539d1a4fe3131ffd2a54
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400807"
---
# <a name="create-an-msix-package-from-a-desktop-installer-msi-exe-or-app-v-on-a-vm"></a>在 VM 上创建 MSIX 包从桌面安装程序 （MSI、 EXE 或 APP-V）

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

可以使用[MSIX 打包工具](../mpt-overview.md)从 HYPER-V 虚拟机 (VM) 上将现有的 MSI、 EXE 或 APP-V 安装创建 MSIX 应用程序包。 VM 必须满足以下要求：

- 它必须配置为[接收远程命令](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/remotely-manage-hyper-v-hosts)(运行[Enable-psremoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting?view=powershell-5.1)命令在 VM 上)
- 它必须运行 Windows 10，版本 1809 或更高版本的 Windows。

在首次启动该工具，系统将提示你同意将遥测数据发送到。 请务必注意你共享的诊断数据只有来自应用程序和永远不会用于识别或与你联系。 这只是有助于我们为你解决问题更快。

创建应用程序包是最常用的选项。 这是从中创建 MSIX 包从安装程序，或通过手动安装的应用程序负载。

![pic1](images/pic1.png)

## <a name="choose-the-installer-you-want-to-package"></a>选择你想要打包的安装程序

![pic2](images/pic2.png)

通过单击导航到安装程序 MSI 或 APP-V**浏览**和文件选取器中选择安装程序。 然后，单击“下一步”  。

可选：
- 下的复选框**使用现有 MSIX 包**、 浏览和选择你想要更新现有 MSIX 包。
- 下的复选框**使用安装程序参数**和所提供的字段中输入所需的参数。 此字段接受任何字符串。
- 下的复选框**用于测试签名包**，浏览到并选择.pfx 证书文件。 如果证书受密码保护，请在密码框中键入的密码。

## <a name="packaging-method"></a>打包方法

![images/pic3](images/pic3.png)

- 选择打包环境的虚拟机。
  - 选择**现有的虚拟机上创建包**并从下拉列表选择现有的虚拟机名称。 您将会看到用户和密码字段，如果存在任何 vm 提供凭据。
  - 单击“下一步”  。

## <a name="package-information"></a>包信息

![images/pic4](images/pic4.png)

选择要打包现有的虚拟机上的应用程序后，必须提供有关向应用程序的信息。 该工具将尝试自动填充这些字段基于可从安装程序的信息。 将始终可以选择根据需要更新条目。 如果为星号 * 字段，它是必需的但您已经知道了。 如果该条目不是有效，则提供的内联帮助。
- 包名称：
    - 所需和对应于包清单来描述包的内容中的标识名称。
    - 必须匹配用于对包进行签名的证书的名称使用者信息。
    - 不会显示给最终用户。
    - 区分大小写，不能有空格。
    - 可以接受字符串中包含字母数字、 句点和短划线字符的长度的 3 到 50 个字符之间。
    - 不能以句点结尾，并且是其中之一："CON"、"PRN"、"AUX"、"NUL"、"COM1"、"COM2"、"COM3"、"COM4"、"COM5"、"COM6"、"COM7"、"COM8"、"COM9"、"LPT1"、"LPT2"、"LPT3"、"LPT4"、"LPT5"、"LPT6"、"LPT7"、"LPT8"和"LPT9"。
- 包显示名称：
    - 所需，对应于要向用户，在开始菜单和设置页中显示友好的包名称的清单中的包。
    - 字段接受介于 1 和 256 之间的字符串长度个字符，并且是可本地化。
- 发布者名称：
    - 所需，对应于包的描述的发布服务器信息。
    - 发布服务器属性必须与用于对包进行签名的证书的发布服务器使用者信息匹配。
    - 此字段接受符合正则表达式的可分辨名称的长度为 1 到 8192 个字符的字符串："(CN |L |O |OU |E |C |S |街道 |T |G |我 |SN |DC |SERIALNUMBER |说明 |邮政编码 |POBox |手机 |X21Address |dnQualifier |(OID。(0 |[1-9][0-9])(.(0 |[1-9][0-9]))+）） = (([^，+ ="<> #;])+ |".")(，((CN |L |O |OU |E |C |S |街道 |T |G |我 |SN |DC |SERIALNUMBER |说明 |邮政编码 |POBox |手机 |X21Address |dnQualifier |(OID。(0 |[1-9][0-9])(.(0 |[1-9][0-9]))+）） = (([^，+ ="<> #;])+ |".")))*".
- 发布者显示名称：
    - 所需，对应于要向用户应用程序安装工具和设置页中显示友好的发布服务器的名称在清单中的包。
    - 字段接受介于 1 和 256 之间的字符串长度个字符，并且是可本地化。
- 版本：
    - 所需，对应于清单，描述包的版本号中的包。
    - 此字段接受四部分表示法的版本字符串："Major.Minor.Build.Revision"。
- 安装位置：
    - 这是安装程序会将应用程序负载复制到 （通常是程序文件文件夹） 的位置。
    - 此字段是可选的但建议专门时应用负载正在安装程序文件文件夹外。
    - 浏览到并选择文件夹路径。
    - 请确保此文件匹配安装程序的安装位置，而您要通过应用程序安装操作。

## <a name="prepare-computer"></a>准备计算机

![images/pic5](images/pic5.png)

下一步，**准备计算机**页面提供用于准备计算机以打包选项。

MSIX 打包工具驱动程序是必需，该工具将自动尝试启用它，如果未启用。 使用 DISM 来查看是否已安装该驱动程序将首先检查该工具。

> [!NOTE]
> MSIX 打包工具驱动程序监视系统，以捕获一个安装程序进行的系统的允许 MSIX 打包工具来创建基于这些更改的包的更改。

- [可选]选中的复选框**Windows Search 处于活动状态**，然后选择**禁用所选**如果您选择禁用搜索服务。
    - 这不是必需的仅建议。
    - 禁用后，该工具将更新为"禁用"状态字段

- [可选]选中的复选框**Windows Update 处于活动状态**，然后选择**禁用所选**如果您选择禁用更新服务。
    - 这不是必需的仅建议。
    - 禁用后，该工具将更新的状态字段**禁用**。

- **正在等待重新启动**复选框默认处于禁用状态。 你将需要手动重新启动计算机，然后启动该工具再次询问您挂起的操作需要重新启动。 这不是必需，仅建议。

完成后准备计算机，单击**下一步**。

## <a name="installation"></a>安装

![images/pic6](images/pic6.png)

- 这是安装阶段，并在其中监视工具和捕获应用程序安装操作。
- 该工具将启动安装程序在更早的阶段中打开它，您将需要完成的安装程序向导来安装应用程序的虚拟机窗口中。
    - 请确保安装路径符合前面在包信息页中定义的内容。
    - 你将需要在新安装的应用程序的桌面中创建一个快捷方式。
    - 完成应用程序安装向导后，请确保在完成或关闭安装向导。
    - 如果你需要运行多个安装程序您可以手动执行操作，在这里。
    - 如果应用需要的其他先决条件，你需要立即安装它们。
    - 如果应用程序需要.Net 3.5/20，请将可选的功能添加到 Windows。
- 完成安装应用程序后，单击**下一步**。

## <a name="manage-first-launch-tasks"></a>管理第一个启动任务

![images/pic7](images/pic7.png)

此页显示了该工具捕获的应用程序可执行文件。 我们建议启动应用程序至少一次捕获任何第一个启动任务。

如果有多个应用程序，选中对应于主入口点的框。 如果看不到应用程序.exe 此处，手动浏览到并运行它。

单击**下一步**系统会提示使用弹出要求确认您结束使用应用程序安装和管理第一个启动任务。
- 如果您完成时，请单击**可以，请继续**。
- 如果你正在这样做，请单击**否，我不已完成**。 您将被带回至最后一页可以在其中启动应用程序、 安装或其他文件和 dll/可执行文件复制到。

## <a name="create-package"></a>创建程序包

![images/pic8](images/pic8.png)

- 提供用于保存 MSIX 包的位置。
- 默认情况下，包保存在本地应用程序数据文件夹中。
- 在设置菜单中，可以定义默认的保存位置。
- 如果你想要继续保存 MSIX 包之前编辑的内容和包的属性，您可以选择"包编辑器"，转到包编辑器。
- 如果想要使用用于测试的预设的证书包进行签名，浏览到并选择的证书。
- 单击**创建**创建 MSIX 包。

将显示与 pop 会创建包时。 此弹出最多将包括名称、 发布服务器上，并保存新创建的包的位置。 可以关闭此弹出窗口和重定向到欢迎页。 此外可以选择包编辑器来查看和修改包的内容和属性。
