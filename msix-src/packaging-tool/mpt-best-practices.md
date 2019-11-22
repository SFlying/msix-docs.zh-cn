---
title: 适用于 MSIX 打包工具的最佳做法
description: 本文介绍适用于将应用重新打包到 MSIX 以及使用 MSIX 打包工具的最佳做法。
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 975ed03b8c3a0e436dff9dac8469e24556557b3c
ms.sourcegitcommit: 073a228653f004914851c3461b9ad6eef343f915
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2019
ms.locfileid: "74309023"
---
# <a name="best-practices-for-the-msix-packaging-tool"></a>适用于 MSIX 打包工具的最佳做法

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

本文介绍适用于将应用重新打包到 MSIX 以及使用 MSIX 打包工具的最佳做法。

## <a name="best-practices-during-setup"></a>安装过程中的最佳做法
 
首先，请确保具有[最新版本的 .Msix 打包工具](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/mpt-overview#latest-public-version---1201910180)。 对于转换过程，我们建议你在开始之前考虑其他一些事项。 

- .MSIX 打包工具的最低操作系统版本要求为 Windows 10 1809。 我们了解，并非每个人都用的是 Windows 10 2018 年 10 月更新或甚至是 Windows 10。 因此，我们建议你创建已预先配置为提供 MSIX 最低版支持的全新 VM。 如果不是这样，我们还提供了一个快速创建 VM，即 Hyper-v 中的[.Msix 打包工具环境](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/quick-create-vm)，该环境已准备好进行转换。 

- 这样建议的另一个原因是，在使用 MSIX 打包工具进行交互式 GUI 转换期间，我们将会侦听设备上的所有内容，它将有助于防止包中出现无关数据。 

- 此外，也最好知道你有哪一类型的依赖项，以便了解哪些依赖项应随应用运行，以及哪些依赖项应以修改包的形式打包。 例如，如果有运行时依赖项，那么最好将这些依赖项包含在主应用程序中。 如果你有一个插件，则应将其打包为一个修改包，以便与你的主应用程序相关联。 


## <a name="best-practices-during-repackaging"></a>重新打包过程中的最佳做法 
当使用 MSIX 打包工具时，还有一些事项是我们建议作为最佳做法来执行的：
- 打包 ClickOnce 安装程序时，有必要将快捷方式发送到桌面（如果安装程序尚未执行该操作）。 一般情况下，始终记得为主应用可执行文件将快捷方式发送到桌面是很好的做法。
- 当创建修改包时，需要在工具 UI 中声明父应用程序的包名称（标识名称），以便该工具在修改包的清单中设置正确的包依赖项。
- 在 "**准备计算机**" 页中执行准备步骤是可选的，但强烈建议这样做，因为这将有助于减少包中的任何无关数据。 
- 需要对包进行签名以进行安装，但我们也建议您将证书加盖时间戳，以便即使证书过期也能安装应用程序。 
- "**包信息**" 页中的 "声明安装位置" 字段是可选的。 请确保此路径与应用程序安装程序的安装位置相匹配。
ms-chap： RS5


## <a name="best-practices-while-bundling-msix-packages"></a>捆绑 MSIX 包时的最佳做法

我们建议在应用程序有不同的安装程序可用于不同的处理器体系结构（如 x86、x64 和 ARM）时捆绑 MSIX 包。 通过将安装程序捆绑在一起，可以允许 Windows 10 OS 确定相应安装程序的适用性，而无需用户必须做出正确选择。 

当捆绑 MSIX 包时，需要已将 Win32 安装程序转换成 MSIX 包。 

- MSIX 打包工具将假定转换正在进行的 Windows 10 OS 版本的处理器体系结构。 例如，如果位于 x64 版的 Windows 10 上，并且要转换 x86 版的安装程序，那么生成的 MSIX 包将为 x64 版。 
- 如果计划捆绑 MSIX 包，则需要在应部署它们的同一环境中转换安装程序。 这样一来，MSIX 打包工具将生成适用于该环境的 MSIX 包。 



