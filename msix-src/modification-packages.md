---
title: 自定义企业应用的修改包
description: 了解如何自定义企业应用
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: a36c02d26e8c83ab43ceddd0704c78af68219649
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829373"
---
# <a name="customize-your-enterprise-apps-with-modification-packages"></a>自定义企业应用的修改包 

可以自定义应用程序的体验非常重要，尤其是适用于企业。 我们见到过的对 IT 专业人员，我们知道是至关重要的工作量将移动到 Windows 10 的自定义应用程序以满足其用户的需求。 在自定义应用程序，它们打包在使用 MSI，这很好理解 IT 专业人员必须获取从开发人员包和重新打包与自定义安装程序，以满足其需求。 这是一项成本高昂的企业工作。 展望未来，我们要分离的自定义和主应用程序，以便不再需要重新打包。 这确保企业获得最新的更新从开发人员同时仍保持其自定义项的控制。

在 Windows 10 版本 1809年我们引入了新类型的名为 MSIX 包*修改包*。 修改包是 MSIX 包存储自定义项。 修改包也可以是插件/加载-ons 可能没有的激活点。 IT 专业人员可以使用此功能，以便应用程序叠加的其企业自定义项功能，灵活地更改 MSIX 容器。 我们很高兴有关此功能并释放外观转发到将来引入的增强功能。 

## <a name="how-it-works"></a>工作原理

修改包专为企业拥有的应用程序的代码并且只让安装程序。 通过使用 MSIX 打包工具的最新版本 （适用于 Windows 10 1809年或更高版本)，可以创建修改包。 如果您的应用程序代码，或者可以创建[应用扩展](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-an-extension)。 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

如果你想要创建具有与主应用程序的严格绑定的修改包，您可以修改包清单中的依赖项声明主应用程序。 

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App"/>
</Dependencies>
```

下面的示例演示如何指定不同的证书或发布服务器。

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App" Publisher="CN=Contoso, C=US" />
</Dependencies>

```

如果修改包和主要包之间的关系是一对一，这是一个简单的配置。 典型的自定义通常需要 HKEY_CURRENT_USER 或 HKEY_CURRENT_USERCLASS 下的注册表项。 在我们 MSIX 包内部，我们有 User.dat 和 Userclass.dat 文件，以捕获的注册表项。 将需要创建 User.dat，如果您需要注册表项下 HKCU\Software\* (就像 Registry.dat 用于 HKLM\Software\*)。 如果您需要在 HKCU\Sofware\Classes 的密钥，请使用 Userclass.dat\*。 

下面是创建.dat 文件的典型方式： 

1.  使用注册表编辑器来创建一个文件。 在 Regedit 中创建配置单元，并插入所需的密钥。 不是右键单击，导出并保存-作为配置单元文件。 请务必将文件命名任一 User.dat 或 Userclass.dat

2.  使用 API 创建必要的文件。 我们已发布[API](https://msdn.microsoft.com/en-us/library/ee210773(v=vs.85).aspx)可用于保存.dat 文件。 请确保将文件经不起考验 User.dat 或 Userclass.dat

已做出必要的更改后，可以创建像任何其他 MSIX 包一样修改包。 然后您可以将与当前部署设置包的部署。 当您重新启动主应用程序时，您可以看到修改包所做的更改。 如果您选择删除修改包，在主要应用将恢复到而无需修改包的状态。 

## <a name="see-also"></a>请参阅
- [修改包在 Windows 10 版本 1809](modification-package-1809-update.md)
- [Windows 10 版本 1903年上修改包](modification-package-1903.md)
