---
title: 应用安装程序文件的相关的文档
description: 列出有关应用安装程序文件架构和 Api 的信息。
ms.date: 2/20/2019
ms.topic: article
keywords: windows 10、 uwp、 应用程序安装程序中，应用安装旁, 加载，API，XML，架构
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 48ca61da7e4e568e9ae9381c353a658be6d80ae9
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828655"
---
# <a name="related-app-installer-file-documentation"></a>应用安装程序文件的相关的文档

## <a name="app-installer-file-apis"></a>应用安装程序文件 Api

[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)并[包](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package)Windows SDK 中的类提供的方法，可以使用添加或修改包通过应用安装程序文件，或检索有关与应用程序安装程序的应用的信息关联。

|  方法  |  描述 | 支持的最低版本 |
|----------|--------------|-------------------|
|  [PackageManager.AddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | 允许使用.appinstaller 文件安装的单个或多个应用程序包。 | Windows 10 Fall Creators Update （版本 1709年，内部版本 16299）   |
|  [PackageManager.RequestAddPackageByAppInstallerFileAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | 允许使用.appinstaller 文件安装的单个或多个应用程序包。 这将执行 SmartScreen 筛选器和用户验证之前安装的应用程序包。 | Windows 10 Fall Creators Update （版本 1709年，内部版本 16299）       |
|  [Package.GetAppInstallerInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | 返回.appinstaller xml 文件位置。 这允许应用开发人员可检索.appinstaller xml 文件位置时所需的应用程序。 | Windows 10，版本 1809年 （内部 17763） |
|  [Package.CheckUpdateAvailabilityAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | 检查.appinstaller 文件中列出的主应用包更新。 它允许开发人员，以确定是否由于.appinstaller 策略需要安装更新。 此方法目前仅适用于通过.appinstaller 文件安装的应用程序。 | Windows 10，版本 1809年 （内部 17763） |

## <a name="app-installer-file-schema"></a>应用安装程序文件架构

有关如何手动设置格式的应用安装程序文件的详细信息，请参阅[应用程序安装程序文件架构参考](https://docs.microsoft.com/en-us/uwp/schemas/appinstallerschema/app-installer-file)。
