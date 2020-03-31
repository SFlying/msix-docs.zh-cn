---
title: 应用包体系结构
description: 详细了解在生成你的 UWP 应用包时应使用哪些处理器体系结构。
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10, uwp, 打包, 体系结构, 包配置
ms.localizationpriority: medium
ms.openlocfilehash: cbd8924a947d27afd62d6f870b67a6f0f810d69d
ms.sourcegitcommit: f6cee51b46fc36a57b5cf9c8cb3fd24a40ae858a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391655"
---
# <a name="app-package-architectures"></a>应用包体系结构

应用包配置为在特定处理器体系结构上运行。 通过选择体系结构，你就指定了你希望应用在何种设备上运行。 通用 Windows 平台 (UWP) 应用可配置为在以下体系结构上运行：
- x86
- x64
- ARM
- ARM64

**强烈**建议你生成面向所有体系结构的应用包。 取消选择设备体系结构，也就限制了你的应用可以运行于其上的设备的数量，从而限制了可以使用你的应用的人群数量。

## <a name="windows-10-devices-and-architectures"></a>Windows 10 设备和体系结构

> [!div class="mx-tableFixed"]
| UWP 体系结构 | 桌面 (x86)      | 桌面 (x64)      | 桌面 (ARM)      | 移动设备             | Windows Mixed Reality 和 HoloLens           | Xbox               | IoT Core（与设备相关） | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM               | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |
| ARM64              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :x:                |


下面我们更详细地讨论这些体系结构。

### <a name="x86"></a>x86
选择 x86 通常对于应用包是最安全的配置，因为它几乎可以在每台设备上运行。 在某些设备上，使用 x86 配置的应用包无法运行，如 Xbox 或某些 IoT Core 设备。 但是，对于 PC 来说，x86 包是最安全的选项，并且具有最广泛的设备部署范围。 Windows 10 设备中有很大一部分将继续运行 x86 版本的 Windows。

### <a name="x64"></a>x64
此配置为比 x86 配置使用得较少。 应注意的是，此配置专为使用 64 位版本的 Windows 10 的桌面、[Xbox 上的 UWP 应用](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation)和 Intel Joule 上的 Windows 10 IoT 核心版保留。

### <a name="arm-and-arm64"></a>ARM 和 ARM64
ARM 上的 Windows 10 配置包括桌面 PC、移动设备，以及一些 IoT Core 设备（Rasperry Pi 2、Raspberry Pi 3 和 DragonBoard）。 在 ARM 桌面 PC 上的 Windows 10 是 Windows 系列的新增产品，因此，如果你是 UWP 应用开发人员，则应将 ARM 包提交到应用商店，以便在这些 PC 上提供最佳体验。

>[!NOTE]
> 若要生成 UWP 应用程序以本机方式面向 ARM64 平台，你必须安装有 Visual Studio 2017 版本15.9 或更高版本。 有关详细信息，请参阅[此博客文章](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)。

有关详细信息，请参阅[ARM 上的 Windows 10](https://docs.microsoft.com/windows/uwp/porting/apps-on-arm.md)。 查看此“构建”相关讨论，观看 [ARM 上的 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171)演示，并详细了解它的工作原理。

有关 IoT 特定主题的详细信息，请参阅[使用 Visual Studio 部署应用](https://developer.microsoft.com/windows/iot/Docs/AppDeployment)。
