---
title: 利用修改包自定义企业应用
description: 本文介绍如何使用修改 .MSIX 包存储自定义项 tocustomize 企业应用程序。
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 6db2a7aa009f0a4bce9a2aa8950e029473df63aa
ms.sourcegitcommit: 90eed7d23240aefa3761085955a193323f4661d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75831459"
---
# <a name="customize-your-enterprise-apps-with-modification-packages"></a>利用修改包自定义企业应用 

自定义应用程序体验的功能非常重要，尤其是对于企业而言。 我们已经说过了 IT 专业人员，我们知道，自定义应用程序以满足其用户需求对于迁移到 Windows 10 的工作至关重要。 自定义使用 MSI 打包的应用程序时，很好地了解 IT 专业人员必须从开发人员处获取包，并使用自定义重新打包安装程序以满足其需求。 这对于企业而言成本高昂。 接下来，我们想要将自定义项和主应用程序分离，以便不再需要重新打包。 这可确保企业从开发人员处获取最新更新，同时仍保持对其自定义项的控制。

在 Windows 10 版本1809中，我们引入了一种称为*修改包*的新类型的 .msix 包。 修改包是存储自定义项的 .MSIX 包。 修改包还可以是可能没有激活点的插件/加载项。 IT 专业人员可以使用此功能灵活地更改 .MSIX 容器，以便应用程序可以通过其企业的自定义进行重叠。 我们非常兴奋这项功能，期待在未来版本中引入增强功能。 

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

1. 使用 Regedit 创建文件。 在 Regedit 中创建 hive 并插入所需的键。 右键单击、导出和另存为 hive 文件。 请确保将该文件命名为用户 .dat 或 Userclass

2. 使用 API 创建必要的文件。 可以使用[ORSaveHive](https://docs.microsoft.com/windows/win32/devnotes/orsavehive)函数保存 .dat 文件。 请确保将该文件命名为网或 Userclass。

进行必要的更改后，可以像创建任何其他 .MSIX 包一样创建修改包。 然后，你可以通过当前的部署设置部署包。 重新启动主应用时，可以看到修改包所做的更改。 如果你选择删除修改包，则主应用将恢复为不带修改包的状态。 

## <a name="find-out-what-modification-packages-are-installed-on-your-device"></a>查明设备上安装了哪些修改包
使用 PowerShell，可以看到已安装的修改包，使用： AppPackage-PackageTypeFilter 可选

## <a name="see-also"></a>另请参阅
- [Windows 10 版本1809上的修改包](modification-package-1809-update.md)
- [Windows 10 版本1903上的修改包](modification-package-1903.md)
