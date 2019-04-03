---
title: 跨多个设备重复 MSIX 打包工具设置
description: 了解如何跨多个设备重复 MSIX 打包工具设置
author: dianmsft
ms.author: diahar
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.openlocfilehash: ea3f1d5b49be685315eda55c2fbb96704568337f
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900269"
---
# <a name="duplicate-msix-packaging-tool-settings-across-multiple-devices"></a>跨多个设备重复 MSIX 打包工具设置 

请遵循这些简单步骤以跨多个设备，重复 MSIX 打包工具设置或备份和还原设置，如果您需要在设备进行重镜像。 此功能是可以开始于 1.2019.110.0 新版 MSIX 打包工具。 

## <a name="build-the-baseline"></a>生成基线

1. 在安装了 MSIX 打包工具与设备上，单击设置图标。
2. 配置设置以满足您的环境的特定要求。
    > [!NOTE]
    > 这通常意味着调整生成的文件和注册表排除项，但可以包括的任何设置。
3. 配置设置后，请确保单击保存设置，然后关闭该工具。  

## <a name="back-up-the-settings-configuration"></a>备份设置配置

从该设备与配置基线，将以下文件复制到一个已知的正确位置，例如网络服务器共享。

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml  

> [!NOTE]
> 请确保此文件不被删除或更改，因为它将是您对于所有其他安装的模板。

## <a name="restore-the-settings-configuration"></a>还原的设置的配置

使用安装了 MSIX 打包工具设置新设备时，将从已知的良好位置复制 settings.xml 回复到本地设备： 

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml 

## <a name="considerations"></a>注意事项

* 请确保<DefaultSaveLocation>条目被删除或是用户的有效位置。 如果未提供值或位置无效，该工具将默认为 C:\Users\\%username%\desktop。

* 设置文件只能复制到相同版本的打包工具或更高版本。 如果在 settings.xml 中的版本复制到该工具的较旧版本，可能不支持设置。 我们建议了解的最新修补程序和打包最好始终使用该工具的最新版本。  
