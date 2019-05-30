---
title: Windows 10 版本 1709年及更高版本上 MSIX 程序包支持
description: 安装 MSIX 包 1709年及更高版本。
author: c-don
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX、 MPT、 MSIX 打包工具，1709年版本 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf737f108cc3c248fe5008a27fde15b79abfbaf9
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795459"
---
# <a name="msix-package-support-on-windows-10-1709-and-later"></a>MSIX 包支持在 Windows 10 1709年及更高版本

如果现有应用程序转换为 MSIX，可能想要在早期版本的 Windows 比 1809 （生成 17701） 使用应用。 这篇博客文章讨论如何能够最早版本 16299，也称为 Windows 10 版本 1709年中生成此类应用程序。 
 
 
## <a name="problem"></a>问题：
因此到 MSIX 使用 MSIX 打包工具转换现有的应用和 1809年及更高版本，它运行良好。 但既然您知道我们将添加对 MSIX 启动内部版本 16299 支持，你想要对此运行 MSIX 应用或任何更高版本。 目前，如果只是尝试生成版本 16299 到 17700 一台计算机上安装 MSIX 应用，你将收到此错误消息： 

![PowerShell MSIX 安装](images/mpt_blog_0.jpg)

## <a name="solution"></a>解决方案：
我们正在努力更新 MSIX 打包工具来处理此操作，但在此期间下面是你可以如何将您的应用程序设置为在这些版本上运行：
 
打开 MSIX 打包工具，并导航到包编辑器。

![open](images/mpt_blog_1.jpg) 
![navigate](images/mpt_blog_2.jpg)


导航到 MSIX 包。 在本例中，它是 Test_App.msix 包。 单击"打开包"

![打开包](images/mpt_blog_3.jpg)

在"包信息"选项卡的底部，观察打开清单文件的选项。 

![打开清单](images/mpt_blog_4.jpg)

选择"打开文件"并编辑 MinVersion 等于版本 16299，如下所示：

![观察清单](images/mpt_blog_5.jpg)
![编辑清单](images/mpt_blog_6.jpg)
![编辑清单 2](images/mpt_blog_7.jpg)

完成后编辑清单，关闭文件。 这将使你返回到包编辑器中。
不要忘记重新编辑对包进行签名，如下所示：

![登录](images/mpt_blog_9.jpg)

后已进行更新，保存所做的更改。

![save](images/mpt_blog_10.jpg)

此时，您可以 16299 生成或更高版本的设备上安装应用。
 


 
## <a name="troubleshooting-tips"></a>故障排除技巧：
现在，在设备中运行的生成版本 16299 到 17700，你可以安装 MSIX 应用通过 PowerShell。 具体而言，需要在 PowerShell 中的此命令： 添加 appxpackage <path to MSIX package>

![PowerPoint 命令](images/mpt_blog_11.jpg)

此外可以使用 Intune、 SCCM 或打包管理器 API。



