---
title: MSIX Core
description: 本文概述了 .MSIX Core，其中提供了对 Windows 7 SP1、Windows 8.1、当前支持的 Windows Server （带有桌面体验）和 Windows 10 版本1709（秋季周年更新）的 .MSIX 支持。
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: fdab2cf07a3a9e7ecbcae82120422647eaf145d5
ms.sourcegitcommit: 90eed7d23240aefa3761085955a193323f4661d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75831479"
---
# <a name="msix-core"></a>MSIX Core

.MSIX Core 将 .MSIX 支持引入 Windows 10 版本1709之前的 Windows 版本。 .MSIX Core 是 GitHub 上的[开源项目](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore)，可用于安装 .msix 包。 可以通过下载[最新版本](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release)或[预览版](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-preview)来开始。

使用 .MSIX Core，需要支持这些早期 Windows 版本用户的开发人员和 IT 专业人员现在可以采用并利用 .MSIX 的优势。

## <a name="what-is-msix-core"></a>什么是 .MSIX 核心？

.MSIX Core 允许在以前版本的 Windows 上安装 .MSIX 应用，前提是这些应用已构建为在这些版本的 Windows 上运行。 .MSIX Core 是针对以下 Windows 版本生成的：目前不支持 .MSIX：

* Windows 7 SP1
* Windows 8.1
* 当前支持的 Windows Server （带有桌面体验）
* 1709之前的 Windows 10 版本

.MSIX Core 面向开发人员和 IT 专业人员。 开发人员可以使用 .MSIX 核心库，使其现有的安装程序能够在以前的 Windows 版本上安装其 .MSIX 封装应用，因此，它们只能生成一个 .MSIX 包以面向所有 Windows 用户。 IT 专业人员可以下载 .MSIX Core 安装程序。  .MSIX 核心安装程序支持 .MSIX 的命令行安装以及用户只需双击它们即可安装 .MSIX 包。

## <a name="considerations-of-msix-core"></a>.MSIX 核心的注意事项

.MSIX 核心的目标是启用 .MSIX 封装应用的安装、查询和删除（这些应用已在这些 Windows 版本中运行），并尽可能地提供卸载。 .MSIX Core 提供了本机 .MSIX 的一小部分功能，其功能类似于现有的 Win32 安装程序类型。

* .MSIX Core 不提供本机 .MSIX 的容器优点，也不允许使用 Windows 10 特定功能的应用在以前的 Windows 版本上运行。
* 在下层操作系统上使用 .MSIX Core 时，[应用执行别名](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-an-alias)仅适用于**Win + R** ，而不是来自命令提示符或 PowerShell。
* .MSIX 核心不支持 Microsoft Store 集成。 想要将应用程序发布到应用商店的开发人员可以按照[此处](https://docs.microsoft.com/windows/uwp/publish/)的文档进行操作。

## <a name="get-started"></a>“开始”

若要将 .MSIX 包与 .MSIX Core 一起部署，必须先[更新现有的 .msix 清单](support-msix-core.md)。 然后，你可以[使用 .Msix core](deploy-with-msix-core.md) （如果只有此包）部署你的 .msix 包，或者可以通过源代码（如果你有源代码）[创建包含 .MSIX Core 的 .msix 包](msixcore-clickonce-solution.md)。
