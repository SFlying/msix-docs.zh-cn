---
title: 应用安装程序文件更新设置
description: 了解如何使用应用安装程序文件配置应用更新。
author: mcleanbyron
ms.author: mcleans
ms.date: 12/12/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5470a3e8c9b39734645da838b467aaf32bf8e79f
ms.sourcegitcommit: e7d974ff7b318af19aa8d578d031914e1f1ff926
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826726"
---
# <a name="configure-update-settings-in-the-app-installer-file"></a>在应用安装程序文件中配置更新设置

如中所述[应用安装程序文件概述](app-installer-file-overview.md)，可以在应用安装程序文件中配置应用程序的更新行为。 本文探讨了更新选项和及其各自的利弊。

可以通过配置应用程序的更新行为[UpdateSettings](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings)元素。 现在，我们探讨更新选项和及其各自的利弊。

简单地说，您可以选择检查更新两个不同的方式：
1. 独立于用户启动应用。
2. 仅当用户启动应用。

此外，您可以选择两个不同的方式应用更新：
1. 通过通知提示用户。
2. 以无提示方式，而不通知用户。

最后时通知用户更新，您可以强制他们采取 update，然后再允许他们启动应用，或可以允许它们进行启动该应用程序并将应用在扩大的更新。

具体而言，以下是可供您的语法：

- [UpdateSettings](https://docs.microsoft.com/uwp/schemas/appinstallerschema/element-update-settings)元素可以具有以下元素：

    - **OnLaunch**:在启动更新检查。 此类型的更新可以显示用户界面，具有以下属性：

        - **ShowPrompt**:一个布尔值，确定是否将向用户显示 UI。 在 Windows 10，版本 1903年及更高版本上支持此值。

        - **UpdateBlocksActivation**:一个布尔值，确定向用户显示的 UI 可让用户以启动应用而无需使更新，还是用户所必须执行的更新前启动应用程序。 此属性可以设置为"true"仅当**ShowPrompt**设置为"true"。 **UpdateBlocksActivation**="true"表示用户界面，用户将看到，允许用户执行更新或关闭应用程序。 **UpdateBlocksActivation**="false"表示用户界面，用户将看到，允许用户执行更新或启动应用程序而不更新。 在后一种情况下，更新将以无提示方式在应用扩大。 在 Windows 10，版本 1903年及更高版本上支持此值。

        > [!NOTE]
        > ShowPrompt 需要设置为 true，如果 UpdateBlocksActivation 设置为 true。

        - **HoursBetweenUpdateChecks**:一个整数，指示频率 （以小时数） 系统会检查应用的更新。 "0"到"255"非独占。 （如果未指定此值），默认值为 24。 例如如果 HoursBetweenUpdateChecks = 3，则当用户启动应用时，如果在过去 3 小时内更新尚未检查系统，它将检查现在是否有更新。  

    - **AutomaticBackgroundTask**:在后台独立于用户是否启动了应用程序每隔 8 小时更新检查。 此类型的更新无法显示 UI。

    - **ForceUpdateFromAnyVersion**:允许应用从版本 x 更新到版本 x + + 或降级到版本 x-从版本 x。 如果没有该元素，该应用程序只能移动到更高版本。
