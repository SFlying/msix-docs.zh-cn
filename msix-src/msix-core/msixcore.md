---
title: MSIX Core
description: 本文概述了 .MSIX Core，其中提供了对 Windows 7 SP1、Windows 8.1、当前支持的 Windows Server （带有桌面体验）和 Windows 10 版本1709（秋季周年更新）的 .MSIX 支持。
ms.date: 04/12/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d0273b73156454c9728fc4c23214d33cd08d56e9
ms.sourcegitcommit: 8aafaa9ac4087ef2e95030343add8fe2ee1cccc9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/18/2019
ms.locfileid: "75186733"
---
# <a name="msix-core"></a>MSIX Core

.MSIX Core 为 Windows 7 SP1、Windows 8.1、当前支持的 Windows 服务器（具有桌面体验）以及1709之前的 Windows 10 版本（秋季更新）提供了 .MSIX 支持。 .MSIX Core 是 GitHub 上的[开源项目](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore)，可用于安装 .msix 包。 可以通过下载[最新版本](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release)或[预览版](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-preview)来开始。

使用 .MSIX Core，需要支持这些早期 Windows 版本用户的开发人员和 IT 专业人员现在可以采用并利用 .MSIX 的优势。

##  <a name="what-is-msix-core"></a>什么是 .MSIX 核心？

.MSIX Core 允许在以前版本的 Windows 上安装 .MSIX 应用，前提是这些应用已构建为在这些版本的 Windows 上运行。 更具体地说，.MSIX Core 是针对当前不支持 .MSIX 的 Windows 版本而构建的： Windows 7 SP1、Windows 8.1、当前支持的 Windows Server （带有桌面体验）以及1709之前的 Windows 10 版本（秋季更新）。

.MSIX Core 面向开发人员和 IT 专业人员。 开发人员可以使用 .MSIX 核心库，使其现有的安装程序能够在以前的 Windows 版本上安装其 .MSIX 封装应用，因此，它们只能生成一个 .MSIX 包以面向所有 Windows 用户。 IT 专业人员可以下载 .MSIX Core 安装程序。  .MSIX 核心安装程序支持 .MSIX 的命令行安装以及用户只需双击它们即可安装 .MSIX 包。

## <a name="considerations-of-msix-core"></a>.MSIX 核心的注意事项

.MSIX 核心的目标是启用 .MSIX 封装应用的安装、查询和删除（这些应用已在这些 Windows 版本中运行），并尽可能地提供卸载。 .MSIX Core 提供了本机 .MSIX 的一小部分功能，其功能类似于现有的 Win32 安装程序类型。 .MSIX Core 不提供本机 .MSIX 的容器优点，也不允许使用 Windows 10 特定功能的应用在以前的 Windows 版本上运行。 

.MSIX Core 还不支持 Microsoft Store 集成。 想要将应用程序发布到 Microsoft Store 的开发人员可以遵循[此处](https://docs.microsoft.com/windows/uwp/publish/)的文档。

### <a name="get-started"></a>“入门”， 
若要将 .MSIX 包部署到 Windows 7 SP1、Windows 8.1、当前支持的 Windows 服务器（具有桌面体验）以及1709（秋季更新）低于 .MSIX Core 的 Windows 10 版本，则需要[更新现有的 .msix 清单](https://docs.microsoft.com/windows/msix/msix-core/support-msix-core)。 

若要利用 .MSIX on Windows 7 SP1、Windows 8.1、当前支持的 Windows 服务器（具有桌面体验）和 Windows 10 版本1709（秋季周年更新）之前的优势，请[使用 .Msix Core 部署 .msix 包](https://docs.microsoft.com/windows/msix/msix-core/deploy-with-msix-core)，或者在你有源代码的情况下，[使用来自你的源代码的 .MSIX Core 创建 .msix 包](https://docs.microsoft.com/windows/msix/msix-core/msixcore-clickonce-solution)。