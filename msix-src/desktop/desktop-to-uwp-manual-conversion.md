---
Description: 介绍如何手动打包适用于 Windows 10 的 Windows 桌面应用程序（例如 Win32、WPF 和 Windows 窗体）。
title: 手动打包应用程序（桌面桥）
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6799625432b42303b81ddcc7fcaacfb72e36f8fe
ms.sourcegitcommit: e703ffe4c635d9b127ecf8c02e087370b676aa9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108451"
---
# <a name="generating-msix-package-components"></a>生成 MSIX 打包组件

本文介绍如何使用命令行工具（不使用 Visual Studio 或 MSIX 打包工具）生成用于打包应用程序的 MSIX 打包组件。

若要手动打包应用，需要创建包清单文件，添加打包组件，然后运行 **MakeAppx.exe** 命令行工具来生成 MSIX 包。

## <a name="first-prepare-to-package"></a>首先准备打包

查看有关[在打包应用程序之前需要知道的事项](../desktop/before-packaging-overview.md)部分（如果尚未查看）。

## <a name="create-a-package-manifest"></a>创建包清单

创建文件，将其命名为 **appxmanifest.xml**，然后在其中添加以下 XML。

这是一个基本模板，其中包含包所需的元素和特性。 在下一部分，我们会在其中添加值。

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>填写文件的包级元素

在此模板中填写用于描述包的信息。

### <a name="identity-information"></a>标识信息

下面是一个带有特性占位符文本的示例 **Identity** 元素。 可将 ``ProcessorArchitecture`` 特性设置为 ``x64`` 或 ``x86``。

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> 如果已在 Microsoft Store 中保留了应用程序名称，可以使用[合作伙伴中心](https://partner.microsoft.com/dashboard)获取“名称”和“发布者”的值。 如果你打算将应用程序旁加载到其他系统上，只要选择的发布者名称与用于对应用进行签名的证书上的名称相匹配，就可以提供自己的名称。

### <a name="properties"></a>“属性”

[Properties](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) 元素包含 3 个必需的子元素。 下面是一个带有元素占位符文本的示例 **Properties** 节点。 **DisplayName** 是在 Store 中保留的应用程序名称，用于上传到 Store 的应用。

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>资源

下面是一个示例 [Resources](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources) 节点。

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>依赖关系

对于要为其创建包的桌面应用，请始终将 ``Name`` 特性设置为 ``Windows.Desktop``。

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>功能
对于要为其创建包的桌面应用，必须添加 ``runFullTrust`` 功能。

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>填写应用程序级元素

在此模板中填写用于描述应用的信息。

### <a name="application-element"></a>Application 元素

对于要为其创建包的桌面应用，Application 元素的 ``EntryPoint`` 特性始终是 ``Windows.FullTrustApplication``。

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>可视元素

下面是一个示例 [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) 节点。

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>（可选）添加基于目标的未着色资产

基于目标的资源是指显示在以下位置的图标和磁贴：Windows 任务栏、任务视图、ALT+TAB、贴靠助手和“开始”磁贴的右下角。 可在[此处](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos#unplated-assets)了解相关详细信息。

1. 获取正确的 44x44 图像，然后将它们复制到包含图像的文件夹（即 Assets）。

2. 对于每个 44x44 图像，在相同的文件夹中创建副本，并将 **.targetsize 44_altform unplated** 附加到文件名。 你应该有每个图标的两个副本，每个都以特定方式命名。 例如，完成该过程后，assets 文件夹可能包含 **MYAPP_44x44.png** 和 **MYAPP_44x44.targetsize-44_altform unplated.png**。

   > [!NOTE]
   > 在此示例中，**MYAPP_44x44.png** 是将在 MSIX 包的 ``Square44x44Logo`` 徽标特性中引用的图标。

3. 在清单文件中，对要设为透明的每个图标设置 ``BackgroundColor``。

4. 转到下一小节，以生成新的包资源索引文件。

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file-using-makepri"></a>使用 MakePri 生成包资源索引 (PRI) 文件

如果已按上面所述创建了基于目标的资源，或者在创建包后修改了应用程序的任何视觉资产，则必须生成新的 PRI 文件。

根据 SDK 的安装路径，下面是 **MakePri.exe** 在 Windows 10 电脑上的位置：
- x86：C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x86\makepri.exe
- x64：C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x64\makepri.exe

没有此工具的 ARM 版本。

1.  打开命令提示符或 PowerShell 窗口。

2.  将目录切换到包的根文件夹，然后运行命令 ``<path>\makepri.exe createconfig /cf priconfig.xml /dq en-US`` 创建 priconfig.xml 文件。

5.  使用命令 ``<path>\makepri.exe new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml`` 创建 resources.pri 文件。

    例如，适用于你的应用程序的命令可能如下所示：``<path>\makepri.exe new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``。

6.  根据下一步骤中的说明打包应用程序。

<a id="make-appx" />

## <a name="test-your-application-before-packaging"></a>在打包之前测试应用程序

可以部署未打包的应用程序，并在打包或签名之前对其进行测试。 为此，请在 PowerShell 窗口中运行以下 cmdlet。 确保传入位于包的根目录中的应用程序清单文件，以及其他所有包组件：

```Add-AppxPackage –Register AppxManifest.xml```

完成此操作后， 应用应会部署到系统中，可对其进行测试，以确保在打包之前一切正常。 若要更新应用的 .exe 或 .dll 文件，请将包中的现有文件替换为新文件，递增 AppxManifest.xml 中的版本号，然后再次运行上述命令。

## <a name="package-your-components-into-an-msix"></a>将组件打包到 MSIX

下一步是使用 **MakeAppx.exe** 为应用程序生成 MSIX 包。 Makeappx.exe 随附在 Windows 10 SDK 中，如果你已安装 Visual Studio，则可通过 Visual Studio 的开发人员命令提示符轻松访问它。

请参阅[使用 MakeAppx.exe 工具创建 MSIX 包或捆绑包](../package/create-app-package-with-makeappx-tool.md)



> [!NOTE]
> 打包的应用程序始终以交互用户的身份运行，任何安装已打包应用程序的驱动器必须以 NTFS 格式化。


