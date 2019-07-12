---
title: 跨多个设备复制 MSIX 打包工具设置
description: 了解如何跨多个设备复制 MSIX 打包工具设置
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 4e153b384594fc9981ff44327bb5ff0afa918913
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829277"
---
# <a name="duplicate-msix-packaging-tool-settings-across-multiple-devices"></a>跨多个设备复制 MSIX 打包工具设置 

请遵循这些简单的步骤跨多个设备复制 MSIX 打包工具的设置，或者在需要重建设备的映像时备份和还原设置。 从 MSIX 打包工具版本 1.2019.110.0 开始已推出此功能。 

## <a name="build-the-baseline"></a>生成基线

1. 在装有 MSIX 打包工具的设备上，单击“设置”图标。
2. 根据具体的环境要求配置设置。
    > [!NOTE]
    > 这通常意味着需要调整文件和注册表排除项，但可以包含任何设置。
3. 配置设置后，请确保单击“保存设置”，然后关闭工具。  

## <a name="back-up-the-settings-configuration"></a>备份设置配置

在配置了基线的设备上，将以下文件复制到已知适当的位置，例如网络服务器共享。

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml  

> [!NOTE]
> 请务必不要删除或更改此文件，因为它是所有其他安装项目的模板。

## <a name="restore-the-settings-configuration"></a>还原设置配置

设置新设备并在其中安装 MSIX 打包工具后，将已知适当位置的 settings.xml 复制回到本地设备： 

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml 

## <a name="considerations"></a>注意事项

* 确保 <DefaultSaveLocation> 条目已删除或者是用户的有效位置。 如果未提供该值或位置无效，该工具默认会使用 C:\Users\\%username%\desktop。

* 只能将设置文件复制到打包工具的相同版本或更高版本。 如果将 settings.xml 中的版本复制到工具的更低版本，可能不会采用这些设置。 我们建议使用最新版本的工具，以获得最新修复程序和最佳的打包体验。  
