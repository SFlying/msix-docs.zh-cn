---
title: 利用修改包自定义企业应用
description: 本文介绍如何使用修改 .MSIX 包存储自定义项 tocustomize 企业应用程序。
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 21dc0d3f5100641253455f2af24ef19c0fa41270
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073117"
---
# <a name="customize-your-enterprise-apps-with-modification-packages"></a>利用修改包自定义企业应用

自定义应用程序体验的功能非常重要，尤其是对于企业而言。 我们已经说过了 IT 专业人员，我们知道，自定义应用程序以满足其用户需求对于迁移到 Windows 10 的工作至关重要。 自定义使用 MSI 打包的应用程序时，很好地了解 IT 专业人员必须从开发人员处获取包，并使用自定义重新打包安装程序以满足其需求。 这对于企业而言成本高昂。 接下来，我们想要将自定义项和主应用程序分离，以便不再需要重新打包。 这可确保企业从开发人员处获取最新更新，同时仍保持对其自定义项的控制。

在 Windows 10 版本1809中，我们引入了一种称为*修改包*的新类型的 .msix 包。 修改包是存储自定义项的 .MSIX 包。 修改包还可以是可能没有激活点的插件/加载项。 IT 专业人员可以使用此功能灵活地更改 .MSIX 容器，以便应用程序可以通过其企业的自定义进行重叠。

## <a name="how-it-works"></a>工作原理

修改包专为不拥有应用程序代码并且只有安装程序的企业而设计。 可以使用最新版本的 .MSIX 打包工具（适用于 Windows 10 版本1809或更高版本）创建修改包。 如果有应用程序的代码，也可以创建[应用扩展](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-an-extension)。 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

如果要创建具有到主应用严格绑定的修改包，可以将主应用声明为修改包清单中的依赖项。 

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App"/>
</Dependencies>
```

下面的示例演示如何指定不同的证书或发行者。

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App" Publisher="CN=Contoso, C=US" />
</Dependencies>

```

如果修改包和主程序包之间的关系是一对一的，则这是一种简单的配置。 典型的自定义通常需要 HKEY_CURRENT_USER 或 HKEY_CURRENT_USERCLASS 下的注册表项。 在我们的 .MSIX 包中，我们有用户 .dat 和 Userclass 文件来捕获注册表项。 如果需要 HKCU\Software 下的注册表项，需要创建用户 .dat\* （就像 Registry 一样使用 HKLM\Software\*）。 如果需要 Userclass\*下的密钥，请使用。 

下面是创建 .dat 文件的典型方法：

* 使用 Regedit 创建文件。 在 Regedit 中创建 hive 并插入所需的键。 右键单击、导出和另存为 hive 文件。 请确保将该文件命名为用户 .dat 或 Userclass

* 使用 API 创建必要的文件。 可以使用[ORSaveHive](https://docs.microsoft.com/windows/win32/devnotes/orsavehive)函数保存 .dat 文件。 请确保将该文件命名为网或 Userclass。

进行必要的更改后，可以像创建任何其他 .MSIX 包一样创建修改包。 然后，你可以通过当前的部署设置部署包。 重新启动主应用时，可以看到修改包所做的更改。 如果你选择删除修改包，则主应用将恢复为不带修改包的状态。 

## <a name="find-out-what-modification-packages-are-installed-on-your-device"></a>查明设备上安装了哪些修改包

使用 PowerShell，可以使用以下命令查看已安装的修改包。

```powershell
Get-AppPackage -PackageTypeFilter Optional
```

## <a name="modification-packages-on-windows-10-version-1809"></a>Windows 10 上的修改包版本1809

在 Windows 10 版本1809上，修改包可能包含需要在注册表中设置的配置，以便主包能够按预期运行。 也就是说，您的主应用程序利用注册表来查看插件是否存在。 部署主包和修改包时，应用程序会在运行时查看主包和修改包的虚拟注册表 (VREG)。

请注意，主包可能会使用 VREG 来执行以下操作：

* 查看加载插件文件（DLL）的位置。 如果存在这种情况，请确保该文件是包的一部分。 这样，主包便可以在运行时访问该文件。
* 查看 VREG 项值的显示位置。 主包可能会检查某个值是否在 VREG 中存在。 手动或者使用此[工具](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf)创建修改包时，请确保该值正确。

## <a name="modification-packages-on-windows-10-version-1903-and-later"></a>Windows 10 版本1903及更高版本上的修改包

Windows 10 版本 1903 中添加了以下功能。

### <a name="manifest-update"></a>清单更新

我们在 MSIX 修改包的清单中添加了对以下元素的支持。

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

为了确保修改包可在 1903 或更高版本中正常运行，修改包的清单必须包含此元素。 如果使用 MSIX 打包工具一月版打包 MSIX 修改包，则系统会自动包含此元素。 如果使用更低版本的工具转换了某个包，可以在工具中编辑现有的包以添加此新元素。 此外，当用户安装修改包时，系统将发出警报，指出该包可能会修改主应用程序。

如果使用的是在版本1903之前创建的修改包，则需要编辑包清单以将 `MaxVersionTested` 属性更新为10.0.18362.0。

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18362.0" />
```

### <a name="create-a-modification-package-using-the-msix-packaging-tool"></a>使用 MSIX 打包工具创建修改包

可以使用 MSIX 打包工具创建修改包：

* 指定主包。 请确保主包的 MSIX 版本在转换的计算机上可用。 如果不可用，我们将要求你手动提供发布者和主应用程序信息。 此外，某些自定义内容要求在计算机上安装主应用程序。
![修改包 MPT](images/MPT-mod-page.png)

* 使用包编辑器完成转换后，请修改包。 可能存在这样一种情况：主包要求修改包在其 VREG 中包含特定的值。 在此情况下，需要相应地编辑该包。

### <a name="create-a-modification-package-using-makeappxexe"></a>使用 MakeAppx.exe 创建修改包

可以使用 Windows 10 SDK 中包含的[makeappx.exe](package/create-app-package-with-makeappx-tool.md)工具手动创建修改包。

* 在清单中指定主包。 包含发布者和主包名称。

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```

* 创建 Registry.dat、User.dat 和 Userclass.dat，以创建加载修改包所需的任何注册表项。 仅当需要主应用程序查看自定义注册表项时，才需要执行此操作。 请记住，由于所有程序都在容器中运行，因此，在运行时，主包和修改包虚拟注册表将会合并，使主包可以查看修改包的虚拟注册表。  

此过程还支持文件系统插件和自定义，只要主应用程序的可执行文件不在虚拟文件系统 (VFS) 中即可。 这是为了确保主包可以获得主包和修改包的所有 VFS。

我们已计划在下一主要版本的 Windows 中，推出当主应用程序的可执行文件位于 VFS 中时，对文件系统插件和自定义的支持。 从 Windows 10 Insider Preview 内部版本 18312 或更高版本开始即可预览此项支持。 有关详细信息，请参阅[此文章](modification-package-1903.md)。 
