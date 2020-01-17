---
title: 什么是 MSIX？
description: 本文介绍 MSIX 打包格式（一种适用于所有 Windows 应用的现代打包体验）的基础知识。
ms.date: 12/02/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 620d71b0e92fa9e22700d51c9c8a118accf6d207
ms.sourcegitcommit: f095305b47b5ba14f96413ee520777be45090e3b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75877106"
---
# <a name="what-is-msix"></a>什么是 MSIX？

MSIX 是一种 Windows 应用包格式，可以为所有 Windows 应用提供现代打包体验。 MSIX 包格式保留了现有应用包和/或安装文件的功能，此外，它还为 Win32、WPF 和 Windows 窗体应用启用了全新的现代打包和部署功能。

> [!TIP]
> 访问 [MSIX 技术社区](https://aka.ms/msixcommunity)页，了解各种讨论内容和最新信息。

下面简要介绍了 MSIX 的一些亮点：

* **将现有的 Windows 应用打包。** 使用 [MSIX 打包工具](packaging-tool/mpt-overview.md)可为任何旧式或新式 Windows 应用创建 MSIX 包。 MSIX 打包工具简化了打包体验，提供交互式用户界面或命令行来转换和打包 Windows 应用。
* **安装 MSIX 应用包。** 使用[应用安装程序](app-installer/app-installer-root.md)可安装或更新在本地或任何内容分发网络上提供的任何 MSIX 应用包。
* **将运行时修补程序应用于已打包的应用。** [包支持框架](psf/package-support-framework-overview.md)是一个开放源代码工具包，有助于在无权访问源代码时将修补程序应用于现有桌面应用，以便其在 MSIX 容器中运行。
* **随时随地使用 MSIX。** 由于使用了开源 [MSIX SDK](msix-sdk/sdk-overview.md)，MSIX 包的用途更广泛，并且不区分平台。 该 SDK 提供在任何平台（包括 Windows 10 平台和非 Windows 10 平台）上验证和解包应用包所需的全部 API。

此视频介绍了 MSIX 打包可帮助你简化和改进应用安装与部署工作流的关键方法。

<br/>

> [!VIDEO https://www.youtube.com/embed/phrD081sMWc]