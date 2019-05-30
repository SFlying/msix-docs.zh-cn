---
title: MSIX SDK 版本 1.4 的发行说明
description: 发行 1.4 MSIX SDK 的发行说明
author: c-don
ms.author: cdon
ms.date: 09/12/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5a5e6663263300abbccbc303b2a3f67c6bfdcdfc
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "58900309"
---
# <a name="msix-sdk-14-update"></a>MSIX SDK 1.4 更新

SDK 版本 (1.4) 中，我们会继续添加更多的功能和性能改进我们的 SDK。  1.4 的发行版，开发人员可以使用 SDK 现在解包和提取应用程序捆绑包和[平面捆绑包](https://docs.microsoft.com/en-us/windows/uwp/packaging/flat-bundles?context=/windows/msix/render)。 

捆绑包的支持下，客户端应用程序可以现在提取应用程序捆绑包和仅下载并提取适用绑定内部的包。 在此版本中，捆绑包支持扩展到也平面捆绑包。 这意味着，客户端应用程序使用该绑定可以访问绑定清单，并指定它想要提取的应用程序包留给开发人员选择和控制。 SDK 调用本机操作系统语言和区域设置的信息，并提供到客户端应用程序的可用于做出适当包捆绑包也选择该信息。

新的 sdk 中，我们添加了对 MSXML6 为 Windows 删除带框依赖关系并因此减少了二进制文件的大小，然后使用本机 XML 库的支持。 

我们还在 Windows 中删除依赖关系，以及删除对 zlib 的依赖关系 （第三方库） 和 MacOS、 iOS 和 Android(AOSP) 上使用内置实现。  此外，我们做了其他改进，减少在所有平台上的二进制大小。 

以及性能和功能的改进，我们还将包括更好的示例，开发人员可以使用若要开始使用 SDK。 使用示例，开发人员可以了解如何实现某些读取 msix 包所需的公共接口。 

可以在 GitHub 上获取最新的 SDK。 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.4" data-linktype="external">GitHub 上的 MSIX SDK</a></p></div>

