---
title: MSIX SDK 版本 1.6 发行说明
description: 发行 1.6 MSIX SDK 的发行说明
author: c-don
ms.author: cdon
ms.date: 02/06/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b964c8942d3da85d85cbff87f78ef8c9399ab142
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "58900319"
---
# <a name="msix-sdk-16-update"></a>MSIX SDK 1.6 更新版

SDK 版本 (1.6) 中，我们在我们的合作伙伴反馈，并添加更多 Api，开发人员提供更多选项和灵活地处理 MSIX 包。 

## <a name="utf8-api-variants"></a>UTF8 API 变体

在此 SDK 版本中，我们将添加大约 14 的新 UTF8 API 变体的现有 API 调用。 使用包含的这些新的 Api，开发人员可以选择要用于字符串操作根据其环境/平台的 Utf8 变体。 与 AppxPackaging Api 一样调用方负责释放使用 LPSTR * out 参数的内存。

以下是新的 UTF8 接口：
- IAppxBlockMapFileUtf8
- IAppxBlockMapReaderUtf8
- IAppxBundleManifestPackageInfoUtf8
- IAppxBundleReaderUtf8
- IAppxFactoryUtf8
- IAppxFileUtf8
- IAppxManifestApplicationUtf8
- IAppxManifestPackageDependencyUtf8
- IAppxManifestPackageIdUtf8
- IAppxManifestPropertiesUtf8
- IAppxManifestQualifiedResourceUtf8
- IAppxManifestResourcesEnumeratorUtf8
- IAppxManifestTargetDeviceFamilyUtf8
- IAppxPackageReaderUtf8


## <a name="override-language-selection"></a>重写语言选择 

默认情况下，处理应用程序捆绑包时，MSIX SDK 返回通过选择也是在系统上的默认语言是适用的语言包。 此 API 允许应用程序以枚举都可用并且重写将处理应用程序捆绑包时返回的语言包的语言包。 

## <a name="other-updates-and-improvements"></a>其他更新和改进

在此更新中， 
- 更新到 1.0.2q OpenSSL lib 依赖关系
- 修复了我们如何处理国际字符 

可以在 GitHub 上获取最新的 SDK。 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.6" data-linktype="external">GitHub 上的 MSIX SDK</a></p></div>

