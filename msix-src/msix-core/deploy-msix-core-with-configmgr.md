---
title: 将 .MSIX Core 与 Microsoft 终结点一起部署 Configuration Manager
description: 介绍如何将 .MSIX Core 与 Microsoft Endpoint Configuration Manager 一起部署。
ms.date: 03/02/2020
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: faa5930763d1bb554f3c21a8f390e67996b54cf4
ms.sourcegitcommit: fa41875f6c2b79db3d7dde29b10c0f24765532bc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79111309"
---
# <a name="deploy-msix-core-with-microsoft-endpoint-configuration-manager"></a>将 .MSIX Core 与 Microsoft 终结点一起部署 Configuration Manager
.MSIX Core 使企业能够根据应用程序的 .MSIX 打包格式实现标准化。 IT 专业人员可以轻松地将 .MSIX Core 安装程序部署到其设备，并启用：安装、查询和删除 .MSIX 封装应用（已在这些 Windows 版本中工作）。

.MSIX Core 提供了本机 .MSIX 的一小部分功能，其功能类似于现有的 Win32 安装程序类型。 .MSIX Core 不提供本机 .MSIX 的容器优点，也不允许使用 Windows 10 特定功能的应用在早期版本的 Windows 版本上运行。

## <a name="get-started"></a>入门
以下步骤将指导你完成使用 Microsoft Endpoint Configuration Manager 设置 .MSIX 核心部署策略的步骤：

1. 将 .MSIX Core 与 Microsoft 终结点一起部署 Configuration Manager
1. [更新现有的 .MSIX 包以支持 .MSIX 核心](support-msix-core.md)
1. [通过 Microsoft 终结点部署 .MSIX Core 应用 Configuration Manager](deploy-msix-core-app-with-configmgr.md)

> [!Note]
> 根据需要，在完成本指南后，你可以选择在安装任何 .MSIX Core 启用应用之前将 .MSIX Core 应用程序的安装推送到环境中的所有所需设备。

## <a name="creating-the-msix-core-application"></a>创建 .MSIX Core 应用程序
以下过程将指导你创建 Microsoft 端点 Configuration Manager 应用程序，以便将 .MSIX Core 部署到 Windows 7 设备。 如果打算将此类推送到其他版本的 Windows 操作系统，请更改应用程序要求，使其面向那些选择的操作系统版本。
 
### <a name="download-msix-core"></a>下载 .MSIX Core
1. 下载最新版本的[.Msix Core](https://github.com/microsoft/msix-packaging/releases)。
1. 将下载的 .MSIX Core （x86/x64）安装媒体移到你的 Microsoft 端点 Configuration Manager 环境可访问的文件共享中。

### <a name="create-microsoft-endpoint-configuration-manager-application---msix-core"></a>Configuration Manager 应用程序创建 Microsoft 终结点-.MSIX 核心
1. Configuration Manager 控制台打开 Microsoft 终结点。 导航到 "**软件库" > 概述 \ 应用程序管理 \ 应用**程序 "。
1. 从功能区中选择 "**创建应用程序**"。
1. 确保已将**类型**设置为**Windows Installer （* .msi 文件）** 。 
1. 选择 "**浏览 ...** " 按钮，导航到下载的 .msix Core （x64）安装媒体，然后选择 "**打开**" 按钮。
1. 选择 "**下一步**" 按钮两次。
1. 查看列出的信息并更新任何空白或缺失字段（可选）。
1. 选择 "**下一步**" 按钮两次。
1. 选择 "**关闭**" 按钮。
1. 在应用程序列表中右键单击 " **.Msix Core** "，然后从下拉菜单中选择 "**属性**"。
1. 从新打开的窗口顶部选择 "**部署类型**" 选项卡。
1. 从部署类型列表中选择 " **.Msix Core Windows Installer （* .msi 文件）** "，然后选择 "**编辑**" 按钮。
1. 将**名称**设置为 "**Windows Installer .msix （* .msi 文件）-64 位**"。
1. 选择新打开的 "属性" 窗口顶部的 "**要求**" 选项卡。
1. 选择 "**添加**" 按钮。
1. 将 "类别" 设置为 "**设备**"，并将 "条件" 设置为 "**操作系统**"。
1. 在操作系统列表中，选中 " **windows 7 > 所有 windows 7 （64位）** " 复选框。
1. 选择 **"确定"** 按钮两次。
1. 选择 "**添加**" 按钮。
1. 确保已将**类型**设置为**Windows Installer （* .msi 文件）** 。
1. 选择 "**浏览 ...** " 按钮，导航到下载的 .msix Core （x86）安装媒体，然后选择 "**打开**" 按钮。
1. 选择 "**下一步**" 按钮两次。
1. 查看列出的信息并更新任何空白或缺失字段（可选）。
1. 选择 "**下一步**" 按钮两次。
1. 选择 "**关闭**" 按钮。
1.  从部署类型列表中选择 " **.Msix Core Windows Installer （* .msi 文件）** "，然后选择 "**编辑**" 按钮。
1. 将**名称**设置为 "**Windows Installer .msix （* .msi 文件）-32 位**"。
1. 选择新打开的 "属性" 窗口顶部的 "**要求**" 选项卡。
1. 选择 "**添加**" 按钮。
1. 将 "类别" 设置为 "**设备**"，并将 "条件" 设置为 "**操作系统**"。
1. 在操作系统列表中，选中 " **windows 7 > 所有 windows 7 （32位）** " 复选框。
1. 选择 **"确定"** 按钮三次。

## <a name="distribute-msix-core-application-to-distribution-points"></a>将 .MSIX Core 应用程序分发到分发点
以下内容将指导你将 .MSIX 核心安装媒体分发到所有分发点，前提是你的环境中存在分发点组 "所有分发点"。 请为你的环境选择合适的目标（分发点、分发点组或集合）。

1. 从 Microsoft 端点 Configuration Manager 控制台中： "**软件库" > 概述/应用程序管理/应用程序** 
1. 在应用程序列表中右键单击 " **.Msix Core** "。
1. 从下拉菜单中选择 "**分发内容**"。
1. 选择 "**下一步**" 按钮两次。
1. 选择 "**添加**" 按钮，然后从下拉菜单中选择 "**分发点组**"。
1. 选中 "**所有分发点**" 复选框，然后选择 "**确定"** 按钮。
1. 选择 "**下一步**" 按钮两次。
1. 选择 "**关闭**" 按钮。
