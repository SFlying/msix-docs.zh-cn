---
description: 包支持框架可用于启动需要传递给它的参数的应用程序。
title: 包支持框架-启动具有参数的应用
ms.date: 03/30/2020
ms.topic: article
keywords: windows 10、uwp、.msix、psf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 42c672a0303b642d0ff1d23f0158fbdf9f4df517
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090695"
---
# <a name="launch-apps-with-parameters-through-package-support-framework"></a>通过包支持框架使用参数启动应用
包支持框架使用文件 config.js来配置应用程序的行为。

## <a name="proceedure"></a>过程
若要将应用程序执行配置为在应用程序启动期间接收传入的参数值，必须完成以下步骤。 

1. 下载包支持框架
1. 在) 上配置包支持框架 ( # B0
1. 将包支持框架文件注入到应用程序包
1. 更新应用程序的清单文件
1. 重新打包应用程序

## <a name="download-the-package-support-framework"></a>下载包支持框架
可以使用独立的 Nuget 命令行工具或通过 Visual Studio 检索包支持框架。

### <a name="nuget-comandline-tool"></a>Nuget comandline 工具：
从 nuget 网站上安装 Nuget 命令行工具： [https://www.nuget.org/downloads](https://www.nuget.org/downloads) 。 安装之后，在管理 PowerShell 窗口中运行以下命令行。

``` powershell
nuget install Microsoft.PackageSupportFramework
```

### <a name="visual-studio"></a>Visual Studio：
在 Visual Studio 中，右键单击解决方案/项目节点，并选择 "管理 Nuget 包" 命令之一。 搜索 " **PackageSupportFramework** " 或 " **PSF** " 以查找 nuget.org 上的包。然后，安装它。


## <a name="create-the-configjson-file"></a>在文件上创建 config.js

若要指定启动应用程序所需的参数，必须在指定应用程序可执行文件和参数的文件上修改 config.js。 在配置文件 config.js之前，请查看下表以了解 JSON strucuture。

### <a name="json-schema"></a>Json 架构

|Array          | key               | 值  |
|---------------|-------------------|--------|
| applications  | id                | 使用包清单中的应用程序元素的 ID 属性的值。|
| applications  | 可执行文件        | 要启动的可执行文件的包相对路径。 它是应用程序元素的可执行属性的值。 |
| applications  | 参数         | 可执行文件 (可选) 命令行参数参数。 |
| applications  | workingDirectory  |  (可选) 要用作启动的应用程序的工作目录的包相对路径。 如果未设置此值，操作系统将使用 System32 目录作为应用程序的工作目录。 如果以空字符串的形式提供值，则它将使用所引用的可执行文件的目录。 |
| applications  | 监视           |  (可选) 如果存在，则监视器将标识在启动主应用程序之前要启动的辅助程序。 |
| 进程     | 可执行文件        | 在大多数情况下，这将是上面配置的可执行文件的名称，其中包含已删除的路径和文件扩展名。 |
| 修正        | dll               | 要加载的链接地址的包相对路径，.msix/.appx。 |
| 修正        | config            |  (可选) 控制修正 dll 的行为方式。 此值的准确格式因修正链接而异，因为每个修正都可以根据需要解释此 "blob"。|

应用程序、进程和修正密钥是数组。 这意味着，可以使用文件 config.js来指定多个应用程序、进程和修复 DLL。

进程中的条目具有一个名为 "可执行文件" 的值，它的值应形成为正则表达式字符串，以便与无路径或文件扩展名的可执行进程的名称相匹配。 启动器应为 RegEx 字符串 Windows SDK std：： library RegEx ECMAList 语法。

``` json
{
    "applications": [
      {
        "id": "PSFExampleID",
        "executable": "VFS\\ThisPCDesktopFolder\\IG-Bentley.exe",
        "workingDirectory": "",
      }
    ],
}
```

## <a name="inject-the-package-support-framework-files-to-the-application-package"></a>将包支持框架文件注入到应用程序包

### <a name="create-the-package-staging-folder"></a>创建包暂存文件夹
如果已 .msix (或 .appx) 文件，则可以将其内容解压缩到一个布局文件夹中，该文件夹将用作包的暂存区域。 可以使用 Makeappx.exe 工具从命令提示符执行此操作。

可在默认位置找到 Makeappx.exe 工具：

| OS 体系结构 | Directory                                                   |
|-----------------|-------------------------------------------------------------|
| Windows 10 x86  | C:\Program Files\Windows Kits\10\bin arch> \[ **版本**] \x86\makeappx.exe       |
| Windows 10 x64  | C:\Program 文件 (x86) \Windows Kits\10\bin arch> \[ **版本**] \x64\makeappx.exe |

```powershell
makeappx unpack /p PrimaryApp.msix /d PackageContents
```

以上 PowerShell 命令会将应用程序的内容导出到本地目录中。

### <a name="inject-required-files"></a>插入所需文件
向包目录添加所需的32位和64位包支持框架 () 和可执行文件。 使用下表作为指南。 还需要根据需要包含任何运行时修补程序。 访问 [包支持框架运行时修复](./package-support-framework.md) 文档一文，了解有关使用包支持框架进行运行时修补程序的指南。

| 应用程序可执行文件为 x64 | 应用程序可执行文件为 x86     |
|-------------------------------|-----------------------------------|
| PSFLauncher64.exe             | PSFLauncher32.exe                 |
| PSFRuntime64.dll              | PSFLauncher32.dll                 |
| PSFRunDll64.exe               | PSFRunDll32.exe                   |

上述文件以及文件中的 **config.js** 必须位于应用程序包目录的根目录中。


## <a name="update-the-applications-manifest"></a>更新应用程序的清单
在文本编辑器中打开应用程序清单，然后将应用程序元素的可执行属性设置为 PSFLauncher 可执行文件的名称。 如果你知道目标应用程序的体系结构，请选择适当的版本 PSFLauncher [x64/x86] .exe。 如果未知，则 PSFLauncher32.exe 适用于所有情况。

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

## <a name="re-package-the-application"></a>重新打包应用程序

使用 makeappx.exe 工具重新打包应用程序。 请参阅以下示例，了解如何使用 PowerShell 打包和签署应用程序：

``` powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
signtool sign /a/v/fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```
