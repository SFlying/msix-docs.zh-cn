---
author: dianmsft
title: 在 Windows 10 1809年更新 MSIX 修改包
description: 在本部分中，我们将回顾在 Windows 10 1809 Update 修改包
ms.author: diahar
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 567417fb77ade5e048388a05ccc47651b32437dc
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900499"
---
# <a name="msix-modification-packages-on-windows-10-version-1809"></a>在 Windows 10 版本 1809 MSIX 修改包 

从 Windows 10 版本 1809年开始，你可以现在打包你的基于注册表的插件和自定义内的[MSIX 修改包](modification-packages.md)。 自定义可包含在注册表中设置，以便按预期方式将运行主包所需的配置。 这意味着主应用程序利用注册表，以查看是否存在一个插件。 在部署主包和修改包时，在运行时应用程序将查看主包和修改包的虚拟注册表 (VREG)。 

请注意，主包可能会使用 VREG 来执行以下操作： 
1.  查看加载的文件即插件的 dll 的位置。 如果这种情况，请确保该文件是包的一部分。 通过执行此操作，主包是可以访问在运行时文件。  
2.  查看在何处查看 VREG 密钥的值。 主包可能会寻找要 VREG 中存在的值。 当创建修改包通过手动或使用我们[工具](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf)，确保值为正确。 

## <a name="create-a-modification-package-using-the-msix-packaging-tool"></a>创建修改包使用 MSIX 打包工具

可以使用 MSIX 打包工具创建修改包：
* 指定主包。 请务必将转换在计算机上有主包可用的 MSIX 版本。 如果不是不是我们将要求您手动提供发布服务器和主应用程序的信息。 此外一些自定义要求在计算机上安装了主应用程序。
![修改包 MPT](images/MPT-mod-page.png)

* 一旦它发生了转换使用包编辑器修改包。 可能有一种情况主要包要求您修改的程序包以在其 VREG 中具有特定值的位置。 这是你将转和适当地编辑包。 

## <a name="create-a-modification-package-using-makeappxexe"></a>创建使用 MakeAppx.exe 修改包

您可以通过使用手动创建修改包[MakeAppX.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool) Windows 10 SDK 中包含的工具：
* 在清单中，指定主包。 包括在发布服务器和主包名称。

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```
- 创建 Registry.dat、 User.dat 和 Userclass.dat 创建加载修改包所需的任何注册表项。 这只是所需需要主应用程序若要查看自定义注册表项。 请记住，由于所有内容是容器内运行，在运行时主包和修改包虚拟注册表将合并此类主要该包可以查看修改包虚拟注册表。  

此过程还支持文件系统插件和自定义项，只要主应用程序的可执行文件不在虚拟文件系统 (VFS)。 这是为了确保主包将获得主要包和修改包的所有 VFS。 

对文件系统插件和自定义项在 VFS 主应用程序的可执行文件时的支持计划的下一主要版本的 Windows。 可以预览此开始在 Windows 10 Insider Preview 构建 18312 或更高版本的支持。 有关详细信息，请参阅[此文章](modification-package-insider-preview-build-18312.md)。 

