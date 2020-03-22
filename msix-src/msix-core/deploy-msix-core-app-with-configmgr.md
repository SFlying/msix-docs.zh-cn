---
title: 通过 Microsoft 终结点部署 .MSIX Core 应用 Configuration Manager
description: 介绍如何将 .MSIX Core 与 Microsoft Endpoint Configuration Manager 一起部署。
ms.date: 03/02/2020
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: b338f9bb5b994b21164278a62983cf9f7cb531df
ms.sourcegitcommit: e703ffe4c635d9b127ecf8c02e087370b676aa9c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80070022"
---
# <a name="deploy-msix-core-apps-with-microsoft-endpoint-configuration-manager"></a>通过 Microsoft 终结点部署 .MSIX Core 应用 Configuration Manager
使用 Microsoft 端点 Configuration Manager 传递 .MSIX 应用程序允许 IT 专业人员将其他应用程序链接为依赖项，并强制它们在之前安装。 通过创建 .MSIX Core 应用程序的依赖项，我们强制仅在设备需要时才安装 .MSIX Core 应用程序。 有关 Micosoft 终结点中的应用程序依赖关系的详细信息 Configuration Manager 参阅：[创建应用程序：部署类型依赖关系](https://docs.microsoft.com/configmgr/apps/deploy-use/create-applications#bkmk_dt-depend)。

## <a name="get-started"></a>入门
以下步骤将指导你完成使用 Microsoft Endpoint Configuration Manager 设置 .MSIX 核心部署策略的步骤：

1. [将 .MSIX Core 与 Microsoft 终结点一起部署 Configuration Manager](deploy-msix-core-with-configmgr.md)
1. [更新现有的 .MSIX 包以支持 .MSIX 核心](support-msix-core.md)
1. 通过 Microsoft 终结点部署 .MSIX Core 应用 Configuration Manager

## <a name="creating-the-msix-core-microsoft-endpoint-configuration-manager-application"></a>Configuration Manager 应用程序创建 .MSIX Core Microsoft 端点
以下过程将指导你创建 Microsoft 端点 Configuration Manager 应用程序，以便将 .MSIX Core 应用程序部署到客户端设备。
 
假设你遵循了前面的指南（请参阅上面 "入门" 部分中的指南列表），并已检索/更新/创建了 .MSIX Core 启用应用。 并已将应用程序复制到可由 Microsoft Endpoint Configuration Manager 工具访问的文件共享中。 下一步是将新应用部署到环境中的客户端设备。

### <a name="create-msix-core-dependent-application-in-microsoft-endpoint-configuration-manager"></a>在 Microsoft 终结点中创建 .MSIX Core 从属应用程序 Configuration Manager
1. 从 Microsoft 终结点 Configuration Manager 控制台导航到：**软件库 > 概述/应用程序管理/应用程序**。
1. 从功能区中选择 "**创建应用程序**"。
1. 选择 "**手动指定应用程序信息"** 单选按钮。
1. 选择 "**下一步**" 按钮。
1. 在相应的字段中输入应用程序详细信息。
1. 选择 "**下一步**" 按钮两次。
1. 选择 "**添加**" 按钮。
1. 将 "类型" 设置为 "**脚本安装程序**"。
1. 选择 "**下一步**" 按钮。
1. 输入后缀为 " **-MSIXCore**" 的应用程序名称（即： "application MSIXCore"）。
1. 选择 "**下一步**" 按钮。
1. 选择 "内容位置" 旁边的 "**浏览**" 按钮，然后导航到包含应用安装媒体的文件共享。
1. 选择 "**选择文件夹**" 按钮。
1. 选择 "安装程序" 旁边的 "**浏览**" 按钮，将 "文件类型" 设置为 "**所有文件（*. *）** "，然后选择安装媒体。
1. 选择 "**打开**" 按钮。
1. 将安装程序字段更新为： 
```batch
"C:\Program Files\msixmgr\msixmgr.exe -AddPackage [Application.msix] -quietUX"
```
1. 将 "卸载程序" 字段设置为： 
```batch
"C:\Program Files\msixmgr\msixmgr.exe" -RemovePackage [Package Family Name] -quietUX
```
1. 将 [Package 家族 Name] 替换为 .MSIX 应用程序的包系列名称。
1. 选择 "**下一步**" 按钮。
1. 选择 "**使用自定义脚本检测此部署类型的状态"** 单选按钮。
1. 选择 "**编辑**" 按钮。
1. 验证是否已将脚本类型设置为**PowerShell**
1. 输入以下内容： 
```PowerShell
Set-Location "C:\Program Files\msixmgr"

IF([Boolean]$(get-item "msixmgr.exe"))
{
    $Result = $(.\msixmgr.exe -FindPackage [Package Family Name]*)

    IF($($Result.GetType().Name) -eq "Object[]")
    {
        Return 1
    }
}
```
1. 用应用程序的 .MSIX 包系列名称更新 [包系列名称]。
1. 选择 "**确定"** 按钮。
1. 选择 "**下一步**" 按钮。
1. 设置要**为用户安装**的安装行为。
1. 为此应用程序设置值 approriate 的最大允许运行时间（分钟）和估计安装时间（分钟）。
1. 将安装程序可见性设置为**隐藏**。
1. 选择 "**下一步**" 按钮。
1. 选择 "**添加**" 按钮。
1. 确保已将 "类别" 设置为 "**设备**"。
1. 将条件设置为**操作系统**
1. 从操作系统列表中选择 " **Windows 7** " 复选框。
1. 选择 "**确定"** 按钮。
1. 选择 "**下一步**" 按钮。
1. 选择 "**添加**" 按钮。
1. 将 "依赖关系组名称" 设置为 " **.Msix Core**"。
1. 选择 "**添加**" 按钮。
1. 从可用应用程序列表中选择 " **.Msix Core** "。
1. 从 "部署类型" 列表中选择 "32 位" 和 "64 位" 选项。
1. 选择 "**确定"** 按钮。
1. 选择 "**确定"** 按钮。
1. 选择 "**下一步**" 按钮两次。
1. 选择 "**关闭**" 按钮。

### <a name="add-non-msix-core-dependent-deployment-type"></a>添加非 .MSIX Core 从属部署类型
1. 选择 "**添加**" 按钮。
1. 确保已将类型设置为 **Windows 应用包（* .appx，* .appxbundle，*. .msix，*. .msixbundle） * *。 
1. 选择 "**浏览 ...** " 按钮，导航到 .msix Core enlighted 应用程序安装媒体，然后选择 "**打开**" 按钮。
1. 选择 "**下一步**" 按钮六次。
1. 选择 "**关闭**" 按钮。
1. 选择 "**下一步**" 按钮两次。
1. 选择 "**关闭**" 按钮。
