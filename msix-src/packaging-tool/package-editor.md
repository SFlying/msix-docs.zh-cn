---
title: 包编辑器 |Microsoft Docs
description: 使用包编辑器修改包的信息
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 79aa7202570c91d3e4d631ff8f3b1413e5f9a79e
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566511"
---
# <a name="package-editor"></a>包编辑器

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>
      
若要对现有 MSIX 包如而无需包安装程序修改的程序包的内容或清单中的属性进行更改同样，您可以使用包编辑器中 MSIX 打包工具。 

在欢迎屏幕上，选择包编辑器图标，为 MSIX 包浏览并选择打开包。

在包的信息页上，可以更改您的包信息通过在 UI 中的字段或选择要更改的清单字段所选的编辑器中手动打开 MSIX 清单文件。 编辑清单时包编辑器页面不能编辑。 保存清单时，将会更新 UI。

您可以导航到要修改容量、 虚拟注册表中或包文件的包编辑器的其他部分。 在完成编辑您的包，请务必对包进行签名并保存所做的更改之前更新版本。 

![pic10](images/pic10.png)

在功能页可以添加或删除[功能](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-capability)包。 如果包中存在一项功能，则相应的复选框将进行检查。 如果您选择或取消选中的功能，它将更新应用清单。 这会转换为<capability>MSIX 清单中的元素。

![pic11](images/pic11.png)

虚拟注册表页显示所有应用程序的打包虚拟注册表项。 

右键单击左窗口中的节点：
- 展开/折叠： 展开或折叠所有注册表密钥存储在配置单元
- 密钥： 允许用户重命名、 删除或创建新的密钥
- 值： 允许用户将项值添加为字符串、 二进制或 DWORD

右键单击任意位置在右侧窗口中：
- Delete： 删除项
- 添加字符串： 要将一个字符串值添加到密钥
- 添加二进制文件： 若要将二进制值添加到密钥
- 添加 DWORD： 若要将 DWORD 值添加到密钥

![pic12](images/pic12.png)

在包文件页中，你可以双击以展开的文件系统。 

右键单击一个文件夹：
- 添加文件：将文件添加到选择的文件夹
- 新文件夹中：创建一个新的空文件夹
- 添加文件夹：浏览以添加现有文件夹
- 删除：删除所选的文件夹
- Move:重命名或将文件夹移到新位置

右键单击一个文件：
- 删除：删除所选的文件
- Move:重命名或将文件移动到新位置

![pic13](images/pic13.png)

