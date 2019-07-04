---
Description: 演示如何手动打包用于 Windows 10 的 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）。
title: 手动打包应用程序 （桌面桥）
ms.date: 05/18/2018
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, uwp, msix
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0c3bf9cec02d2db18fba3d6246a740e7d62a401c
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555521"
---
# <a name="package-a-desktop-application-manually"></a>手动打包桌面应用程序

本主题演示如何打包应用程序，而无需使用 Visual Studio 或 Desktop App Converter (DAC) 等工具。

若要手动打包应用，请创建程序包清单文件，然后运行命令行工具生成 Windows 应用包。

如果使用 xcopy 命令，安装应用程序或您熟悉您的应用程序的安装程序会对系统的更改，请考虑手动打包，并且希望更精细地控制该过程。

如果不确定安装程序会对系统进行哪些更改，或如果更希望使用自动化工具来生成程序包清单，请考虑任一[这些](desktop-to-uwp-root.md#convert)选项。

> [!IMPORTANT]
> 在 Windows 10，版本 1607，引入的功能来创建 Windows 应用程序包为桌面应用程序 （也称为桌面桥） 且不能仅用在面向 Windows 10 周年更新 (10.0; 项目Build 14393) 或更高版本在 Visual Studio 中的。

## <a name="first-prepare-your-application"></a>首先，准备应用程序

在开始创建你的应用程序的包之前，请查看本指南：[准备将桌面应用程序打包](desktop-to-uwp-prepare.md)。

## <a name="create-a-package-manifest"></a>创建程序包清单

创建文件，将其命名为 **appxmanifest.xml**，然后向其添加此 XML。

这是一个基本模板，其中包含程序包需要的元素和特性。 我们将在下一步部分中对它们添加值。

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

## <a name="fill-in-the-package-level-elements-of-your-file"></a>填写文件的程序包级别元素

在此模板中填写描述程序包的信息。

### <a name="identity-information"></a>标识信息

下面是一个带特性占位符文本的示例 **Identity** 元素。 可将 ``ProcessorArchitecture`` 特性设置为 ``x64`` 或 ``x86``。

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> 如果你已保留在 Microsoft Store 应用程序名称，则可以通过使用获取的名称和发布服务器[合作伙伴中心](https://partner.microsoft.com/dashboard)。 如果打算旁加载应用程序转移到其他系统，您可以自己项目的名称，前提是用于登录您的应用程序选择与证书上的名称相匹配的发布服务器名称。

### <a name="properties"></a>properties

[属性](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) 元素具有 3 个所需子元素。 下面是一个带元素占位符文本的示例**属性**节点。 **DisplayName**是保留在存储中，上载到应用商店的应用的应用程序的名称。

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>资源

下面是一个示例[资源](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources)节点。

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>依存关系

对于桌面应用程序创建的包，始终设置``Name``属性为``Windows.Desktop``。

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>功能
对于桌面应用程序创建包，您需要添加``runFullTrust``功能。

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>填写应用程序级别元素

在此模板中填写描述应用的信息。

### <a name="application-element"></a>应用程序元素

对于桌面应用程序创建的包``EntryPoint``应用程序元素的属性始终是``Windows.FullTrustApplication``。

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

1. 获取正确的 44x44 图像，然后将它们复制到包含图像（即资产）的文件夹。

2. 对于每个 44x44 图像，在相同的文件夹中创建副本，并将 **.targetsize 44_altform unplated** 附加到文件名。 你应该有每个图标的两个副本，每个都以特定方式命名。 例如，完成该过程后，资产文件夹可能包含 **MYAPP_44x44.png** 和 **MYAPP_44x44.targetsize-44_altform unplated.png**。

   > [!NOTE]
   > 在此示例中，**MYAPP_44x44.png** 就是将在 Windows 应用包的 ``Square44x44Logo`` 徽标特性中引用的图标。

3.  在 Windows 应用包中，将制作的每个图标的 ``BackgroundColor`` 设置为透明。

4. 继续下一小节，以生成新的包资源索引文件。

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>生成包资源索引 (PRI) 文件

如果上面的部分中所述创建基于目标的资产或你创建包后，会修改任何应用程序的视觉资产，你必须生成新的 PRI 文件。

1.  打开**适用于 VS 2017 的开发人员命令提示符**。

    ![开发人员命令提示符](images/developer-command-prompt.png)

2.  将目录更改为程序包的根文件夹，然后通过运行命令 ``makepri createconfig /cf priconfig.xml /dq en-US`` 创建 priconfig.xml 文件。

5.  使用命令 ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml`` 创建 resources.pri 文件。

    例如，适用于你的应用程序的命令可能类似以下形式： ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``。

6.  使用下一步中的说明打包 Windows 应用包。

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>生成 Windows 应用包

使用 **MakeAppx.exe** 为项目生成 Windows 应用包。 它随 Windows 10 SDK 提供，如果你已安装 Visual Studio，则可通过用于 Visual Studio 版本的开发人员命令提示符轻松访问它。

请参阅[使用 MakeAppx.exe 工具创建应用包](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

## <a name="run-the-packaged-app"></a>运行打包的应用

你可以运行你的应用程序进行测试本地而无需获取证书并对其进行签名。 只需运行此 PowerShell cmdlet：

```Add-AppxPackage –Register AppxManifest.xml```

若要更新应用的 .exe 或 .dll 文件，请将程序包中的现有文件替换为新文件、增加 AppxManifest.xml 中的版本号，然后再次运行上述命令。

> [!NOTE]
> 打包的应用程序始终运行以交互式的用户，并安装到打包的应用程序的任何驱动器必须格式化为 NTFS 格式。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)向我们提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**单步执行代码 / 查找并修复问题**

请参阅[运行、 调试和测试打包桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-debug)

**应用程序进行签名，然后将其分发**

请参阅[分发打包桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)