---
title: 相关应用安装程序文件文档
description: 本文提供了有关 Windows SDK 提供的应用安装程序文件架构和相关 Api 的文档的链接。
ms.date: 2/20/2019
ms.topic: article
keywords: windows 10，uwp，应用安装程序，AppInstaller，旁加载，API，XML，架构
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 72d2bd90a667d560749fb2641012b4307f84a139
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328249"
---
# <a name="related-app-installer-file-documentation"></a>相关应用安装程序文件文档

## <a name="app-installer-file-apis"></a>应用安装程序文件 Api

Windows SDK 中的[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)和[包](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package)类提供了可用于通过应用安装程序文件添加或修改包的方法，或用于检索应用程序安装程序关联的应用程序的相关信息。

|  方法  |  描述 | 支持的最低版本 |
|----------|--------------|-------------------|
|  [PackageManager. AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | 允许使用 appinstaller 文件安装单个或多个应用程序包。 | Windows 10 秋季创意者更新（版本1709，版本16299）   |
|  [PackageManager. RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | 允许使用 appinstaller 文件安装单个或多个应用程序包。 这会在安装应用程序包之前执行 SmartScreen 筛选器和用户验证。 | Windows 10 秋季创意者更新（版本1709，版本16299）       |
|  [GetAppInstallerInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | 返回 appinstaller xml 文件位置。 这样，应用程序开发人员便可以根据应用程序的需要检索 appinstaller xml 文件位置。 | Windows 10 版本1809（版本17763） |
|  [CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | 检查 appinstaller 文件中列出的主应用包的更新。 它允许开发人员确定是否需要更新，因为 appinstaller 策略。 此方法目前仅适用于通过 appinstaller 文件安装的应用程序。 | Windows 10 版本1809（版本17763） |

## <a name="app-installer-file-schema"></a>应用安装程序文件架构

有关如何手动设置应用程序安装程序文件格式的详细信息，请参阅[应用安装程序文件架构参考](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)。
