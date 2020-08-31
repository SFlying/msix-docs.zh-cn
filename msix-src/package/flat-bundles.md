---
title: 平面捆绑应用包
description: 介绍如何创建平面捆绑包来将你的应用的 .appx 包文件与对应用包的引用进行绑定。
ms.date: 02/05/2020
author: dianmsft
ms.author: diahar
ms.topic: article
keywords: windows 10，.msix，打包，包配置，平面束
ms.localizationpriority: medium
ms.openlocfilehash: a4d67d89b922cb8d9f850157e9a673017f92c43d
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091115"
---
# <a name="flat-bundle-app-packages"></a>平面捆绑应用包 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用平面捆绑包。

平面捆绑是一种用于捆绑应用包文件的改进方式。 典型的 Windows 应用包文件使用多级别打包结构，其中的应用包文件需要包含在捆绑包中，平面束只需引用应用包文件即可消除这种需求，从而使它们可以在应用捆绑包外使用。 由于应用包文件不再包含在绑定中，因此可以对其进行并行处理，从而减少上传时间、更快的发布 (，因为每个应用包文件可同时处理) ，最终更快的开发迭代。

![平面捆绑包示意图](images/bundle-combined.png)

平面捆绑包的另一个好处是减少了包的数量。 由于仅引用应用包文件，因此，如果包没有在两个版本之间发生更改，则两个版本的应用可以引用同一包文件。 这样，在为应用的下一个版本构建包时，你只需要创建发生了更改的应用包。
默认情况下，平面绑定将在同一文件夹内引用应用包文件。 但是，该引用可以更改为其他路径（相对路径、网络共享和 http 位置）。 要执行该操作，你必须在创建平面捆绑包期间直接提供 **BundleManifest**。 

## <a name="how-to-create-a-flat-bundle"></a>如何创建平面捆绑包

可以使用 MakeAppx.exe 工具或使用包布局定义捆绑包结构来创建平面捆绑包。

### <a name="using-makeappxexe"></a>使用 MakeAppx.exe

若要使用 MakeAppx.exe 创建平面束，请照常使用 "MakeAppx.exe 束" 命令，但使用/fb 开关来生成平面应用捆绑 (文件，该文件将非常小，因为它仅引用应用包文件，并且不包含任何实际有效负载) 。 

以下是命令语法示例：

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

有关使用 MakeAppx.exe 的详细信息，请参阅[使用 MakeAppx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md)。

### <a name="using-packaging-layout"></a>使用包布局

或者，你也可以使用包布局创建平面捆绑包。 要执行该操作，请在应用程序包清单的 **PackageFamily** 元素中将 **FlatBundle** 属性设置为 **true**。 要了解有关包布局的详细信息，请参阅[用包布局创建程序包](packaging-layout.md)。

## <a name="how-to-deploy-a-flat-bundle"></a>如何部署平面捆绑包 

在部署平面束之前，除了应用捆绑包外 (每个应用包) 必须使用相同的证书进行签名。 这是因为)  ( 的所有应用程序包文件现在都是独立的文件，并且不再包含在应用捆绑包 ( .msixbundle) 文件中。

对包进行签名后，可以通过以下选项之一安装应用：
* 双击要随应用安装程序一起安装的应用捆绑包文件。
* 在 PowerShell 中使用 [add-appxpackage cmdlet](/powershell/module/appx/add-appxpackage?view=win10-ps) 并指向应用捆绑包文件， (假设应用包是应用捆绑包希望) 的位置。 

不能自行部署单层包的单独 .appx/. .msix 包。 它们必须通过 .appxbundle/. .msixbundle 部署。 但是，你可以在初始安装后更新单层包的各个 .appx/. .msix 包。 如果你确实要更新单个 .appx/. .msix 包，你还需要更新平面捆绑包的清单。

例如，如果你的 v1 平面捆绑包由 .msixbundle、.msix、.msix 和 .msix 组成，并且你知道你的 v2 捆绑包仅更改了资产包，则只需生成 .msixbundle 和，就能安装该更新了。 "）"。 您必须为 v2 生成 .msixbundle，因为捆绑将跟踪 .msix 包的所有版本。 通过将碰撞的版本 .msix 为 v2，你需要具有此新引用的 .msixbundle。 .Msixbundle 可以包含对 v1 .msix 和 .msix 的引用。平面绑定的 .msix 包不需要具有相同的版本号。