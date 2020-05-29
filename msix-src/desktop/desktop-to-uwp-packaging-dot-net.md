---
Description: 本指南介绍了如何配置 Visual Studio 解决方案来编辑、调试和打包桌面应用。
title: 使用 Visual Studio 从源代码中将桌面应用打包
ms.date: 02/02/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: bd6cf9c3c00221fc929e395296eb527b3e3fc049
ms.sourcegitcommit: 7a52883434aa05272c15d033d85b67e2dd1e8c75
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2020
ms.locfileid: "84107375"
---
# <a name="set-up-your-desktop-application-for-msix-packaging-in-visual-studio"></a>在 Visual Studio 中设置用于 MSIX 打包的桌面应用程序

你可以使用 Visual Studio 中的 **Windows 应用程序打包项目**项目为桌面应用生成程序包。 然后，可以将你的程序包分发到 Microsoft Store、Web、你的企业或你所使用的任何其他分发机制中。

## <a name="required-visual-studio-version-and-workload"></a>必需的 Visual Studio 版本和工作负载

以下 Visual Studio 版本中提供了“Windows 应用程序打包项目”项目  ：

* Visual Studio 2019
* Visual Studio 2017 15.5 和更高版本

若要在“添加新项目”菜单中看到“Windows 应用程序打包项目”模板，需要确保至少安装了以下 Visual Studio 工作负载之一  ：

* “通用 Windows 平台开发”工作负载
* NET Core 工作负载中的可选组件“MSIX 打包工具”。
* .NET 桌面开发工作负载中的可选组件“MSIX 打包工具”。

 为了获得最佳体验，建议使用最新 Visual Studio 版本。

> [!IMPORTANT]
> Visual Studio 中的 **Windows 应用程序打包项目**项目在 Windows 10 版本 1607 和更高版本中受支持。 它只能用于面向 Windows 10 周年更新（10.0；内部版本 14393）或更高版本的项目中。

下面是你可以从 Visual Studio 应用程序打包项目中执行的一些其他操作：

:heavy_check_mark:自动生成可视资产。

:heavy_check_mark:使用可视设计器对清单进行更改。

:heavy_check_mark:使用向导生成程序包或捆绑包。

:heavy_check_mark:（如果发布到 Microsoft Store）基于你已在[合作伙伴中心](https://partner.microsoft.com/dashboard)中预留的名称轻松为应用程序分配标识。


## <a name="prepare-your-application"></a>准备应用程序

在开始为应用程序创建程序包之前请查看此指南：[准备打包桌面应用程序](desktop-to-uwp-prepare.md)。

<a id="new-packaging-project"/>

## <a name="setup-the-windows-application-packaging-project-in-your-solution"></a>在你的解决方案中设置 Windows 应用程序打包项目

1. 在 Visual Studio 中，打开包含你的桌面应用程序项目的解决方案。

2. 向解决方案中添加一个 **Windows 应用程序打包项目**项目。

   你无需向此项目中添加任何代码。 此项目只用于为你生成程序包。 我们将此项目称为“打包项目”。

   ![打包项目](images/packaging-project.png)

3. 将此项目的**目标版本**设置为你需要的任何版本，但请务必将**最低版本**设置为 **Windows 10 周年更新**。

   ![打包版本选择器对话框](images/packaging-version.png)

4. 在“解决方案资源管理器”中，右键单击打包项目下的“应用程序”文件夹，然后选择“添加引用”。  

   ![添加项目引用](images/add-project-reference.png)

5. 选择你的桌面应用程序项目，然后选择“确定”  按钮。

   ![桌面项目](images/reference-project.png)

   你可以在程序包中包括多个桌面应用程序，但在用户选择应用磁贴时只能启动其中一个应用程序。 在“应用程序”  节点中，右键单击你希望用户在选择应用磁贴时启动的应用程序，然后选择“设置为入口点”  。

   ![设置入口点](images/entry-point-set.png)

6. 生成打包项目，以确保未显示任何错误。 如果收到错误，请打开**配置管理器**并确保你的项目以同一平台为应用目标。

   ![配置管理器](images/config-manager.png)

7. 使用[创建应用程序包](../package/packaging-uwp-apps.md)向导生成 MSIX 程序包/捆绑包或 .msixupload/.appxupload 文件（用于发布到应用商店）。


## <a name="next-steps"></a>后续步骤

**在 Visual Studio 中打包桌面应用**

请参阅[在 Visual Studio 中打包桌面或 UWP 应用](../package/packaging-uwp-apps.md)

**运行、调试或测试桌面应用程序**

请参阅[运行、调试和测试打包的应用程序](desktop-to-uwp-debug.md)

## <a name="additional-resources"></a>其他资源

**视频**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

**通过添加 UWP API 来增强桌面应用程序**

请参阅[增强用于 Windows 10 的桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**通过添加 UWP 项目和 Windows 运行时组件来扩展你的桌面应用程序**

请参阅[使用新式 UWP 组件扩展桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)。

**分发你的应用**

请参阅[分发打包的桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
