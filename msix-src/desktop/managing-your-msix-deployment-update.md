---
Description: 本文提供有关更新 MSIX 应用时可用的选项的详细信息。
title: MSIX 应用包的差异更新
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 893b67394b4ed996d4506a65b030205a1b8a9f3f
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77074113"
---
# <a name="differential-updates-for-msix-app-packages"></a>MSIX 应用包的差异更新

## <a name="understanding-msix-app-package-updates"></a>了解 MSIX 应用包更新
创建 MSIX 应用包时，将会生成一个清单文件，其中包含与 MSIX 应用包中所含文件相关的详细信息。 在创建包的过程中，将会创建元数据片段并将其存储在 .msix 或 .msixbundle 中，使 Windows 能够唯一标识包的组成部分。 以后在更新过程中，Windows 可以使用此元数据文件来比较旧包与新包，并确定需要将哪些内容下载到设备。 由于使用此元数据可以唯一标识包的组成部分，因此这意味着，差异更新机制完全可以在不同版本的包中正常运行（假设源包的版本低于目标包）。

一切都从 AppxBlockMap.xml 文件（前面所述的元数据）开始。 AppxBlockMap.xml 文件是一个 XML 文档，其中包含有关包中文件的信息的二维列表。 第一维列出有关文件的高级别详细信息（例如名称和大小），第二维提供该文件的每个 64KB 切片的 SHA2-256 哈希表示形式（也称作“块”）。

第一个哈希表示该文件的第一个 64KB 块，第二个哈希表示剩余的 35KB - 假设文件大小为 101188 字节。

在更新期间，如果该文件的第二个块已修改，则哈希也会更新以反映此事实。 下载组件将识别到此事实，只会提取第二个块，并重复使用旧包中第一个未经更改的块。

此外，如果整个文件未更改（根据整组不变化的块确定），则可以从现有包中重复使用该文件 - 从而为 Windows 10 用户带来极大的节省

### <a name="upgrading-to-newer-versions"></a>升级到较新版本
安装较新版本的 MSIX 应用包时，将会比较清单文件，并识别已修改的文件块。 由于 MSIX 应用包已升级到较新版本，因此只会检索已修改的文件。如果更新的应用程序驻留在网络共享中或者组织的外部，这可以减少带宽消耗。

### <a name="upgrading-to-older-versions"></a>升级到较旧版本
安装较旧版本的 MSIX 应用包时，将会比较清单文件，并识别已修改的文件块。 由于 MSIX 应用包已升级到较旧版本，因此只会检索已修改的文件。如果更新的应用程序驻留在网络共享中或者组织的外部，这可以减少带宽消耗。

## <a name="optimizing-upgrade-experiences"></a>优化升级体验
可对设备上的 MSIX 应用包传送或安装进行配置，以改善用户体验。 部署应用后，可将设备配置为在用户关闭该应用之后更新应用，或者强制关闭应用程序并强制更新应用。

### <a name="powershell"></a>PowerShell
使用 PowerShell 将 MSIX 应用包安装到设备的过程利用 [add-appxpackage](/powershell-msix-cmdlets.md) cmdlet。 此 cmdlet 包含以下参数，这些参数可以改变 MSIX 应用包安装或升级用户体验。

|||
|-|-|
| -DeferRegistrationWhenPackagesAreInUse | 指示当用户当前将应用保持打开状态时，此 cmdlet 会阻止 MSIX 应用包更新。 |
| -ForceApplicationShutdown | 指示此 cmdlet 强制关闭与包或其依赖项相关联的所有活动进程 |
| -ForceUpdateFromAnyVersion | 指示 MSIX 应用包将强制暂存/注册包的特定版本，而不管是否暂存/注册了更高的版本。 |
| -InstallAllResources | 指示此 cmdlet 强制部署 bundle 参数中指定的所有资源包。 这会替代部署引擎的资源适用性检查，并强制暂存所有资源包。 |
| -RetainFilesOnFailure | 在部署失败的情况下，如果此开关设置为 True，则不会删除安装期间在目标计算机上创建的文件。 |
| -Update | 指定所要添加的包是依赖项包更新。 删除父应用时，将删除依赖项包。 如果未指定，则删除父应用时不会删除该包。 |

有关此 cmdlet 的可用参数的完整列表，请访问有关 [add-appxpackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 的 PowerShell 文章。

