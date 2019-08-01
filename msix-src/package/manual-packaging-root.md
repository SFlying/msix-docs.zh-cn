---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: 手动打包应用
description: 本部分包含或链接, 指向有关手动打包 Windows 应用的文章。
ms.date: 07/30/2019
ms.topic: article
keywords: windows 10, uwp, 打包
ms.localizationpriority: medium
ms.openlocfilehash: b1e367050f0c1b84aafb0ead037120bd24dea448
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689986"
---
# <a name="manual-app-packaging"></a>手动打包应用

如果要创建 Windows 应用包并对其进行签名, 但未使用 Visual Studio 来开发应用, 则需要使用手动应用打包工具。

> [!IMPORTANT] 
> 如果使用 Visual Studio 开发 Windows 应用, 建议使用 Visual Studio 向导来创建应用程序包并对其进行签名。 有关详细信息, 请参阅使用[Visual Studio 打包 UWP 应用](packaging-uwp-apps.md)和[使用 visual Studio 从源代码打包桌面应用](../desktop/desktop-to-uwp-packaging-dot-net.md)。

## <a name="purpose"></a>用途

本部分包含或链接, 指向有关手动打包 Windows 应用的文章。

| 主题 | 描述 |
|-------|-------------|
| [使用 Makeappx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md) | MakeAppx.exe 创建、加密、解密应用程序包和捆绑包，并从中提取文件。 |
| [手动打包桌面应用](../desktop/desktop-to-uwp-manual-conversion.md) | 了解如何使用**MakeApp**打包桌面应用程序。 |
| [为包签名创建证书](create-certificate-package-signing.md) | 使用 PowerShell 工具为应用包签名创建和导出证书。 |
| [使用 SignTool 对应用包进行签名](sign-app-package-using-signtool.md) | 使用带证书的 SignTool 手动对应用包签名。 |

### <a name="advanced-topics"></a>高级主题

本部分包含更多高级主题，用于对大型或复杂应用进行组件化，从而提高打包和安装效率。 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用本部分中的任何高级功能。


| 主题 | 描述 |
|-------|-------------|
| [资产包简介](asset-packages.md) | 资产包是一种用作应用程序公用文件集中存放位置的程序包，可有效消除整个体系结构程序包中的重复文件。 |
| [用资产包和包折叠进行开发](package-folding.md) | 了解如何使用资产包和包折叠高效组织你的应用。 |
| [平面绑定应用包](flat-bundles.md) | 描述如何为应用的包文件创建平面束。 |
| [打包布局的包创建](packaging-layout.md) | 包布局是用于描述应用的包结构的单个文档。 它指定应用中的捆绑（主要和可选）、捆绑中的包以及包中的文件。 |
