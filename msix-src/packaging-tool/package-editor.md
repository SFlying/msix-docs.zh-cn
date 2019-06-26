---
title: 包编辑器 | Microsoft Docs
description: 使用包编辑器修改包信息
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 79aa7202570c91d3e4d631ff8f3b1413e5f9a79e
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "59566511"
---
# <a name="package-editor"></a>包编辑器

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>
      
若要对现有的 MSIX 包进行更改，例如，在无需再次打包安装程序的情况下修改清单中的属性或包的内容，可以使用 MSIX 打包工具中的包编辑器。 

在欢迎屏幕中选择“包编辑器”图标，浏览到你的 MSIX 包并选择“打开包”。

在“包信息”页上，可以通过 UI 中的字段更改包信息，或者在所选的编辑器中手动打开 MSIX 清单文件，对清单字段进行更改。 编辑清单时，包编辑器页不可编辑。 保存清单后，UI 将会更新。

可以导航到包编辑器的其他部分，以修改功能、虚拟注册表或包文件。 编辑完包后，请务必为包签名并更新版本，然后保存更改。 

![pic10](images/pic10.png)

在“功能”页上，可以添加或删除包的[功能](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-capability)。 如果包中存在某项功能，则相应的复选框将会选中。 选择或取消选择某项功能会更新清单。 这相当于 MSIX 清单中的 <capability> 元素。

![pic11](images/pic11.png)

“虚拟注册表”页显示应用程序的所有已打包虚拟注册表项。 

在左侧窗口中右键单击某个节点：
- 展开/折叠：展开或折叠配置单元中的所有注册表项
- 项：可让用户重命名、删除或新建注册表项
- 值：可让用户添加字符串、二进制或 DWORD 类型的注册表项值

右键单击右侧窗口中的任意位置：
- 删除：删除注册表项
- 添加字符串：将字符串值添加到注册表项
- 添加二进制值：将二进制值添加到注册表项
- 添加 DWORD 值：将 DWORD 值添加到注册表项

![pic12](images/pic12.png)

在“包文件”页上，可以双击展开文件系统。 

右键单击某个文件夹：
- 添加文件：将文件添加到所选的文件夹
- 新建文件夹：创建新的空文件夹
- 添加文件夹：浏览到现有的文件夹以添加该文件夹
- 删除：删除所选的文件夹
- 移动：重命名文件夹或将其移到新位置

右键单击某个文件：
- 删除：删除所选的文件
- 移动：重命名文件或将其移到新位置

![pic13](images/pic13.png)

