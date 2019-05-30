---
title: MSIX 打包工具的最佳做法
description: MSIX 打包工具的最佳做法
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 59f38d2f23470824687a739eb87a808baac29562
ms.sourcegitcommit: fe59d6b39d81eb4a155887fa8fe9a08b6fe48584
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65488526"
---
# <a name="best-practices-for-msix-packaging-tool"></a>MSIX 打包工具的最佳做法

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

本文介绍如何重新打包你的应用 MSIX 和使用 MSIX 打包工具的最佳实践。

## <a name="best-practices-during-setup"></a>在安装过程的最佳实践
 
开始时，必须具有 MSIX 打包工具的版本的 Windows 10 2018 年 10 月更新 （也称为版本 1809年）。 对于转换过程中，有几个其他操作，我们建议您考虑在开始之前。 

- 我们了解，并非每个人都是在 Windows 10 上年 10 月 2018 Update 或甚至 Windows 10。 因此，我们建议您创建全新的 VM，它预先配置为对 MSIX 支持的最小版本。 

- 这是建议的另一个原因是，在交互式 GUI 转换期间使用 MSIX 打包工具，我们将会侦听在设备上的所有内容和它将有助于防止在包中的无关数据。 

- 其也很有必要知道应使用您的应用程序运行，以便可以了解哪些有依赖项的类型和它们应以修改包的形式打包。 例如，如果您有运行时依赖项，它是在主应用程序中包括一个好办法。 如果插件，您应打包，以关联的修改包的形式。 


## <a name="best-practices-during-repackaging"></a>在重新打包过程的最佳实践 
当使用 MSIX 打包工具时，有几件事，我们还建议您作为最佳实践：
- 打包 ClickOnce 安装程序时，必须将发送到桌面快捷方式，如果安装程序不已执行操作。 一般情况下，它是很好的做法始终记得发送到主应用程序的桌面快捷方式可执行文件。
- 当修改包时，需要声明包的父级的名称 （标识名称） 应用程序中工具 UI，以便该工具修改包的清单中设置正确的包依赖项。
- 声明中的安装位置字段**包信息**页是可选的但建议。 请确保此路径与应用程序安装程序的安装位置相匹配。
- 中的步骤执行准备工作**准备计算机**页是可选但强烈推荐。
ms.custom:RS5


## <a name="best-practices-while-bundling-msix-packages"></a>捆绑 MSIX 包时的最佳实践

我们建议当不同的安装程序可用于不同的处理器体系结构如 x86、 x64 和 ARM 为应用程序捆绑 MSIX 包。 通过一起捆绑中的安装程序，您可以允许 Windows 10 OS 以确定而不是用户无需做出正确选择适当的安装程序的适用性。 

同时捆绑 MSIX 包，需要已转换成 MSIX 包 Win32 安装程序。 

- MSIX 打包工具将假定转换正在进行的 Windows 10 OS 版本的处理器体系结构。 例如，如果您在 x64 版本的 Windows 10 中，并且您要转换的安装程序，生成的 MSIX 包版本将 x86 上为 x64。 
- 如果您计划对捆绑 MSIX 包，需要将您想要将它们部署在同一环境中的安装程序。 这样一来，MSIX 打包工具将生成该环境的相应 MSIX 程序包。 



