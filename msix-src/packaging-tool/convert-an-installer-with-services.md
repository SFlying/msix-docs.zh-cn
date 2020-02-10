---
title: 使用服务转换安装程序
description: 本文介绍如何使用 .MSIX 打包工具将现有的安装程序与服务转换为 .MSIX
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、.MSIX、.MSIX 打包工具、服务
ms.localizationpriority: medium
ms.openlocfilehash: 6766d97533724e65eeee885195c535d950d93cc4
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072787"
---
# <a name="convert-an-installer-that-includes-services"></a>转换包含服务的安装程序

Windows 10 版本2004引入了对运行包含服务的 .MSIX 包的支持。 可以使用 .MSIX 打包工具获取现有的服务安装程序，并将其转换为 .MSIX。 此支持由[.Msix 打包工具](tool-overview.md)（1.2019.1220.0）的2020年1月版发布。 将打包的 .MSIX 与服务一起使用后，将需要管理员权限才能在计算机上安装。

## <a name="instructions"></a>说明

若要转换包含服务的安装程序，请使用 .MSIX 打包工具，就像使用任何[应用程序包](create-app-package.md)一样。 选择包含服务的安装程序，您将在创建 .MSIX 包的最后一步之前看到 "**服务**报表" 页。

"**服务**报表" 页将列出在转换过程中检测到的服务。 所**含**的表中将显示具有所需的所有信息并支持的服务。 **排除**的表中将显示需要其他信息、需要修复或不受支持的服务。

若要修复服务或查看有关服务的其他数据，请双击表中的服务条目以查看弹出窗口，其中包含有关服务的详细信息。 如果需要，可以编辑此信息。

- **项名称：** 服务的名称。 这是不可编辑的。
- **说明：** 服务项的说明。
- **显示名称：** 服务的显示名称。
- **映像路径：** 服务可执行文件的位置。 这是不可编辑的。
- **启动帐户：** 服务的启动帐户。
- **启动类型：** 服务的启动类型。 支持**自动**、**手动**和**禁用**。
- **参数：** 要在服务启动时运行的参数。
- **依赖项：** 服务的依赖关系。

修复了某个服务后，可以将其移动到**包含**的表中，或者，如果不希望在最终包中使用它，也可以选择将它保留在**排除**的表中。 然后，可以继续执行最后一个步骤来创建 .MSIX 包。

## <a name="known-limitations"></a>已知限制

服务可执行文件路径（也称为图像路径）当前不可编辑。 若要解决与路径有关的任何问题，必须在转换安装程序之前手动编辑服务可执行文件路径。 或者，在转换后，可以使用 .MSIX 打包工具中的[包编辑器](package-editor.md)手动编辑清单。

服务报表当前在**包编辑器**中不可用。 你必须手动编辑清单以更改 .MSIX 包中包含的服务。

目前，我们不支持在包外具有依赖关系的服务。

## <a name="add-a-service-manually-using-your-manifest"></a>使用清单手动添加服务

如果手动将服务添加到应用程序，则需要[将服务添加](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-service)到应用程序清单。 这需要将[受限功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)添加到应用程序。
