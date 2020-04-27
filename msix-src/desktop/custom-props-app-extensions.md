---
title: 使用应用扩展的自定义属性
description: 了解如何使用 AppExtensions 的自定义属性
ms.date: 02/06/2020
ms.topic: article
keywords: windows 10, msix, uwp, 扩展
ms.localizationpriority: medium
ms.openlocfilehash: 126fa9b8f897e543d0418ea39d35cd088f0aeace
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77073873"
---
# <a name="using-custom-properties-for-app-extensions"></a>使用应用扩展的自定义属性

应用扩展属性是应用扩展开发人员在其应用清单中提供的自定义元数据字段。 应用扩展主机在检查应用扩展时可以读取此元数据，而无需从应用扩展加载任何内容。

属性是可选但非常有用的功能。 在创建应用扩展主机平台时，可能需要用到各种可能的元数据，例如版本、功能、支持的文件类型列表或其他数据，在加载应用扩展之前了解这些数据会很有帮助。 此类信息甚至可能确定应用扩展的加载方式。 应用扩展属性不是尝试预测可能需要的所有字段，而是提供一个打开的画布，用于以最适合应用的方式定义确切的需求。

## <a name="advantages-of-app-extension-properties"></a>应用扩展属性的优势

利用应用扩展属性的重要原因有两个：

* 这是存储有关应用扩展平台的基本或重要元数据的简单安全方式，这样就无需将此信息放到文件中。 可以在版本控制、权限和强制实施等重要概念中使用应用扩展属性。 例如，应用可以坚决要求定义 `version` 字段以及在属性中提供该字段，并根据该值提供不同的加载行为。

* 由于信息存储在应用清单中，因此可对其编制索引，以后可以通过 API 使用它。 这意味着，可以气泡的形式将它显示给用户，或者对特定的属性运行更细化的应用扩展搜索，而无需先部署并加载扩展。

## <a name="how-to-declare-properties"></a>如何声明属性

在包的 Package.appxmanifest 文件中声明属性。 在运行时，扩展主机可将其作为属性集读取。 由于主机定义平台，因此由主机负责向扩展开发人员传达有关哪些属性可用并且应放入 `AppExtension` 声明的信息。

> [!NOTE]
> Visual Studio 中的清单设计器不支持定义属性的功能。 必须直接编辑 Package.appxmanifest 才能定义属性。

若要声明属性，请将其放入 `<uap3:Properties/>` 声明下的 `<uap3:AppExtension>` 元素中。 下面是使用 Edge 支持的属性的 Microsoft Edge 示例 `<uap3:AppExtension>` 声明。

```xml
<uap3:AppExtension Name="com.microsoft.edge.extension" Id="FirstExtension" PublicFolder="Extension" DisplayName="MyExtension">
  <uap3:Properties>
    <Capabilities>
      <Capability Name="websiteContent" />
      <Capability Name="websiteInfo" />
      <Capability Name="browserWebRequest" />
      <Capability Name="browserStorage" />
    </Capabilities>
  </uap3:Properties>
</uap3:AppExtension>
```

Edge 已使用声明的扩展功能列表定义了 `Capabilities` 的已知属性值。 主机可以支持要放入应用扩展中的任何属性。 由于这是一个属性集，因此也可以使用嵌套属性。 最好是提供一个根版本属性，以便将来在更改扩展的格式时可以使用。 我们有意地未将 `Version` 设计为应用扩展的特性，以免人为地将你局限于使用我们的版本控制语义。 而是，在我们创建的属性中，版本可以是多个自定义特性之一，无论需要采用哪种处理方式和格式。

## <a name="how-to-use-properties"></a>如何使用属性

假设应用扩展中包含一个描述版本的简单属性，如下所示。

```xml
<uap3:Properties>
    <Version>1.0.0.0</Version>
</uap3:Properties>
```

若要在运行时获取此数据，只需对应用扩展调用 [GetExtensionPropertiesAsync()](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextension.getextensionpropertiesasync) 即可。

```csharp
string extensionVersion = "Unknown";
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;
if (properties != null)
{
    if (properties.ContainsKey("Version"))
    {
        PropertySet versionProperty = properties["Version"] as PropertySet;
        extensionVersion = versionProperty["#text"].ToString();
    }
}
```
