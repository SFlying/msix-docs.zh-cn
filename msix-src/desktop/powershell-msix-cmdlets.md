---
Description: 本文提供了在企业环境中管理 MSIX 应用程序部署所需的所有详细信息。  本文的目标读者是企业和 IT 开发人员。
title: 通过 PowerShell 管理 MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix, PowerShell, PSH, PS, PoSh, cmdlet
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: b5c4366ac7e22fc67fb3314737c9f59f4a057aca
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074043"
---
# <a name="managing-msix-with-powershell"></a>通过 PowerShell 管理 MSIX
本文介绍了用来管理 .appx 和 .msix 程序包的 PowerShell cmdlet。

## <a name="msix-powershell-cmdlets"></a>MSIX PowerShell cmdlet

| PowerShell cmdlet | 说明 |
|-------------------|-------------|
| [Add-AppPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) | 此 cmdlet 用于将已签名的应用（*.msix、*.appx）程序包添加到设备。 添加与其他 MSIX 应用有关系的 MSIX 应用时，也可以使用 Add-AppPackage cmdlet，例如：外部程序包、[可选程序包](https://docs.microsoft.com/windows/msix/package/optional-packages)和[相关程序包](https://docs.microsoft.com/windows/msix/package/optional-packages)。 |
| [Remove-AppPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps) | 此 cmdlet 用于从设备中删除已签名的应用（*.msix、*.appx）程序包。 删除时，将删除已签名应用安装到的文件夹的内容，以及计算机上对已卸载应用程序的任何引用。 |
| [Get-AppPackage](https://docs.microsoft.com/powershell/module/appx/get-appxpackage?view=win10-ps) | 此 cmdlet 将提供计算机上已安装的所有已签名应用（*.msix、*.appx）程序包的列表。 可以提供一个值来筛选结果。 若要创建经过筛选的返回值，请在 **-Name** 参数中提供完整或部分字符串，使用 * 作为通配符。 |
| [Get-AppxDefaultVolume](https://docs.microsoft.com/powershell/module/appx/get-appxdefaultvolume?view=win10-ps) | 此 cmdlet 将提供计算机上的已签名应用（*.msix、*.appx）程序包使用的默认卷。 默认卷是计算机上所有部署或安装操作的目标。 不能从卷列表中删除此卷。 |
| [Get-AppPackageManifest](https://docs.microsoft.com/powershell/module/appx/get-appxpackagemanifest?view=win10-ps) | 此 cmdlet 将为指定的已签名应用完整程序包名称返回已签名应用（*. .msix、* .appx）程序包清单 xml 对象。 |
