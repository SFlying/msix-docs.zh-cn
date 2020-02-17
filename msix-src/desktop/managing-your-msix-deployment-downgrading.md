---
Description: 本文提供了有关 MSIX 应用程序如何处理被降级的详细信息。
title: 安装 MSIX 应用包的早期版本
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 9e6ab85924ae7120ba2540d6beaeb827a86b1764
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074243"
---
# <a name="install-earlier-versions-of-an-msix-app-package"></a>安装 MSIX 应用包的早期版本

如果之前没有删除应用程序，可以在设备上执行早期版本的 MSIX 应用程序包。 如果触发同一应用程序早期版本的安装操作，这将导致客户端检查安装 MSIX 应用程序包中包含的应用清单文件。 检查 appxmanifest.xml 文件将查明当前已安装应用与设备上正在执行的安装程序之间的增量差异。 此过程会维护 *appdata* 文件夹中存储的数据。 如果安装了早期版本的 MSIX 应用程序包，则在安装升级后版本时所做的任何更改都不会撤消。
