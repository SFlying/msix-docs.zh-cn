---
Description: 如果你在 Visual studio 中有打包项目，并且想要应用包支持框架
title: 在 Visual Studio 中应用包支持框架
ms.date: 05/14/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f006bb54348415259aae596d7fe5fe0328115394
ms.sourcegitcommit: 8d6bc53d5f5ae80d9ce191fe81660407e9f11e0e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83430409"
---
# <a name="apply-package-support-framework-in-visual-studio"></a>在 Visual Studio 中应用包支持框架
包支持框架（PSF）是一个[开源项目](https://github.com/Microsoft/MSIX-PackageSupportFramework/)，可用于将修补程序应用于现有的桌面应用程序。 PSF 使应用程序能够在不修改代码的情况下以 .MSIX 封装格式运行。 包支持框架可帮助应用程序遵循新式运行时环境的最佳做法。

在以下部分中，我们将探讨如何创建新的 Visual Studio 项目，如何在解决方案中包含包支持框架，以及如何创建运行时修复。

## <a name="step-1-create-a-package-solution-in-visual-studio"></a>步骤1：在 Visual Studio 中创建包解决方案
在 Visual Studio 中，创建一个新的**Visual Studio 解决方案 "空白解决方案"**。 将所有应用程序项目包括到新创建的**空白解决方案**。 

## <a name="step-2-add-a-packaging-project"></a>步骤2：添加打包项目

如果还没有**Windows 应用程序打包项目**，请创建一个项目并将其添加到解决方案中。 创建新的**Visual c # > Windows 通用 > Windows 应用程序打包项目**，并将其添加到新创建的解决方案中。

有关 Windows 应用程序打包项目的详细信息，请参阅[使用 Visual Studio 打包你的应用程序](../desktop/desktop-to-uwp-packaging-dot-net.md)。

在**解决方案资源管理器**中，右键单击打包项目，选择 "**编辑**"，然后将其添加到项目文件的底部：

```xml
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<Runtime fix project name>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

## <a name="step-3-add-project-for-the-runtime-fix"></a>步骤3：为运行时修复添加项目

向解决方案添加新的**Visual C++ > Windows 桌面 > 动态链接库（DLL）** 项目。

接下来，右键单击该项目，然后选择 "**属性**"。

在 "属性" 页中，找到 "**配置属性-> c/c + +-> 语言 > c + + 语言标准**" 字段。 然后从下拉菜单中选择 "ISO c + + 17 标准（/std： c + + 17）"。

右键单击该项目，然后在上下文菜单中，选择 "**管理 Nuget 包**" 选项。 确保 "**包源**" 选项设置为 "**全部**" 或 " **nuget.org**"。

单击 "设置" 图标旁边的字段。

在 Nuget 包中搜索**PSF**，然后安装此项目的**PackageSupportFramework** 。

![NuGet 包](images/psf-package.png)

## <a name="step-4-add-a-project-that-starts-the-psf-launcher-executable"></a>步骤4：添加启动 PSF 启动器可执行文件的项目
向解决方案中添加新的**Visual C++ > 常规 > 空项目**。

请执行以下步骤： 
1. 右键单击该项目，然后在上下文菜单中，选择 "**管理 Nuget 包**" 选项。 确保 "**包源**" 选项设置为 "**全部**" 或 " **nuget.org**"。
1. 单击 "设置" 图标旁边的字段。
1. 在 Nuget 包中搜索 PSF，然后安装此项目的 PackageSupportFramework。

打开项目的 "**属性" 页**，并在 "**常规**设置" 页中，将 "**目标名称**" 属性设置为 ``PSFLauncher32`` 或， ``PSFLauncher64`` 具体取决于应用程序的体系结构。

将项目引用添加到解决方案中的运行时修复项目。

右键单击该引用，然后在 "**属性**" 窗口中应用这些值。

| properties | Value |
|-------|-----------|
| 复制本地 | True |
| 复制本地附属程序集 | True |
| 引用程序集输出 | True |
| 链接库依赖项 | False |
| 链接库依赖项输入 | False |

## <a name="step-5-configure-the-packaging-project"></a>步骤5：配置打包项目

若要配置打包项目，请执行以下步骤： 
1. 在打包项目中，右键单击 "**应用程序**" 文件夹，然后从下拉菜单中选择 "**添加引用**"。
1. 选择 PSF 启动器项目和桌面应用程序项目，然后选择 **"确定"** 按钮。
1. 选择**PSF 启动器**和**桌面应用程序**项目，然后单击 "确定" 按钮。 如果应用程序源代码不可用，则仅选择 PSF 启动器项目。
1. 在 "**应用程序**" 节点中，右键单击 PSF 启动器应用程序，然后选择 "**设置为入口点**"。

向打包项目添加一个名为的文件 ``config.json`` ，然后将以下 json 文本复制并粘贴到该文件中。 将 "**包操作**" 属性设置为 "**内容**"。

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "fixups": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```

为每个键提供一个值。 使用此表作为指南。

| Array | 键 | Value |
|-------|-----------|-------|
| applications | id |  使用 `Id` `Application` 包清单中元素的属性的值。 |
| applications | 可执行文件 | 要启动的可执行文件的包相对路径。 在大多数情况下，你可以在修改之前从包清单文件中获取此值。 它是元素的属性的值 `Executable` `Application` 。 |
| applications | workingDirectory | 可有可无要用作启动的应用程序的工作目录的包相对路径。 如果未设置此值，操作系统将使用 `System32` 目录作为应用程序的工作目录。 |
| 进程 | 可执行文件 | 在大多数情况下，这将是上面配置的名称， `executable` 其中包含已删除的路径和文件扩展名。 |
| 修正 | dll | 要加载的链接地址 DLL 的包相对路径。 |
| 修正 | config | 可有可无控制修正 DLL 的行为方式。 此值的准确格式因修正链接而异，因为每个修正都可以根据需要解释此 "blob"。 |

完成后， ``config.json`` 文件将如下所示。

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`、 `processes` 和 `fixups` 键是数组。 这意味着，你可以使用配置 json 文件来指定多个应用程序、进程和修复 DLL。

### <a name="debug-a-runtime-fix"></a>调试运行时修补程序

在 Visual Studio 中，按 F5 启动调试器。  第一件事是 PSF 启动器应用程序，该应用程序随后会启动目标桌面应用程序。  若要调试目标桌面应用程序，必须通过选择 "**调试"->"附加到进程**"，然后选择应用程序进程，手动附加到桌面应用程序进程。 若要允许使用本机运行时修复 DLL 调试 .NET 应用程序，请选择 "托管和本机代码类型（混合模式调试）"。  

可以在桌面应用程序代码和运行时修复项目中的代码行旁边设置断点。 如果你的应用程序没有源代码，则只能在运行时修复项目中的代码行旁边设置断点。