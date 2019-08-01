---
title: 可选包和相关集
description: 本文列出了用于了解如何通过应用安装程序安装相关集的资源。
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: c5a72f9b2b4561a223ea29cd34775fb8b3add260
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68685271"
---
# <a name="optional-packages-and-related-sets"></a>可选包和相关集

可选包中包含可与主要包相集成的内容。 这些内容可用于可下载内容 (DLC)，因为大小限制而划分大型应用，或者用于随附从原始应用中单独分隔出来的任何其他内容。

相关集是可选包的扩展。 它们允许跨主包和可选包强制实施一组严格的版本。 它们还使你能够从可选包加载本机代码 (C++)。

有关这些类型的包的详细信息, 请参阅[可选包和相关集创作](../package/optional-packages.md)。 如果刚开始处理可选包或相关集, 则可以通过以下附加博客文章来入门:

1.  [使用可选包扩展应用程序](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [构建第一个可选包](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [从可选包加载代码](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [用于创建相关集的工具](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)

从 Windows 10 秋季创意者更新 (Windows 10, 版本 1709) 开始, 现在可以通过应用安装程序安装相关集。 这样，可以向用户分发和部署相关集的应用包。 有关如何创建应用安装程序文件来定义相关集的详细信息, 请参阅[手动创建应用安装程序文件](how-to-create-appinstaller-file.md)。
