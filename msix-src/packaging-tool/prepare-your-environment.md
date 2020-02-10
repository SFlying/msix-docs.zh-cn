---
title: 为转换准备环境
description: 概述如何从现有的安装程序转换 .MSIX。
ms.date: 01/27/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 1b4a171039e4f442a17150154119aa4027669887
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073613"
---
# <a name="prepare-your-environment-for-conversion"></a>为转换准备环境

准备转换环境是转换过程中的一个重要步骤。 以下建议可帮助确保在将现有安装程序转换为 .MSIX 时获得成功。

- .MSIX 打包工具的最低操作系统版本要求为 Windows 10 1809。 我们了解，并非每个人都用的是 Windows 10 2018 年 10 月更新或甚至是 Windows 10。 因此，我们建议创建一个预配置的干净 VM，该 VM 为 .MSIX 打包工具支持的最低版本。 部署生成的 .MSIX 包具有不同的支持要求。

- 用于转换的干净计算机很重要，因为在 .MSIX 打包工具的安装步骤中，我们将侦听环境中的所有内容，以捕获安装程序正在执行的操作。 干净的计算机意味着在你的计算机上运行的应用程序或服务不会被捕获到你的包中。

- 建议将转换计算机配置为模拟将运行 .MSIX 包的环境，因此，如果存在要在其中运行的服务或策略，则可以测试该包是否会实际工作。

- 转换环境应与要部署应用程序的体系结构相匹配。 例如，如果要将 .MSIX 包部署到 x64 计算机上，则应在 x64 计算机上执行转换。 

- 如果这不是我们提供的 "**快速创建 VM**"，则可以在 hyper-v 中使用[.Msix 打包工具环境](quick-create-vm.md)，该环境可与 Windows 10 1809 和最新版本的 .msix 打包工具结合使用。 

- 遵循有关[设置 .Msix 打包工具](tool-best-practices.md)（或所选工具）的最佳实践建议，然后为 VM 创建检查点。 这样，你就可以使用 VM 来转换、还原到以前的检查点，并且它将是一个干净的配置计算机，可以再次进行转换或验证是否已成功转换 .MSIX 包。

- 此外，也最好知道你有哪一类型的依赖项，以便了解哪些依赖项应随应用运行，以及哪些依赖项应以修改包的形式打包。 例如，如果有运行时依赖项，那么最好将这些依赖项包含在主应用程序中。 如果你有一个插件，则应将其打包为一个修改包，以便与你的主应用程序相关联。

- 如果要在远程计算机上执行转换，则需要执行其他一些[设置](remote-conversion-setup.md)以使其能够转换。
