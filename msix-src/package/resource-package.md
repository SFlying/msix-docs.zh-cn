---
title: 资源包
description: 本文介绍资源包以及开发人员如何在代码中使用它们。
ms.date: 02/06/2020
author: dianmsft
ms.topic: article
ms.author: diahar
keywords: windows 10，.msix，uwp，可选包，相关集，包扩展，visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 1900e748075339ad6843f13a6c145147422e2db0
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073763"
---
# <a name="resource-packages"></a>资源包 
资源包提供了一种很好的方法，可通过将语言分段或将特定的资产划分为 Windows 自动下载的单独包（具体取决于用户计算机配置）来减少用户的磁盘占用量。 如果用户在**区域 & 语言**设置中向其 OS 语言列表添加了一种新语言，或在自动存储更新中更改了显示配置，则操作系统将为设备上所有已安装的应用提取适用的资源包。

在 Windows SDK 10.0.17095.0 [ADDRESOURCEPACKAGEASYNC API](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.PackageCatalog)中，开发人员可根据需要为应用安装资源包。 


## <a name="how-to-use-the-addresourcepackageasync-api"></a>如何使用 AddResourcePackageAsync API 
- AddResourcePackageAsync 采用应用程序的**PackageFamilyName**和唯一标识尝试下载的资源包的资源 ID。 资源 ID 对应于资源包的**appxmanifest.xml**中的**ResourceId**元素。

- 应用程序必须重新启动才能获取可用于应用程序的所有资源的合并视图。 该 API 提供了**ForceTargetApplicationShutdown**选项，该选项可以通过来重新启动应用程序。

- 如果在下载资源包时有可用于应用程序的更新，则 API 将失败，因为旧版本的应用程序不再可用。 传入**ApplyUpdateIfAvailable**标志后，API 将更新应用，并将请求的资源包作为相同下载的一部分获取。 

- API 将返回可在应用程序中显示的下载进度。 另请注意，对于发布到应用商店的应用，资源包获取会显示在 Microsoft Store**下载和更新**"页中，并显示为应用程序的更新。 最终用户可以选择暂停或取消从 Microsoft Store 下载资源包。 

示例 
```
 private async void DownloadGermanResourcePackage(object sender, RoutedEventArgs e)
{            
    // ResourceId that is unique to the resource package
    string resourceId = "split.language-de";
    // Warn user that application will need to restart.
    // To take update if available pass in the ApplyUpdateIfAvailable flag
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    PackageCatalogAddResourcePackageResult result = await packageCatalog.AddResourcePackageAsync("29270depappf.CaffeMacchiato_gah1vdar1nn7a", resourceId, 
        AddResourcePackageOptions.ApplyUpdateIfAvailable | AddResourcePackageOptions.ForceTargetApplicationShutdown)
        .AsTask<PackageCatalogAddResourcePackageResult, PackageInstallProgress>(new Progress
        (progress =>;
        {
                // Draw progress
        }));

        if (result.ExtendedError != null)
        {
                //Display error or retry if needed
        }
}
```
## <a name="validation"></a>验证
 对于本地验证，开发人员可以创建 .msixbundle 或 .appxbundle，并从本地驱动器、网络共享或 web 服务器进行安装。 当应用程序调用 AddResourcePackageAsync API 时，Windows 将从安装原始应用程序的位置获取资源包。 这使得验证变得简单。 完成此操作后，便可以部署应用了。 

## <a name="removing-resource-packages"></a>删除资源包 
应用可以选择删除，删除通过[REMOVERESOURCEPACKAGEASYNC API](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.PackageCatalog)下载的资源包。 但是，不能卸载适用于用户或设备的资源包。 

示例 
```
 private async void RemoveResourcePackage(object sender, RoutedEventArgs e)
{            
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    List resourcePackagesToRemove = new List();
    resourcePackagesToRemove.Add("split.language-de");
    //Warn user that application will be restarted
    var removePackageResult = await packageCatalog.RemoveResourcePackagesAsync(resourcePackagesToRemove);
    if (removePackageResult.ExtendedError != null)
    {
        // display error
    }
}
```
