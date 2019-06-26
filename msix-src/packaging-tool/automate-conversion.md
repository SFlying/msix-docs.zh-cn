---
title: 将 Windows 安装程序自动转换为 MSIX 包
description: 使用命令行接口自动转换现有的 Windows 安装程序以生成 MSIX 包
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4408381a0ebbcc7fbdad7c517c1b64bbec2254da
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "58900169"
---
# <a name="automate-conversion-of-windows-installers-to-msix-packages"></a>将 Windows 安装程序自动转换为 MSIX 包

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">获取 MSIX 打包工具</a></p></div>

MSIX 打包工具支持使用命令行接口创建 MSIX 应用程序包，可让用户使用简单的 PowerShell 脚本自动重新打包和执行批量转换。

下面是一个使用示例转换模板的简单 PowerShell 脚本，它为文件夹路径中的每个安装程序创建相应的模板，并将模板传递给 MSIX 打包工具以创建 MSIX 包。


```ps1
$root = "C:\Installers\"
$ConversionTemplate = $root + "\SampleTemplate.xml"
$MSIXSaveLocation = "C:\MSIX\Converted"

get-childitem $root -recurse | where {$_.extension -eq ".msi"} | % {
  
    $Installerpath = $_.FullName
    $filename = $_.BaseName
    $XML_Path = $root + $filename + "ConversionTemplate.xml"

    [xml]$XmlDocument = Get-Content $ConversionTemplate
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Path = $Installerpath
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Arguments = "/qb"
    $XmlDocument.MSIXPackagingToolTemplate.Installer.InstallLocation = "C:\Program Files (x86)\"

    $XmlDocument.MSIXPackagingToolTemplate.SaveLocation.Path = $MSIXSaveLocation 
    
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherName = "CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" 
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ExecutableName = $filename +".exe"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.DisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.Description = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ID = $filename +"1"

    $xmldocument.Save($XML_Path)

    MsixPackagingTool.exe create-package --template $XML_Path

}
```

