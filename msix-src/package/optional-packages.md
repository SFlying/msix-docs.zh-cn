---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 可选包和相关集创作
description: 可选包中包含可与主要包相集成的内容。 这对于可下载内容（DLC）和其他方案非常有用。
ms.date: 07/02/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10，.msix，uwp，可选包，相关集，包扩展，visual studio
ms.localizationpriority: medium
ms.openlocfilehash: c8b0dbc98e73e4086556fa6f169522ed5fcc37d9
ms.sourcegitcommit: f6bb9ced4cce853ae6acd3a359cbbb5e2e3f7187
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85441654"
---
# <a name="optional-packages-and-related-set-authoring"></a>可选包和相关集创作

可选包中包含可与主要包相集成的内容。 这些内容可用于可下载内容 (DLC)，因为大小限制而划分大型应用，或者用于随附从原始应用中单独分隔出来的任何其他内容。

相关集是可选包的扩展 - 它们使你能够跨主要包和可选包强制执行严格的一组版本。 如果相关集部署在应用商店外，则它们可能会有不同的发布者。

可选包和相关集全部在主应用的 .MSIX 容器中运行。

## <a name="prerequisites"></a>先决条件

- Visual Studio 2019 或 Visual Studio 2017 （版本15.1 或更高版本）
- Windows 10 版本 1703 或更高版本
- Windows 10 版本 1703 SDK 或更高版本

若要获取所有最新的开发工具，请参阅[适用于 Windows 10 的下载和工具](https://developer.microsoft.com/windows/downloads)。

> [!NOTE]
> 若要将使用可选包和/或相关集的应用提交到 Microsoft Store，你将需要权限。 可选包和相关集可用于业务线（LOB）或无合作伙伴中心权限的企业应用（如果未提交到商店）。 请参阅 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)，获取提交使用可选包和相关集的应用的权限。

## <a name="code-sample"></a>代码示例

在阅读本文时，建议你按照 GitHub 上的[可选包代码示例](https://github.com/AppInstaller/OptionalPackageSample)进行操作，通过亲自实践以了解可选包和相关集在 Visual Studio 内的工作原理。

## <a name="optional-packages"></a>可选包

若要在 Visual Studio 中创建可选包，你需要：
1. 请确保应用的**目标平台最小版本**设置为：10.0.15063.0 或更高版本。
2. 从**主要包**项目中打开 `Package.appxmanifest` 文件。 导航到“打包”选项卡，然后记下**包系列名称**，即“_”字符之前的所有内容。
3. 在**可选包**项目中，右键单击 `Package.appxmanifest`，然后选择**打开方式 > XML(文本)编辑器**。
4. 在文件中找到 `<Dependencies>` 元素。 添加以下，并将替换 `[MainPackageDependency]` 为步骤2中的**包系列名称**。 这将**可选包**指定为依赖于**主要包**。
    ```XML
    <uap3:MainPackageDependency Name="[MainPackageDependency]"/>
    ```

从步骤1到步骤4设置了包依赖关系之后，可以继续照常进行开发。 

每次部署可选包时，都可将 Visual Studio 配置为重新部署主要包。 若要设置 Visual Studio 中的生成依赖关系，应该：

1. 右键单击可选包项目，然后选择**生成依赖关系 > 项目依赖关系...**
2. 查看主要包项目，然后选择“确定”。 

现在，每次按 F5 或生成可选包项目时，Visual Studio 将首先生成主要包项目。 这将确保你的主要项目和可选项目保持同步状态。

## <a name="related-sets"></a>相关集<a name="related_sets"></a>

相关集由主包和可选包组成，后者通过在主包的 .appxbundle 或 .msixbundle 文件中指定的元数据紧密耦合。 此元数据将主包链接到可选包（使用 .appxbundle 文件 + 版本的名称），并将可选包链接到主包（使用独立于版本的名称）。 Visual Studio 可帮助你获取文件中的正确元数据。 

相关集中包的版本控制是以不允许使用任何包的最新版本的方式进行同步的，直到安装了所有相关集包（由主包中的版本指定）。 包是独立服务的，但在对其进行更新之前，可能不会使用在集中指定的包。

若要为相关集配置应用的解决方案，请使用以下步骤：

1. 右键单击主要包项目，然后选择**添加 > 新项目...**
2. 在窗口中，搜索扩展名为“.txt”的已安装模板，然后添加一个新的文本文件。
    > [!IMPORTANT]
    > 新文本文件必须命名为 `Bundle.Mapping.txt`。
3. 在 `Bundle.Mapping.txt` 文件中，你将指定任何可选包项目或外部包的相对路径。 示例 `Bundle.Mapping.txt` 文件应如下所示：
    ```syntax
    [OptionalProjects]
    "..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
    "..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

    [ExternalPackages]
    "..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
    ```

以这种方式配置解决方案时，Visual Studio 将为主包创建一个名为 AppxBundleManifest.xml 的[绑定清单](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest)，其中包含相关集的所有必需元数据。 

请注意，与可选包一样， `Bundle.Mapping.txt` 相关集的文件将仅适用于 Windows 10 版本1703或更高版本。 此外，应用的目标平台最低版本应设置为10.0.15063.0 或更高版本。

## <a name="removing-optional-packages"></a>删除可选包 
用户可以进入其 "**设置**" 应用，并删除可选包。 同样，开发人员可以使用[RemoveOptionalPackageAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.PackageCatalog)删除可选包的列表。 

```csharp
PackageCatalog catalog = PackageCatalog.OpenForCurrentPackage();
List<string> optionalList = new List<string>();
optionalList.Add("FabrikamAgeAnalysis_kwpnjs8c36mz0");
    
// Warn user that application will be restarted. 
var result = await catalog.RemoveOptionalPackagesAsync(optionalList);
if (result.ExtendedError != null)
{
    throw removalResult.ExtendedError;
}
```
> [!NOTE]
> 对于相关集，平台将需要重新启动主应用程序以完成删除操作，以避免应用包含从正在删除的包加载的内容的情况。 应用必须通知用户，应用程序在应用调用 API 之前需要重新启动。

如果可选包仅为内容，则开发人员应明确告知平台应用程序在删除可选包之前，应用程序不会将其删除。 这也允许开发人员在不重新启动的情况下删除包。

## <a name="known-issues"></a>已知问题<a name="known_issues"></a>

Visual Studio 目前不支持调试相关集可选项目。 若要解决此问题，可以部署和启动激活 (Ctrl + F5)，然后手动将调试程序附加到进程中。 若要附加调试程序，请转到 Visual Studio 中的“调试”菜单、选择“附加到进程...”，然后将调试程序附加到**主应用进程**。
