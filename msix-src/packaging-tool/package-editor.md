---
title: 使用包编辑器编辑包
description: 本文介绍如何使用 .MSIX 包工具中的包编辑器来编辑包信息，如清单中的属性。
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a64a25074f8ff20c9eff461cdbad541548c6c284
ms.sourcegitcommit: e650c86433c731d62557b31248c7e36fd90b381d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/02/2020
ms.locfileid: "82726505"
---
# <a name="edit-a-package-using-package-editor"></a>使用包编辑器编辑包

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

若要对现有的 .MSIX 包进行更改（例如编辑清单中的属性或包的内容，而无需重新打包安装程序），可以在 .MSIX 打包工具中使用**包编辑器**。

在 .MSIX 打包工具的 "欢迎" 页中，选择 "**包编辑器**" 图标，浏览到 .msix 包，然后选择 "**打开包**"。 你还可以右键单击 .MSIX 包，并选择 "**使用 .Msix 打包工具编辑**" （在版本1.2019.1018.0 和更高版本中提供）。

可以通过底部的 "解压缩" 按钮，从包编辑器中解压缩 .MSIX 包。 然后，可以选择要在其中解压缩 .MSIX 包的位置。 

## <a name="package-information-page"></a>“包信息”页

在 "**包信息**" 页上，您可以通过 UI 中的字段更改您的包信息，也可以选择在所选编辑器中手动打开 .msix 清单文件以更改清单字段。 编辑清单时，包编辑器页不可编辑。 保存清单后，UI 将会更新。

你可以导航到包编辑器的其他部分，以编辑你的功能、虚拟注册表或包文件。 编辑完包后，请务必为包签名并更新版本，然后保存更改。

![packageeditorpkginfo1](images/PackageEditorPkgInfo1.png)

![packageeditorpkginfo2](images/PackageEditorPkgInfo2.png)

## <a name="capabilities-page"></a>“功能”页

在 "**功能**" 页上，可以添加或删除包的[功能](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-capability)。 如果包中存在某项功能，则相应的复选框将会选中。 如果选择或取消选择某项功能，则会更新清单。 这相当于 MSIX 清单中的 <capability> 元素。

![pic11](images/pic11.png)

## <a name="virtual-registry-page"></a>“虚拟注册表”页

"**虚拟注册表**" 页将显示该应用程序的所有打包虚拟注册表项。

在左侧窗口中右键单击某个节点可执行以下操作：

- 展开/折叠：展开或折叠配置单元中的所有注册表项。
- 项：可让用户重命名、删除或新建注册表项。
- 值：可让用户添加字符串、二进制或 DWORD 类型的注册表项值。

右键单击右侧窗口内的任意位置可执行以下操作：

- 删除：删除注册表项。
- 添加字符串：将字符串值添加到注册表项。
- 添加二进制值：将二进制值添加到注册表项。
- 添加 DWORD 值：将 DWORD 值添加到注册表项。

![pic12](images/pic12.png)

## <a name="package-files-page"></a>“包文件”页

在“包文件”**** 页上，可以双击以展开包内容的文件系统。 例如，可以使用此页[编辑应用图标和资产](edit-icons-and-assets.md)。

右键单击某个文件夹可执行以下操作：

- 添加文件：将文件添加到所选的文件夹。
- 新文件夹：创建新的空文件夹。
- 添加文件夹：浏览以添加现有文件夹。
- 删除：删除所选的文件夹。
- 移动：将文件夹重命名或移动到新位置。

右键单击某个文件可执行以下操作：

- 删除：删除所选文件。
- 移动：将文件重命名或移动到新位置。

![pic13](images/pic13.png)
