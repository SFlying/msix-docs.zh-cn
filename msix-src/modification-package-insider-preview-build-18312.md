---
author: dianmsft
title: 在 Windows 10 生成 18312 MSIX 修改包
description: 在本部分中，我们将回顾在 Windows 10 1903 Update 修改包
ms.author: diahar
ms.date: 01/14/2019
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.openlocfilehash: 5404b01de2841206669d73dc6f2fb14cabe93ef9
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900529"
---
# <a name="msix-modification-packages-on-windows-10-insider-preview-build-18312-and-later"></a>MSIX 修改包在 Windows 10 Insider Preview 构建 18312 及更高版本 
在 Windows 10 版本 1809年，我们引入了[MSIX 修改包](modification-packages.md)允许企业自定义其 Windows 10 上的应用。 在下一主要版本的 Windows，我们将添加更多的支持，可让 IT 专业人员和包的自定义，例如基于文件的插件到 MSIX 包。 

以下功能添加到的下一主要版本的 Windows，并可在 Windows 10 Insider Preview 构建 18312 中启动。

## <a name="manifest-update"></a>清单更新
我们为 MSIX 修改包清单中添加了对以下元素的支持。

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

若要确保修改包工作生成 18312 或更高版本中，修改包的清单必须包含此元素。 这会在为您如果打包 MSIX 修改包使用 MSIX 打包工具的年 1 月版。 如果已转换的包使用我们的工具发布之前，你可以编辑在我们的工具将此新元素添加现有包。 此外，如果用户安装修改包，它们会发出警报，包可能会修改主应用程序。

如果使用的生成 18132 之前创建的修改包，则需编辑包清单，以更新到 10.0.18132.0 MaxVersionTested 属性。

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18132.0" />
```

## <a name="overriding-a-file-in-the-main-package"></a>重写中主要包文件
您可以重写中修改包的主要包文件。 这并不意味着您要更改的主要包文件。 该文件保持不变的修改包。 但是，主包在运行时发现其文件和修改包的文件，并且它将选择修改要加载的包的文件。 

> [!NOTE]
> 修改程序包打算重写的文件必须在虚拟文件系统 (VSF) 文件夹中。 

## <a name="file-system-based-plug-in"></a>基于插件的文件系统
您可以打包基于插件以 MSIX 修改包的形式在文件系统。 如果主包加载其插件来看一下一个文件夹，可以单独安装主包和修改包。 在运行时，该插件会出现，因为主应用程序可以查询其文件夹和修改的文件夹。 

> [!NOTE]
> 主应用程序用来加载插件文件夹必须位于虚拟文件系统 (VFS) 文件夹中。  

## <a name="what-remains-the-same"></a>内容保持不变
虚拟注册表的管理单元已转换为 MSIX 修改包仍会支持 Windows 的下一个主版本中。 

