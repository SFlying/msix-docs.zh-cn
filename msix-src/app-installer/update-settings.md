---
title: 应用安装程序文件更新设置
description: 本文介绍了有关如何使用应用安装程序文件配置应用更新行为的选项。
ms.date: 06/12/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f4ffccb0cdfbdd851c6561593e92059f72198ced
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89089925"
---
# <a name="configure-update-settings-in-the-app-installer-file"></a>在应用安装程序文件中配置更新设置

如 [应用安装程序文件概述](app-installer-file-overview.md)中所述，你可以在应用程序安装程序文件中配置应用程序的更新行为。 本文将探讨更新选项及其各自的利弊。

可以通过使用 [UpdateSettings](/uwp/schemas/appinstallerschema/element-update-settings) 元素来配置应用程序的更新行为。 在这里，我们将探讨更新选项及其各自的利弊。

简而言之，你可以选择通过两种不同的方式来检查更新：
1. 独立于启动应用程序的用户。
2. 仅当用户启动应用时。

此外，还可以选择采用两种不同的方式来应用更新：
1. 通过向用户通知提示。
2. 无提示通知用户。

最后，当你向用户通知某一更新时，你可以在允许用户启动应用之前强制他们执行更新，也可以允许他们启动应用并在时机的时间应用更新。


[UpdateSettings](/uwp/schemas/appinstallerschema/element-update-settings)元素可以具有以下子元素：

| 应用安装程序文件更新设置 | 最小 Windows 10 版本
|------------------|--------------------|
|  OnLaunch| 1709                |
|  HoursBetweenUpdateChecks| 1803                |
| AutomaticBackgroundTask | 1803 |
| UpdateBlocksActivation  | 1903 |
|  ShowPrompt | 1903 |
|  ForceUpdateFromAnyVersion | 1903 |

- **OnLaunch**：启动时检查更新。 这种类型的更新可以显示 UI，并且具有以下属性：

    - **HoursBetweenUpdateChecks**：一个整数，该整数指示系统) 系统将在多长时间内检查应用更新 (。 "0" 到 "255" （含）。 如果未指定此值，则默认值为 24 () 。 例如，如果 HoursBetweenUpdateChecks = 3，则当用户启动应用程序时，如果系统未在过去3小时内检查更新，它将立即检查更新。  

     - **ShowPrompt**：确定 UI 是否向用户显示的布尔值。 Windows 10 版本1903及更高版本支持此值。

     - **UpdateBlocksActivation**：一个布尔值，确定向用户显示的 UI 是否允许用户在不进行更新的情况下启动应用，或者用户必须在启动应用之前获取更新。 仅当 **ShowPrompt** 设置为 "true" 时，此属性才可设置为 "true"。 **UpdateBlocksActivation**= "true" 表示用户将看到的 UI，允许用户进行更新或关闭应用。 **UpdateBlocksActivation**= "false" 表示用户将看到的 UI，允许用户进行更新或启动应用而不进行更新。 在后一种情况下，将在时机时以静默方式应用更新。 Windows 10 版本1903及更高版本支持此值。

        > [!NOTE]
        > 如果 UpdateBlocksActivation 设置为 true，则需要将 ShowPrompt 设置为 true。

- **AutomaticBackgroundTask**：每8小时检查一次后台更新，与用户是否启动应用无关。 此类型的更新无法显示 UI。

- **ForceUpdateFromAnyVersion**：允许应用从版本 x 更新到版本 x + +，或从版本 x 降级到版本 x--。 如果没有此元素，应用只能转到较高的版本。