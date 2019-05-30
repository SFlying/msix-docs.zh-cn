---
title: MSIX SDK 1.5 版发行说明
description: MSIX SDK 版本 1.5 的发行说明
author: c-don
ms.author: cdon
ms.date: 02/06/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: f6b6350c6d55b0d330d73c4588d4c9c50fb151e1
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "58900489"
---
# <a name="msix-sdk-15-update"></a>MSIX SDK 1.5 更新

SDK 版本 (1.5) 中，我们主要投资时减少 SDK 的二进制代码大小。 有关我们的 SDK 的主要反馈之一就是大小，尤其是对于移动应用具有二进制的依赖关系要成本约 5 MB 不令人满意。 

若要解决此问题，我们替换为用于 XML 分析与 Android 和 iOS/MacOS 上的收件箱二进制文件的静态二进制依赖关系。 作为二进制代码大小缩减的一部分，我们还可使用 SDK 生成脚本中添加不同功能的开关。 通过仅将在库中添加应用所需的功能，可进一步减少 MSIX SDK 二进制大小的大小。 

可以在 GitHub 上获取最新的 SDK。 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.5" data-linktype="external">GitHub 上的 MSIX SDK</a></p></div>

