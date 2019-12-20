---
Description: 本指南介绍如何配置 Visual Studio 解决方案，以编辑、调试和打包桌面应用程序。
title: 使用 Visual Studio 从源代码中将桌面应用打包
ms.date: 07/18/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: cc4a113c53b9c061764d2cdbcc32c105c51de3fa
ms.sourcegitcommit: 0412ba69187ce791c16313d0109a5d896141d44c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303146"
---
# <a name="package-a-desktop-app-from-source-code-using-visual-studio"></a>使用 Visual Studio 从源代码中将桌面应用打包

您可以使用 Visual Studio 中的 " **Windows 应用程序打包项目**" 项目为桌面应用程序生成包。 然后，你可以将该包发布到 Microsoft Store 或将其旁加载到一个或多个电脑上。

**Windows 应用程序打包项目**项目在以下版本的 Visual Studio 中提供。 为了获得最佳体验，建议使用最新版本。

* Visual Studio 2019
* Visual Studio 2017 15.5 及更高版本

> [!IMPORTANT]
> Windows 10 版本1607及更高版本支持 Visual Studio 中的**Windows 应用程序打包项目**项目。 它只能用于面向 Windows 10 周年更新的项目（10.0;版本14393）或更高版本。

## <a name="prepare-your-application"></a>准备应用程序

开始为应用程序创建包之前，请查看本指南：[准备打包桌面应用程序](desktop-to-uwp-prepare.md)。

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>创建程序包

1. 在 Visual Studio 中，打开包含你的桌面应用程序项目的解决方案。

2. 向解决方案中添加一个 **Windows 应用程序包项目**项目。

   你无需向该项目中添加任何代码。 该项目只用于为你生成包。 我们将此项目称为“打包项目”。

   ![打包项目](images/packaging-project.png)

3. 将此项目的**目标版本**设置为你需要的任何版本，但请务必将**最低版本**设置为 **Windows 10 周年更新**。

   ![打包版本选择器对话框](images/packaging-version.png)

4. 在解决方案资源管理器中，右键单击打包项目下的 "**应用程序**" 文件夹，然后选择 "**添加引用**"。

   ![添加项目引用](images/add-project-reference.png)

5. 选择你的桌面应用程序项目，然后选择**确定**按钮。

   ![桌面项目](images/reference-project.png)

   你可以在程序包中包括多个桌面应用程序，但在用户选择应用磁贴时只能启动其中一个应用程序。 在**应用程序**节点中，右键单击你希望用户在选择应用磁贴时启动的应用程序，然后选择**设置为入口点**。

   ![设置入口点](images/entry-point-set.png)

6. 如果打包的应用程序面向 .NET Core 3，请按照以下步骤将新的生成目标添加到项目文件。 这仅适用于面向 .NET Core 3 的应用程序。  

    1. 在解决方案资源管理器中，右键单击打包项目节点，然后选择 "**编辑项目文件**"。

    2. 在文件中找到 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 元素。

    3. 将此元素替换为以下 XML。

        ``` xml
        <ItemGroup>
          <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
          </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
          <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
              <SourceProject>
              </SourceProject>
            </_FilteredNonWapProjProjectOutput>
          </ItemGroup>
        </Target>
        ```

    4. 保存并关闭项目文件。

7. 生成打包项目，以确保未显示任何错误。 如果收到错误，请打开**Configuration Manager**并确保项目面向同一平台。

   ![配置管理器](images/config-manager.png)

8. 使用 "[创建应用包](../package/packaging-uwp-apps.md)" 向导来生成 msixupload/.appxupload 文件。

   可以直接将该文件上传到存储区。

**视频**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**运行、调试或测试桌面应用程序**

请参阅[运行、调试和测试打包的桌面应用程序](desktop-to-uwp-debug.md)

**通过添加 UWP Api 增强桌面应用程序**

请参阅[增强用于 Windows 10 的桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**通过添加 UWP 项目和 Windows 运行时组件来扩展桌面应用程序**

请参阅[使用新式 UWP 组件扩展桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)。

**分发应用**

请参阅[分发打包的桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
