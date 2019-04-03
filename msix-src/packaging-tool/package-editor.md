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
ms.openlocfilehash: ba85a6f8c55774af070be53819dc60417ca86236
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900369"
---
# <a name="package-editor"></a>包编辑器

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>
      
若要向现有 MSIX 包而无需包安装程序中更改包的内容或清单中的属性进行更改，其中一个可以中再次使用包编辑器 MSIX 打包工具。 

在欢迎屏幕上，选择包编辑器选项：

![pic1](images/pic1.PNG)

然后它们命中浏览并找到需要对其进行编辑，并选择打开包 MSIX 包：

![pic10](images/pic10.png)

包编辑器现在显示的内容和包的属性。

![pic11](images/pic11.png)

第一页显示包的信息，用户可以更改在 UI 中的包信息或选择以选择要更改的清单字段的其编辑器中手动打开 MSIX 清单文件。 虽然他们正在编辑的清单包编辑器页面不能编辑。 一次保存清单，UI 将会更新。

在完成编辑包或命中时，用户可以选择保存按钮取消或 x 以放弃所做的更改。 否则它们可以通过导航到其他页面的页面上的左侧单击。

![pic12](images/pic12.png)

在此页中，他们可以添加或删除包的功能。 如果包中存在一项功能，则相应的复选框将进行检查。
- 这会转换为<capability>MSIX 清单中的元素。

![pic13](images/pic13.png)

此页显示所有应用程序的打包虚拟注册表项。 有多个上下文菜单选项在此页上：

1. 右键单击左窗口中的节点：
    - 展开/折叠： 展开或折叠所有注册表密钥存储在配置单元
    - 密钥： 允许用户重命名、 删除或创建新的密钥
    - 值： 允许用户将项值添加为字符串、 二进制或 DWORD


2. 右键单击任意位置在右侧窗口中：
 
    - Delete： 删除项
    - 添加字符串： 要将一个字符串值添加到密钥
    - 添加二进制文件： 若要将二进制值添加到密钥
    - 添加 DWORD： 若要将 DWORD 值添加到密钥

此页显示包文件中，用户可以添加或删除文件，通过右键单击文件并选择将文件添加或删除。


