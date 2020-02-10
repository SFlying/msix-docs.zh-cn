---
title: AppInstaller 文件生成器工具
description: 本文详细介绍了 .MSIX 工具包中的 AppInstaller 文件生成器工具。
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10、.msix、.msix 工具包
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: eaf2234423ccc20cb2bb5f1024a7f8144722f1c1
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073553"
---
# <a name="appinstaller-file-builder-tool"></a>AppInstaller 文件生成器工具

可使用 .MSIX 工具包中的[AppInstaller 文件生成器工具](https://github.com/microsoft/MSIX-Toolkit/tree/master/AppInstallerFileBuilder)来配置 .msix 打包的应用，以便查看更新的中央源。 此工具有助于构造 AppInstaller （AppInstaller）文件。 AppInstaller 文件可与不同的[部署选项](../desktop/managing-your-msix-deployment-overview.md)一起使用。

## <a name="create-an-appinstaller-file"></a>创建 AppInstaller 文件

1. 下载[AppInstaller 文件生成器](https://github.com/microsoft/MSIX-Toolkit/releases/download/1.3.3/AppInstallerFileBuilder_1.2019.1001.0.msix)
2. 安装并启动**AppInstaller 文件生成器**应用。
3. 选择 "**获取包信息**" 按钮，然后浏览到要使用 AppInstaller 文件安装的 .msix 应用。
4. 指定应用程序包所需的更新选项。
    > [!Note]
    > 根据所选的最低操作系统版本，选项可能有所不同。 若要查看或配置这些值，必须在此工具内启用 "**应用启动时检查更新**"。
5. 继续执行向导，根据需要添加[可选包](../package/optional-packages.md)、[修改包](//modification-packages.md)、[相关包](../package/optional-packages.md)和依赖项。
6. 查看摘要详细信息后，选择 "**生成**" 按钮以创建 AppInstaller 文件。
