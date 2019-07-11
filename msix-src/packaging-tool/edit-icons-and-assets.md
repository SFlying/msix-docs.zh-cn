---
title: 使用 MSIX 打包工具编辑图标和资产
description: 介绍如何使用 MSIX 打包工具编辑图标和资产。
author: sharlaakers
ms.author: shakers
ms.date: 06/27/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 4a44d1e97ab250f033dbf54d0c921dbe05506eda
ms.sourcegitcommit: d5d91ea18875d26b1661a9ef2c750494fa7cf75d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798608"
---
# <a name="edit-icons-and-assets-using-the-msix-packaging-tool"></a>使用 MSIX 打包工具编辑图标和资产

当使用 MSIX 打包工具时，可在转换过程中修改应用包资产的方式有以下几种：

* 可以在转换过程中修改应用包资产。
* 创建包后，可以通过[包编辑器](package-editor.md)修改应用包资产。

进行修改时，请参阅[图标和资产指南](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos)。

## <a name="modify-assets-during-the-conversion-process"></a>在转换过程中修改资产

在 MSIX 打包工具工作流的受监视安装阶段中，MSIX 打包工具将捕获你对应用程序的文件或文件夹进行的任何更改。 如果在监视过程中更改资产，这些更改将捕获到最终包中。

## <a name="modify-assets-after-your-package-has-been-created"></a>在创建包后修改资产

若要在创建 MSIX 包后修改应用包资产，请在[包编辑器](package-editor.md)中打开 MSIX 包，然后打开“包文件”  页。 可以删除现有资产或添加新图标或资产。

- 若要添加新的资产文件，请右键单击资产文件夹，然后选择“添加文件”  或“添加文件夹”  。
- 若要删除现有资产文件，请右键单击该文件，并选择“删除”  。

若要验证资产更改，请转到“包信息”  页，并打开清单文件。 确认 [uap:VisualElements](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-uap-visualelements) 节点中表示了已添加或删除的资产。