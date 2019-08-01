---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: 应用流式安装
description: 应用程序流安装使你可以指定要 Microsoft Store 首先下载哪些应用程序部分。 当首先下载了该应用的基本文件后，用户就可以启动该应用并与其进行交互，而应用的其余部分则会在后台完成下载。
ms.date: 07/02/2019
author: dianmsft
ms.author: diahar
ms.topic: article
keywords: windows 10、.msix、uwp、流式处理安装
ms.localizationpriority: medium
ms.openlocfilehash: 95c4301b21ccbe7775afbac873565d778ba64e32
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68690006"
---
# <a name="app-streaming-install"></a>应用流式安装

应用程序流安装使你可以指定要 Microsoft Store 首先下载哪些应用程序部分。 当首先下载了该应用的基本文件后，用户就可以启动该应用并与其进行交互，而应用的其余部分则会在后台完成下载。

若要使用应用程序流安装, 需要将应用的文件划分为多个部分。 为此，需要创建一个内容组映射，该映射是一个与应用打包在一起的 XML 文件，使用它可设置下载优先级和顺序。 有关详细信息，请参见以下链接的主题。

有关将流式处理安装添加到应用的完整指南, 请查看此[博客系列](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/)。

| 主题 | 描述 |
|-------|-------------|
| [创建和转换源内容组映射](create-cgm.md) | 若要为应用程序准备好进行流式处理, 需要创建内容组映射。 本文介绍有关创建和转换内容组映射的具体信息，同时提供一些相关提示和技巧。 |