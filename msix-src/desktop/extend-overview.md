---
Description: 有关基于源代码创建 MSIX 程序包的主题概述
title: 基于代码生成 MSIX 程序包
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5d0f2105f9ac7fa245b2ced572a7c9c75485b2f0
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77074033"
---
# <a name="extend-your-packaged-applications"></a>扩展打包的应用程序

MSIX 允许你使用应用扩展和可选程序包来轻松扩展应用程序。 应用扩展提供的功能类似于插件、外接程序和加载项在其他平台上提供的功能。 你可以使应用程序成为扩展主机，以允许它使用打包的扩展中的内容和部署事件。 Windows 10 周年纪念版（版本 1607，内部版本 10.0.14393）中引入了应用扩展。

可选程序包对于拆分大型或复杂的应用，或者向已发布的应用添加新组件很有用。 使用 Visual Studio 2017 版本 15.7 和 .NET Native 2.1，你可以从 C++ 和 C# 可选程序包加载可执行代码。

应用扩展是一个开放的生态系统，可供任何人用来增强你的应用。 对于谁可以创建应用扩展，未设门槛或限制。 可选程序包是一个封闭的生态系统，作为发布者，你可以决定允许谁为你的主程序包创建可选程序包。

应用扩展也是独立的程序包。 它们可以是独立的应用，不能依赖于另一个应用进行部署。  可选程序包需要主程序包，并且没有主程序包就无法运行。

|主题| 说明 |
|:---|:---|
|[创建和托管应用扩展](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-an-extension?context=/windows/msix/render)|本部分讨论了如何在 MSIX 程序包中创建并托管应用扩展。 |
[应用扩展的自定义属性](custom-props-app-extensions.md)|本部分讨论了如何使用应用扩展的自定义属性。 |
|[使用可选程序包扩展应用](../package/optional-packages-with-executable-code.md)| 本部分讨论了如何利用可选程序包模型将内容加载到主程序包中。 |


