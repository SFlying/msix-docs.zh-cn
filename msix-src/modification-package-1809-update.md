---
author: dianmsft
title: Windows 10 1809 更新版中的 MSIX 修改包
description: 本部分介绍 Windows 10 1809 更新版中的修改包
ms.author: diahar
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 7273f3866eb8ed416b8dc60a621498cbddec54bd
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "65795423"
---
# <a name="msix-modification-packages-on-windows-10-version-1809"></a>Windows 10 版本 1809 中的 MSIX 修改包 

从 Windows 10 版本 1809 开始，可以在 [MSIX 修改包](modification-packages.md)中打包基于注册表的插件和自定义内容。 自定义内容包括需要在注册表中设置的，使主包能够按预期方式运行的配置。 这意味着，主应用程序将利用注册表来查看某个插件是否存在。 部署主包和修改包时，应用程序会在运行时查看主包和修改包的虚拟注册表 (VREG)。 

请注意，主包可能会使用 VREG 来执行以下操作： 
1.  查看插件 DLL 等文件的加载位置。 如果存在这种情况，请确保该文件是包的一部分。 这样，主包便可以在运行时访问该文件。  
2.  查看 VREG 项值的显示位置。 主包可能会检查某个值是否在 VREG 中存在。 手动或者使用此[工具](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf)创建修改包时，请确保该值正确。 

## <a name="create-a-modification-package-using-the-msix-packaging-tool"></a>使用 MSIX 打包工具创建修改包

可以使用 MSIX 打包工具创建修改包：
* 指定主包。 请确保主包的 MSIX 版本在转换的计算机上可用。 如果不可用，我们将要求你手动提供发布者和主应用程序信息。 此外，某些自定义内容要求在计算机上安装主应用程序。
![修改包 MPT](images/MPT-mod-page.png)

* 使用包编辑器完成转换后，请修改包。 可能存在这样一种情况：主包要求修改包在其 VREG 中包含特定的值。 在此情况下，需要相应地编辑该包。 

## <a name="create-a-modification-package-using-makeappxexe"></a>使用 MakeAppx.exe 创建修改包

可以使用 Windows 10 SDK 中包含的 [MakeAppX.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool) 工具手动创建修改包：
* 在清单中指定主包。 包含发布者和主包名称。

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```
- 创建 Registry.dat、User.dat 和 Userclass.dat，以创建加载修改包所需的任何注册表项。 仅当需要主应用程序查看自定义注册表项时，才需要执行此操作。 请记住，由于所有程序都在容器中运行，因此，在运行时，主包和修改包虚拟注册表将会合并，使主包可以查看修改包的虚拟注册表。  

此过程还支持文件系统插件和自定义，只要主应用程序的可执行文件不在虚拟文件系统 (VFS) 中即可。 这是为了确保主包可以获得主包和修改包的所有 VFS。 

我们已计划在下一主要版本的 Windows 中，推出当主应用程序的可执行文件位于 VFS 中时，对文件系统插件和自定义的支持。 从 Windows 10 Insider Preview 内部版本 18312 或更高版本开始即可预览此项支持。 有关详细信息，请参阅[此文章](modification-package-1903.md)。 

