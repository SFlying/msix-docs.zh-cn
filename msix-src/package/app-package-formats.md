---
title: 应用包格式
description: 本文介绍适用于某些情况的不同类型的 .MSIX 包格式。
ms.date: 07/02/2019
ms.topic: article
keywords: windows 10、.msix、uwp、desktop、package
ms.localizationpriority: medium
ms.openlocfilehash: 30d05cc1b2f007cb2d0049608c8b003ab04dde89
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091145"
---
# <a name="app-package-formats"></a>应用包格式

除了包含 Windows 应用的标准 .MSIX 包之外，还有几种不同类型的专用 .MSIX 包格式适用于某些情况。

## <a name="optional-packages"></a>可选包

可选包用于补充或扩展应用包的原始功能。 可先发布应用，晚些时候再发布可选包，或同时发布应用和可选包。 通过可选包来扩展应用，你将拥有将内容作为单独的应用包来分发和盈利的优势。 可选包通常由原始应用开发人员来开发，因为它们用主应用的标识来运行（与应用扩展不同）。 根据定义可选包的方式，你可以从可选包向主应用中加载代码、资产或代码和资产。 如果需要利用可以获取收益、许可和分发的内容来增强应用程序，则可选的包可能是正确的选择。 

有关更多详细信息，请参阅 [可选包和相关集创作](optional-packages.md)。

## <a name="app-streaming-install"></a>应用流式安装

流式安装是一种优化应用程序如何传递给用户的方法。 无需等待整个应用下载完成再使用，只要下载完所需部分，用户便可开始使用应用。 完全由作为开发人员的你来决定，将应用分段为用于基本激活的必需部分，并为应用的其余部分启动其他内容。 

有关更多详细信息，请参阅 [应用程序流式处理安装](streaming-install.md)。

## <a name="flat-bundle-packages"></a>平面捆绑包

平面绑定应用程序包类似于 [常规应用捆绑](packaging-uwp-apps.md#types-of-app-packages)包，只不过此类应用包仅包含对这些应用包的 *引用* 。 由于平面捆绑包包含应用包的引用而不是文件本身，因而可减少打包和下载应用所需的时间。

有关更多详细信息，请参阅 [平面捆绑应用程序包](flat-bundles.md)。

## <a name="asset-packages"></a>资产包

资产包是可执行文件的通用集中式源，或者是可由应用程序使用的不可执行文件。 它们通常是非处理器或特定于语言的文件。 例如，可以在一个资产包中包含一系列图片，在另一个资产包中包含视频，两种资产都由同一个应用使用。 如果你的应用支持多个体系结构和多种语言，则这些资产可能包括在体系结构包或资源包中，但这也意味着将跨各种体系结构包多次复制资产，从而占用磁盘空间。 如果使用资产包，则只需将其包含在整个应用包中一次。 

有关更多详细信息，请参阅 [资产包简介](asset-packages.md)。

## <a name="resource-packages"></a>资源包

资源包是仅资产包，使你的应用能够适应多个屏幕大小和系统语言。 资源包面向用户语言、系统规模和 DirectX 功能，使应用可根据多种用户方案进行定制。 尽管应用包可以包含若干资源，但操作系统只根据用户设备下载相关资源，从而节省带宽和磁盘空间。

## <a name="app-extensions"></a>应用扩展

[应用扩展](/uwp/api/windows.applicationmodel.appextensions) 使你的应用能够承载其他应用提供的内容。 从这些应用发现、枚举和访问只读内容。

如果应用支持扩展，任何开发人员均可为该应用提交扩展。 因此，在加载未经过预测试的扩展时，主机应用需要非常可靠。 应将扩展视为不受信任。

应用程序不能从扩展加载代码。 如果需要执行代码，请考虑应用服务。

## <a name="app-services"></a>应用服务

Windows 应用服务通过允许应用向其他应用提供服务，实现应用到应用的通信。 应用服务允许你创建应用可在同一设备上调用的无 UI 服务，从 Windows 10 版本 1607 开始，应用可在远程设备上调用这些服务。 有关详细信息，请参阅[创建和使用应用服务](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

应用服务类似于设备上的 web 服务。 应用服务作为后台任务在主机应用中运行，并可向其他应用提供其服务。 例如，应用服务可能会提供其他应用可能使用的条形码扫描仪服务。 应用的企业套件中可能有一个通用的拼写检查应用服务，该服务可供套件中的其他应用使用。

## <a name="modification-packages"></a>修改包 
修改包允许 IT 专业人员自定义应用，而无需重新打包。 在 Windows 10 版本1809中，我们引入了一种称为 *修改包*的新类型的 .msix 包。 修改包还可以是可能没有激活点的插件/加载项。 IT 专业人员可以使用此功能灵活地更改 .MSIX 容器，以便应用程序可以通过其企业的自定义进行重叠。 

## <a name="see-also"></a>另请参阅

[创建和使用应用服务](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[资产包简介](asset-packages.md)  
[使用打包布局创建包](packaging-layout.md)  
[可选包和相关集创作](optional-packages.md)  
[用资产包和包折叠进行开发](package-folding.md)  
[应用流式安装](streaming-install.md)  
[平面捆绑应用包](flat-bundles.md)  
[Windows.ApplicationModel.AppService 命名空间](/uwp/api/Windows.ApplicationModel.AppService)  
[Windows.ApplicationModel.Extensions 命名空间](/uwp/api/windows.applicationmodel.appextensions)