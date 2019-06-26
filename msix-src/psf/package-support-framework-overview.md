---
author: mcleanbyron
Description: 包支持框架可帮助解决阻止桌面应用程序在 MSIX 容器中运行的问题。
title: 包支持框架
ms.author: mcleans
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f009690a704347e70db716b5491a873a820a0434
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "65985917"
---
# <a name="package-support-framework"></a>包支持框架

包支持框架是一个开源工具包，在你无法访问源代码时，可帮助你将修复程序应用到现有的 win32 应用程序，使它可以在 MSIX 容器中运行。 包支持框架可帮助应用程序遵循新式运行时环境的最佳做法。

若要创建包支持框架，我们利用了 [Detour](https://www.microsoft.com/en-us/research/project/detours) 技术，这是 Microsoft Research (MSR) 开发的一个开源框架，可帮助实现 API 重定向和挂接。

此轻型框架是开源的，可以使用它来快速解决应用程序问题。 在其中还可以咨询全球各地的社区，并基于其他产品的投资构建解决方案。

有关分步指南，请参阅[使用包支持框架将运行时修复程序应用到 MSIX 包](https://docs.microsoft.com/windows/uwp/porting/package-support-framework)。

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>包支持框架内部速览

包支持框架包含一个可执行文件、一个运行时管理器 DLL 和一组运行时修复程序。

![包支持框架](images/package-support-framework.png)

它的工作原理如下。 创建一个配置文件用于指定要应用到应用程序的修复程序。 然后，将包修改为指向包支持框架 (PSF) 启动器可执行文件。

当用户启动该应用程序时，包支持框架启动器是运行的第一个可执行文件。 该启动器将读取配置文件，并将运行时修复程序和运行时管理器 DLL 注入应用程序进程。 如果应用程序需要使用该修复程序才能在 MSIX 容器中运行，则运行时管理器会应用该修复程序。

![包支持框架 DLL 注入](images/package-support-framework-2.png)

## <a name="how-to-use-the-package-support-framework"></a>如何使用包支持框架

为应用程序创建包后，请安装并运行它，同时观察其行为。 收到的错误消息可能会帮助你识别兼容性问题。 也可以使用[进程监视器](https://docs.microsoft.com/sysinternals/downloads/procmon)来识别问题。

发现问题后，可以检查 [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/) 页中是否提供了修复程序。 如果找到了修复程序，可将其应用到包。 我们的[分步指南](https://docs.microsoft.com/windows/uwp/porting/package-support-framework)介绍了如何执行此操作。 其中还介绍了如何使用 Visual Studio 调试器逐步调试应用程序，验证该修复程序是否起到了作用并可以解决兼容性问题。

如果找不到可以解决问题的运行时修复程序，可以创建一个。 为此，需要识别当应用程序在 MSIX 容器中运行时哪些函数调用失败。 然后可以创建替代的函数，让运行时管理器改为调用这些函数。 这样，便可以使用符合新式运行时环境规则的行为来替代函数的实现。

## <a name="get-started-with-the-package-support-framework"></a>开始使用包支持框架

如果你已准备好开始使用包支持框架来解决兼容性问题，请参阅我们的分步指南：[使用包支持框架将运行时修复程序应用到 MSIX 包](https://docs.microsoft.com/windows/uwp/porting/package-support-framework)。
