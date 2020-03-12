---
title: 应用包体系结构
description: 详细了解在生成你的 UWP 应用包时应使用哪些处理器体系结构。
ms.date: 07/13/2017
ms.topic: article
keywords: windows 10、uwp、打包、体系结构、包配置
ms.localizationpriority: medium
ms.openlocfilehash: f64c4ae114d334cb614c417ca45616cc50cd0654
ms.sourcegitcommit: fa41875f6c2b79db3d7dde29b10c0f24765532bc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79097907"
---
# <a name="app-package-architectures"></a>应用包体系结构

应用包配置为在特定处理器体系结构上运行。 通过选择体系结构，你可以指定要在哪个设备上运行应用。 通用 Windows 平台（UWP）应用可以配置为在以下体系结构上运行：
- x86
- x64
- ARM
- ARM64

**强烈**建议生成应用包以面向所有体系结构。 通过取消选择设备体系结构，你会限制应用可在其上运行的设备的数量，这反过来会限制可以使用你的应用的人员！

## <a name="windows-10-devices-and-architectures"></a>Windows 10 设备和体系结构

> [!div class="mx-tableFixed"]
| UWP 体系结构 | 桌面（x86）      | 桌面（x64）      | 桌面（ARM）      | 移动设备             | Windows Mixed Reality 和 HoloLens           | Xbox               | IoT Core （依赖于设备） | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | x-blade                | :heavy_check_mark: | x-blade                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | x-blade                | :heavy_check_mark: | x-blade                | x-blade                | x-blade                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM               | x-blade                | x-blade                | :heavy_check_mark: | :heavy_check_mark: | x-blade                | x-blade                | :heavy_check_mark:          | x-blade                |
| ARM64              | x-blade                | x-blade                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | x-blade                | :heavy_check_mark:          | x-blade                |


让我们更详细地介绍这些体系结构。

### <a name="x86"></a>x86
选择 x86 通常是应用程序包的最安全的配置，因为它将在几乎每个设备上运行。 在某些设备上，不会运行具有 x86 配置的应用包，如 Xbox 或某些 IoT 核心设备。 但是，对于 PC，x86 包是最安全的选择，并具有最大的设备部署范围。 Windows 10 设备的一个巨大部分将继续运行 x86 版本的 Windows。

### <a name="x64"></a>x64
此配置的使用频率比 x86 配置少。 应注意的是，在 Intel Joule 上使用64位版本的 Windows 10、 [UWP 上的 UWP 应用](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation)和 Windows 10 IoT Core 的桌面保留了此配置。

### <a name="arm-and-arm64"></a>ARM 和 ARM64
Windows 10 on ARM 配置包括台式 Pc、移动设备和某些 IoT 核心设备（Rasperry Pi 2、Raspberry Pi 3 和 DragonBoard）。 ARM 桌面电脑上的 windows 10 是 Windows 系列的新新增方案，因此，如果您是 UWP 应用程序开发人员，则应将 ARM 包提交到应用商店，以获得有关这些电脑的最佳体验。

>[!NOTE]
> 若要生成 UWP 应用程序以本机方式面向 ARM64 平台，你必须安装有 Visual Studio 2017 版本15.9 或更高版本。 有关详细信息，请参阅[此博客文章](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)。

有关详细信息，请参阅[ARM 上的 Windows 10](https://docs.microsoft.com/windows/uwp/porting/apps-on-arm.md)。 查看此 build 的讨论，观看[ARM 上 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171)的演示，并了解其工作原理的详细信息。

有关 IoT 特定主题的详细信息，请参阅[使用 Visual Studio 部署应用](https://developer.microsoft.com/windows/iot/Docs/AppDeployment)。
