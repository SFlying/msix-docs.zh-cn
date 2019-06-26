---
author: dianmsft
title: Windows 10 版本 1903 中的 MSIX 修改包
description: 本部分介绍 Windows 10 1903 更新版中的修改包
ms.author: diahar
ms.date: 01/14/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: bca7001b4db8ac2e6ced6763d4eca7bb7c8d294d
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "65802373"
---
# <a name="msix-modification-packages-on-windows-10-version-1903"></a>Windows 10 版本 1903 中的 MSIX 修改包
 
我们在 Windows 10 版本 1809 中引入了 [MSIX 修改包](modification-packages.md)，使企业能够在 Windows 10 上自定义其应用。 在下一个 Windows 主要版本中，我们将添加更多的支持，使 IT 专业人员能够将自定义内容（例如基于文件的插件）打包成 MSIX 包。 

Windows 10 版本 1903 中添加了以下功能。

## <a name="manifest-update"></a>清单更新
我们在 MSIX 修改包的清单中添加了对以下元素的支持。

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

为了确保修改包可在 1903 或更高版本中正常运行，修改包的清单必须包含此元素。 如果使用 MSIX 打包工具一月版打包 MSIX 修改包，则系统会自动包含此元素。 如果使用更低版本的工具转换了某个包，可以在工具中编辑现有的包以添加此新元素。 此外，当用户安装修改包时，系统将发出警报，指出该包可能会修改主应用程序。

如果使用的修改包是由低于 1903 的版本创建的，则需要编辑包清单，以将 MaxVersionTested 属性更新为 10.0.18362.0。

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18362.0" />
```

## <a name="overriding-a-file-in-the-main-package"></a>替代主包中的文件
可以使用修改包替代主包中的文件。 这并不意味着更改主包的文件。 修改包不会更改该文件。 但是，在运行时，主包会同时看到自身的文件和修改包的文件，并且会选择修改包的文件进行加载。 

> [!NOTE]
> 修改包要替代的文件必须位于虚拟文件系统 (VSF) 文件夹中。 

## <a name="file-system-based-plug-in"></a>基于文件系统的插件
可将基于文件系统的插件打包为 MSIX 修改包。 如果主包通过查找某个文件夹来加载其插件，则你可以单独安装主包和修改包。 在运行时，该插件将会显示，因为主应用可以查询自身的文件夹和修改包的文件夹。 

> [!NOTE]
> 主应用程序用来加载插件的文件夹必须位于虚拟文件系统 (VFS) 文件夹中。  

## <a name="what-remains-the-same"></a>保持不变的功能
已转换为 MSIX 修改包的虚拟注册表插件在下一个  Windows 主要版本中仍受支持。 

