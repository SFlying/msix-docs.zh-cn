---
title: 组策略如何与 MSIX 打包的应用配合工作
description: 介绍组策略如何与转换成 MSIX 的应用配合工作。
ms.date: 04/12/2019
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 100fd87c2d40bcb15aa68ad146fb7de693b965a8
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090415"
---
# <a name="group-policy-and-msix-packaged-apps"></a>组策略和 MSIX 打包的应用

使用 MSIX 的开发人员可以采用类似于其他安装程序类型的方式利用组策略。

如果你已将 Win32 应用打包成 MSIX（或者使用桌面桥生成了应用），则该应用已启用完全信任功能。 这样，便可从组策略注册表项中读取数据。 在运行时，你的应用的组策略注册表视图与使用其他方法安装该应用后的相同。 从 Windows 10 版本 1809 开始，如果你的应用是通用 Windows 平台 (UWP) 应用，则该应用可访问这些组策略注册表项。 有关创建组策略的详细信息，请参阅[此文](/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3)。

如果你正在使用 [MSIX 打包工具](./packaging-tool/tool-overview.md)将现有的安装程序转换为 MSIX，则无需执行新的操作，你的应用即可支持组策略。 可以像平时使用原始安装程序后那样管理组策略。 转换为 MSIX 的应用仍可从现有的组策略注册表项中读取数据。 

组策略原生并不支持安装 MSIX 应用程序。 

## <a name="policies-for-blocking-microsoft-store-and-msix"></a>阻止 Microsoft Store 和 MSIX 的策略 

对于要如何配置来自 Microsoft Store 应用的应用更新，你可能有自己的要求。 Microsoft Store 应用会触发第三方应用和第一方应用（如“计算器”和“照片”）等应用的更新。 如果从计算机上删除了 Microsoft Store 应用，则可能导致该计算机上没有应用更新。

下面列出了 Microsoft Store 策略，还显示了各个策略如何影响你的 MSIX 包。

### <a name="turn-off-automatic-download-and-install-of-updates"></a>关闭更新的自动下载和安装

此策略用于启用或禁用应用更新的自动下载和安装。 如果启用此设置，则会关闭应用更新的自动下载和安装。 如果禁用此设置，则会打开应用更新的自动下载和安装。 如果不配置此设置，则应用更新的自动下载和安装由一项注册表设置确定，用户可通过 Microsoft Store 中的“设置”来更改它。

* **GPO：** `Computer Configuration\Administrative Templates\Windows Components\Store`
* **注册表：** `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\WindowsStore AutoDownload REG_DWORD`数据：2 表示“启用”，指不更新应用，4 表示“禁用”，指自动更新应用）
* **应用更新：** 如果启用，则将关闭应用更新的自动下载和安装。 如果禁用，则将打开应用更新的自动下载和安装。 

### <a name="turn-off-store-application"></a>关闭 Microsoft Store 应用程序

此策略用于拒绝或允许访问 Microsoft Store 应用程序。 如果启用此设置，则拒绝访问 Microsoft Store 应用程序。 要安装应用更新，需具备对 Microsoft Store 的访问权限。 如果禁用或不配置此设置，则允许访问 Microsoft Store 应用程序。

* **GPO：** `Computer Configuration\Administrative Templates\Windows Components\Store` 或 `User Configuration\Administrative Templates\Windows Components\Store`
* **注册表：** `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\WindowsStoreRemoveWindowsStore REG_DWORD` 或 `HKEY_CURRENT_USER\Software\Policies\Microsoft\WindowsStoreRemoveWindowsStore REG_DWORD`
* **应用更新：** 如果是在计算机上下文中配置的，则此策略会关闭应用更新。