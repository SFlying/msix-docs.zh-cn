---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: 수동 앱 패키징
description: 本部分包含或链接，指向有关手动打包 Windows 应用的文章。
ms.date: 07/30/2019
ms.topic: article
keywords: windows 10, uwp, 패키징
ms.localizationpriority: medium
ms.openlocfilehash: 95b84bb77a99a5bec8138eed8267aebf7d5c377c
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723334"
---
# <a name="manual-app-packaging"></a>수동 앱 패키징

如果要创建 Windows 应用包并对其进行签名，但未使用 Visual Studio 来开发应用，则需要使用手动应用打包工具。

> [!IMPORTANT] 
> 如果使用 Visual Studio 开发 Windows 应用，建议使用 Visual Studio 向导来创建应用程序包并对其进行签名。 有关详细信息，请参阅使用[Visual Studio 打包 UWP 应用](packaging-uwp-apps.md)和[使用 visual Studio 从源代码打包桌面应用](../desktop/desktop-to-uwp-packaging-dot-net.md)。

## <a name="purpose"></a>용도

本部分包含或链接，指向有关手动打包 Windows 应用的文章。

| 항목 | 설명 |
|-------|-------------|
| [生成包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-manual-conversion) | 创建包清单并添加基于目标的未着色资产（可选） |
| [使用 Makeappx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md) | MakeAppx.exe는 앱 패키지와 번들을 만들고, 암호화 및 암호 해독하고, 파일을 추출합니다. |
| [手动打包桌面应用](../desktop/desktop-to-uwp-manual-conversion.md) | 了解如何使用**MakeApp**打包桌面应用程序。 |
| [为包签名创建证书](create-certificate-package-signing.md) | PowerShell 도구로 앱 패키지 서명용 인증서를 만들고 내보냅니다. |
| [使用 SignTool 对应用包进行签名](sign-app-package-using-signtool.md) | SignTool을 사용하여 인증서로 앱 패키지에 수동으로 서명합니다. |

### <a name="advanced-topics"></a>고급 항목

이 섹션에는 더 효율적인 패키징 및 설치에 대한 대규모 및 복잡한 앱을 구성 요소화하는 고급 항목이 포함되어 있습니다. 

> [!IMPORTANT]
> Microsoft Store에 앱을 제출하고자 하는 경우 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)에 문의하여 이 섹션에 나온 기능의 모든 사용에 대한 승인을 얻어야 합니다.


| 항목 | 설명 |
|-------|-------------|
| [资产包简介](asset-packages.md) | 자산 패키지는 아키텍처 패키지 전체에서 중복된 파일에 대한 필요성을 효율적으로 제거하는 응용 프로그램의 일반적인 파일에 대한 중앙 집중식 위치 역할을 수행할 패키지의 유형입니다. |
| [用资产包和包折叠进行开发](package-folding.md) | 자산 패키지 및 패키지 접기를 사용하여 앱을 효율적으로 구성하는 방법을 알아보세요. |
| [平面绑定应用包](flat-bundles.md) | 描述如何为应用的包文件创建平面束。 |
| [打包布局的包创建](packaging-layout.md) | 패키징 레이아웃은 앱 패키징 구조를 설명하는 단일 문서입니다. 앱(기본 및 선택)의 번들, 번들의 패키지, 패키지의 파일을 지정합니다. |
