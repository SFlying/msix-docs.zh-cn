---
author: mcleanbyron
Description: 包支持框架可帮助您修复阻止 MSIX 容器中运行桌面应用程序的问题。
Search.Product: eADQiWindows 10XVcnh
title: 包支持框架
ms.author: mcleans
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2eccc27d426b3192112770db91c7ab65745738ff
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900859"
---
# <a name="package-support-framework"></a>包支持框架

包支持框架是一个开放源代码工具包，可帮助你应用修补程序到现有的 win32 应用程序时不能访问源代码，以便它可以在 MSIX 容器中运行。 包支持框架可帮助遵循最佳做法现代的运行时环境的应用程序。

若要创建包支持框架，我们利用[Detour](https://www.microsoft.com/en-us/research/project/detours)技术，这是由 Microsoft Research (MSR) 开发的开放源代码框架，帮助 API 重定向以及挂接。

此框架是开放源代码，轻量的并且您可以使用它来解决应用程序问题快速。 它还提供机会与全球社区进行协商，并为构建基础的其他人的投资。

有关分步指南，请参阅[应用运行时修补程序通过使用包支持框架 MSIX 包](https://docs.microsoft.com/windows/uwp/porting/package-support-framework)。

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>快速查看包支持框架内部

包支持框架包含一个可执行文件、 运行时管理器 DLL 和一组的运行时修补程序。

![包支持框架](images/package-support-framework.png)

它的工作原理如下。 将创建一个配置文件，指定你想要应用到你的应用程序的修补程序。 然后，将修改你的包，使其指向包支持框架 (PSF) 启动程序可执行文件。

当用户启动应用程序时，包支持框架启动器是运行的第一个可执行文件。 它读取配置文件并将应用程序进程中注入的运行时修补程序和运行时管理器 DLL。 运行时管理器适用于要在 MSIX 容器内运行的应用程序所需的修补程序。

![包支持 Framework DLL 注入](images/package-support-framework-2.png)

## <a name="how-to-use-the-package-support-framework"></a>如何使用包支持框架

为你的应用程序创建一个包后，安装和运行它，并观察其行为。 可能会收到错误消息可以帮助你确定兼容性问题。 此外可以使用[Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon)识别问题。

发现问题后，可以检查我们[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/)页会提供修补程序。 如果找到，可以将其应用到包。 我们[分步指南](https://docs.microsoft.com/windows/uwp/porting/package-support-framework)演示如何执行此操作。 它还将显示您如何使用 Visual Studio 调试器单步执行您的应用程序，并验证的修补程序正常工作以及它解决了兼容性问题。

如果找不到解决你遇到的问题的运行时修补程序，则可以创建一个。 若要执行操作，您要知道哪些函数调用将失败时 MSIX 容器中运行应用程序。 然后，可以创建你想要改为调用的运行时管理器的替代函数。 这使你有机会函数的实现替换为符合现代的运行时环境的规则的行为。

## <a name="get-started-with-the-package-support-framework"></a>开始使用包支持框架

如果已准备好开始使用包支持框架来解决兼容性问题，请参阅我们分步指南，网址[应用运行时修补程序通过使用包支持框架 MSIX 包](https://docs.microsoft.com/windows/uwp/porting/package-support-framework)。
