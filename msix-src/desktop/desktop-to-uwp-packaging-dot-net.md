---
Description: 本指南介绍如何配置 Visual Studio 解决方案来编辑、 调试和打包桌面应用程序。
title: 打包桌面应用中使用 Visual Studio 的源代码
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 0cb6807c0aa126bbbdaf8e034f9fab489e49cffa
ms.sourcegitcommit: 6173086c11ffeb5fa836da6bd42711a9a626fc0e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66411406"
---
# <a name="package-a-desktop-app-from-source-code-using-visual-studio"></a>打包桌面应用中使用 Visual Studio 的源代码

可以使用**Windows 应用程序打包项目**Visual Studio 为桌面应用程序生成一个包中的项目。 然后，你可以发布的 Microsoft Store 或旁加载到其打包到一个或多台电脑上。

**Windows 应用程序打包项目**项目包含在以下版本的 Visual Studio。 为获得最佳体验，我们建议使用最新版本。

* Visual Studio 2019
* Visual Studio 2017 15.5年及更高版本

> [!IMPORTANT]
> **Windows 应用程序打包项目**在 Windows 10，版本 1607，及更高版本上支持在 Visual Studio 中的项目。 仅可在面向 Windows 10 周年更新 (10.0; 的项目Build 14393) 或更高版本。

## <a name="first-prepare-your-application"></a>首先，准备应用程序

在开始创建你的应用程序的包之前，请查看本指南：[准备将桌面应用程序打包](desktop-to-uwp-prepare.md)。

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>创建程序包

1. 在 Visual Studio 中，打开包含你的桌面应用程序项目的解决方案。

2. 向解决方案中添加一个 **Windows 应用程序包项目**项目。

   你无需向该项目中添加任何代码。 该项目只用于为你生成包。 我们将此项目称为“打包项目”。

   ![打包项目](images/packaging-project.png)

3. 将此项目的**目标版本**设置为你需要的任何版本，但请务必将**最低版本**设置为 **Windows 10 周年更新**。

   ![打包版本选择器对话框](images/packaging-version.png)

4. 在打包项目中，右键单击**应用程序**文件夹，然后选择**添加引用**。

   ![添加项目引用](images/add-project-reference.png)

5. 选择你的桌面应用程序项目，然后选择**确定**按钮。

   ![桌面项目](images/reference-project.png)

   你可以在程序包中包括多个桌面应用程序，但在用户选择应用磁贴时只能启动其中一个应用程序。 在**应用程序**节点中，右键单击你希望用户在选择应用磁贴时启动的应用程序，然后选择**设置为入口点**。

   ![设置入口点](images/entry-point-set.png)

6. 生成打包项目，以确保未显示任何错误。  如果收到错误，打开**Configuration Manager** ，并确保你的项目面向同一平台。

   ![配置管理器](images/config-manager.png)

7. 使用[创建应用包](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)向导生成 appxupload 文件。

   可以将该文件上载到应用商店直接。

**视频**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**运行、 调试或测试桌面应用程序**

请参阅[运行、 调试和测试打包桌面应用程序](desktop-to-uwp-debug.md)

**通过添加 UWP Api 增强您的桌面应用程序**

请参阅[增强用于 Windows 10 的桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**通过添加 UWP 项目和 Windows 运行时组件来扩展你的桌面应用程序**

请参阅[使用新式 UWP 组件扩展桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)。

**将应用分发**

请参阅[分发打包桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
