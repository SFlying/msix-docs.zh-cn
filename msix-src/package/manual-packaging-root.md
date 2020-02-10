---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: 从命令行打包
description: 本部分包含或链接，指向有关手动打包 Windows 应用的文章。
ms.date: 01/30/2020
ms.topic: article
keywords: windows 10, uwp, 打包
ms.localizationpriority: medium
ms.openlocfilehash: 9bbc940360ed7c3c0a428ba4aa3f981274c0a5d7
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072679"
---
# <a name="package-from-the-command-line"></a>从命令行打包

如果不在 Visual Studio 中开发应用程序，可以使用 .MSIX 命令行工具对应用程序进行打包和签名。


## <a name="purpose"></a>用途

本部分链接到有关使用命令行工具手动将应用打包为 .MSIX 的文章。

| 主题 | 说明 |
|-------|-------------|
| [正在生成包组件](../desktop/desktop-to-uwp-manual-conversion.md) | 创建包清单并添加基于目标的未着色资产（可选） |
| [使用 Makeappx.exe 工具创建 .MSIX 包或捆绑包](create-app-package-with-makeappx-tool.md) | MakeAppx.exe 创建、加密、解密应用程序包和捆绑包，并从中提取文件。 |
| [为包签名创建证书](create-certificate-package-signing.md) | 使用 PowerShell 工具为应用包签名创建和导出证书。 |
| [使用 SignTool 对应用包进行签名](sign-app-package-using-signtool.md) | 使用带证书的 SignTool 手动对应用包签名。 |

### <a name="advanced-topics"></a>高级主题

本部分包含更多高级主题，用于对大型或复杂应用进行组件化，从而提高打包和安装效率。 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用本部分中的任何高级功能。


| 主题 | 说明 |
|-------|-------------|
| [打包布局的包创建](packaging-layout.md) | 包布局是用于描述应用的包结构的单个文档。 它指定应用中的捆绑（主要和可选）、捆绑中的包以及包中的文件。 |
| [资产包简介](asset-packages.md) | 资产包是一种用作应用程序公用文件集中存放位置的程序包，可有效消除整个体系结构程序包中的重复文件。 |
| [用资产包和包折叠进行开发](package-folding.md) | 了解如何使用资产包和包折叠高效组织你的应用。 |
| [平面绑定应用包](flat-bundles.md) | 描述如何为应用的包文件创建平面束。 |

