---
title: 手动创建应用安装程序文件
description: 本文介绍如何通过应用安装程序安装相关集，其中包括如何创建定义相关集的 *. appinstaller 文件。
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 2e7e3752fbdf7ba16a6cd6ba0b6fae3aee41eacd
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328296"
---
# <a name="create-an-app-installer-file-manually"></a>手动创建应用安装程序文件

本文介绍如何手动创建用于定义[相关集](install-related-set.md)的应用安装程序文件。 相关集不是一个实体，而是主要包和可选包的组合。 

为了能够将相关集作为一个实体来安装，我们必须能够将主要包和可选包指定为一个整体。 为此，我们需要创建一个具有**appinstaller**扩展名的 XML 文件来定义相关集。 应用安装程序使用**appinstaller**文件，并允许用户通过单击来安装所有已定义的包。 

## <a name="app-installer-file-example"></a>应用安装程序文件示例

在深入了解更多详细信息之前，下面是一个完整的示例 .msixbundle *. appinstaller 文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix"
            ProcessorArchitecture="x64" />

    </OptionalPackages>
    
    <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0"/>   
  </UpdateSettings>

</AppInstaller>
```

在部署期间，会根据 `Uri` 元素中引用的应用包来验证应用安装程序文件。 因此，`Name`、`Publisher` 和 `Version` 应与应用程序包清单中的 [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素匹配。

## <a name="how-to-create-an-app-installer-file"></a>如何创建应用安装程序文件

若要作为一个实体来分配相关集，你必须创建一个应用安装程序文件，其中包含该 [appinstaller 架构](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)所需的元素。

### <a name="step-1-create-the-appinstaller-file"></a>第 1 步：创建 *.appinstaller 文件
使用文本编辑器，创建一个文件（其中将包含 XML）并将其命名为 &lt;文件名&gt;.appinstaller

### <a name="step-2-add-the-basic-template"></a>第 2 步：添加基本模板
基本模板包括应用安装程序文件信息。
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>第 3 步：添加主要包信息

如果主要应用程序包是 .msix 或 .appx 文件，请使用 `<MainPackage>`，如下所示。 请确保包含 ProcessorArchitecture，因为它对非捆绑包是必需的。

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainPackage
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        ProcessorArchitecture="x64"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msix" />

</AppInstaller>
```

如果主应用包为 .msixbundle 或 .appxbundle 或文件，则使用 `<MainBundle>` 来代替 `<MainPackage>`，如下所示。 对于捆绑包，ProcessorArchitecture 不是必需的。

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

</AppInstaller>
```

`<MainBundle>` 或 `<MainPackage>` 属性中的信息应该分别与应用程序包清单或应用包清单中的 [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素匹配。

### <a name="step-4-add-the-optional-packages"></a>第 4 步：添加可选包
类似于主应用包属性，如果可选包可以是应用包或应用程序包，则 `<OptionalPackages>` 属性中的子元素应该分别是 `<Package>` 或 `<Bundle>`。 子元素中的包信息应该与程序包或程序包清单中的 Identity 元素相匹配。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x64"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>第 5 步：添加依赖项
在依赖项元素中，你可以指定主要包或可选包所需的框架包。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>第 6 步：添加更新设置

应用安装程序文件还可以指定更新设置，以便在发布较新的应用安装程序文件时可以自动更新相关集。 **<UpdateSettings>** 是一个可选元素。 在 **<UpdateSettings>** 中，OnLaunch 选项指定应在应用启动时进行更细你检查，HoursBetweenUpdateChecks="12" 则指定应每隔 12 小时执行更新检查。 如果未指定 HoursBetweenUpdateChecks，则用于检查更新的默认时间间隔为 24 小时。 其他类型的更新，如后台更新，可以在 "更新设置"[架构](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings)中找到;在 OnLaunch[架构](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-onlaunch)中可以找到其他类型的启动更新，如使用提示更新的更新。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

有关 XML 架构的所有详细信息，请参阅[应用安装程序文件参考](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)。

> [!NOTE]
> 应用安装程序文件类型是 Windows 10 版本1709（Windows 10 秋季创建者更新）中的新类型。 在以前版本的 Windows 10 上，不支持使用应用安装程序文件部署 Windows 10 应用。 从 Windows 10 版本1803开始， **HoursBetweenUpdateChecks**元素可用。
