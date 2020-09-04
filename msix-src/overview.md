---
title: 什么是 MSIX？
description: 本文介绍 MSIX 打包格式（一种适用于所有 Windows 应用的现代打包体验）的基础知识。
ms.date: 12/02/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 108fb7be52c24c52f8b65fd3c17b9c35feb96a30
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091175"
---
# <a name="what-is-msix"></a>什么是 MSIX？

MSIX 是一种 Windows 应用包格式，可以为所有 Windows 应用提供现代打包体验。 MSIX 包格式保留了现有应用包和/或安装文件的功能，此外，它还为 Win32、WPF 和 Windows 窗体应用启用了全新的现代打包和部署功能。

MSIX 可使企业掌握最新信息，并确保其应用程序始终保持最新状态。 它使 IT 专业人员和开发人员能够交付以用户为中心的解决方案，同时通过减少重新打包的需求，来降低应用程序的所有权成本。

## <a name="key-features"></a>关键功能

* **可靠性。** MSIX 提供可靠的安装，在数百万次安装中，其成功率达到了引以为豪的 99.96%，并提供有保证的卸载体验。
* **网络带宽优化。** MSIX 只会下载 64k 大小的数据块，可以减轻对网络带宽的影响。 此项优势是利用 MSIX 应用包中的 AppxBlockMap.xml 文件来实现的（参阅下面的详细信息）。 MSIX 旨在用于新式系统和云。
* **磁盘空间优化。** 使用 MSIX 时，不会在应用之间复制文件，Windows 将跨应用管理共享的文件。 应用仍然彼此独立，因此更新不会影响共享该文件的其他应用。 即使平台跨应用管理共享的文件，也能保证干净卸载。

## <a name="highlights"></a>亮点

* **将现有的 Windows 应用打包。** 使用 [MSIX 打包工具](./packaging-tool/tool-overview.md)可为任何旧式或新式 Windows 应用创建 MSIX 包。 MSIX 打包工具简化了打包体验，提供交互式用户界面或命令行来转换和打包 Windows 应用。
* **安装 MSIX 应用包。** 使用[应用安装程序](app-installer/app-installer-root.md)可安装或更新在本地或任何内容分发网络上提供的任何 MSIX 应用包。
* **将运行时修补程序应用于已打包的应用。** [包支持框架](psf/package-support-framework-overview.md)是一个开放源代码工具包，有助于在无权访问源代码时将修补程序应用于现有桌面应用，以便其在 MSIX 容器中运行。
* **随时随地使用 MSIX。** 由于使用了开源 [MSIX SDK](msix-sdk/sdk-overview.md)，MSIX 包的用途更广泛，并且不区分平台。 该 SDK 提供在任何平台（包括 Windows 10 平台和非 Windows 10 平台）上验证和解包应用包所需的全部 API。

## <a name="introduction-video-to-msix-and-resources"></a>MSIX 的简介视频和资源

此视频介绍了 MSIX 打包可帮助你简化和改进应用安装与部署工作流的关键方法。

<br/>

> [!VIDEO https://www.youtube.com/embed/phrD081sMWc]

访问 [MSIX 技术社区](https://aka.ms/msixcommunity)页，获取有关 MSIX 的各种讨论内容和最新信息。 有关 MSIX 的其他学习资源，请参阅[此文](resources.md)。

## <a name="inside-an-msix-package"></a>MSIX 包的内部

![MSIX 包示意图](package/images/msixpackage.png)

### <a name="app-payload"></a>应用有效负载

有效负载文件是生成应用时创建的应用代码文件和资产。

### <a name="appxblockmapxml"></a>AppxBlockMap.xml

包块映射文件是一个 XML 文档，其中包含应用的文件列表，以及存储在包中的每个数据块的索引和加密哈希。 为包签名时，将使用数字签名来验证和保护块映射文件本身。 使用块映射文件能够以增量方式下载和验证 MSIX 包，在安装应用文件后，还可以使用块映射文件来支持对应用文件进行差异更新。

### <a name="appxmanifestxml"></a>AppxManifest.xml

包清单是一个 XML 文档，其中包含系统在部署、显示和更新 MSIX 应用时所需的信息。 此信息包括包标识、包依赖项、所需功能、可视元素和扩展点。

### <a name="appxsignaturep7x"></a>AppxSignature.p7x

为包签名时，将生成 AppxSignature.p7x。 在安装之前，需要为所有 MSIX 包签名。 借助 AppxBlockmap.xml，平台可以安装包，并可对平台进行验证。

## <a name="supported-platforms"></a>支持的平台

有关支持 MSIX 的平台的完整列表，请参阅[此文](supported-platforms.md)。

## <a name="benefits-of-app-containers"></a>应用容器的优势

使用 MSIX 打包的应用在一个轻型应用容器中运行。 MSIX 应用进程及其子进程在该容器内部运行，并使用文件系统和注册表虚拟化进行隔离。 所有 MSIX 应用都可以读取全局注册表。 MSIX 应用写入到其自身的虚拟注册表和应用程序数据文件夹，卸载或重置应用时会删除此数据。 其他应用无法访问 MSIX 应用的虚拟注册表或虚拟文件系统。