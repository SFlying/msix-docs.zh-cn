---
title: 了解安装程序
description: 本文列出了在打包桌面应用程序之前需要知道的事项。 你可能不需要执行很多操作即可使应用为打包过程做好准备。
ms.date: 01/29/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 735e7cdaa83573120b55379d22918d89431e87ac
ms.sourcegitcommit: 4d912f89e385268757e87bf8fd9ca1828b99e109
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77544580"
---
# <a name="know-your-installer"></a>了解安装程序

本文列出了在将现有的安装程序转换为 .MSIX 之前需要了解的知识。 你可能无需执行任何操作即可为打包过程准备好应用程序，但如果以下任何项适用于你的应用程序，则需要在打包之前解决。

+ __应用程序具有服务__。 我们支持[使用服务转换应用程序](convert-an-installer-with-services.md)，但务必要记住转换服务的[限制](convert-an-installer-with-services.md#known-limitations)。 转换后，需要管理员提升才能安装包含服务的 .MSIX。 可以从 .MSIX 打包工具版本1.2019.1220.0 开始，使用服务转换应用程序，并且可以在 Windows 10 春季2020发行版中从部署 .MSIX。

+ __安装程序需要重新启动__。 如果安装程序需要[重新启动](support-restart.md)，则从版本1.2019.701.0 开始，.Msix 打包工具支持此项。 如果安装程序返回一个不常见的退出代码以指示它需要重新启动，则应将其添加到 .MSIX 打包工具设置的[重新启动退出代码](tool-best-practices.md#other-settings)部分。 

+ __.NET 应用程序需要低于 4.6.2 的 .NET Framework 版本__。 如果你正在打包 .NET 应用程序，我们建议使应用程序以 .NET Framework 4.6.2 或更高版本为目标。 Windows 10 版本 1607（也称为周年更新）中首次引入了安装和运行打包桌面应用程序的功能，此 OS 版本默认包含 .NET Framework 4.6.2。 更高版本的 OS 包含更高版本的 .NET Framework。 有关更高版本的 Windows 10 中包含的 .NET 版本完整列表，请参阅[此文](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies)。

  在大多数情况下，预期都可以在打包的桌面应用程序中以低于 4.6.2 的 .NET Framework 版本为目标。 但是，如果以低于 4.6.2 的版本为目标，则在将打包的桌面应用程序分发给用户之前，应先对其进行全面测试。

  + 4.0-4.6.1：面向这些版本的 .NET Framework 的应用程序应在4.6.2 或更高版本上运行，而不会出现任何问题。 因此，在 Windows 10 版本 1607 或更高版本上使用 OS 原装 .NET Framework 版本的情况下，这些应用程序应该无需进行任何更改即可安装和运行。

  + 2.0 和3.5：在我们的测试中，以这些版本的 .NET Framework 为目标的打包桌面应用程序通常可以运行，但在某些情况下可能会出现性能问题。 要使这些打包的应用程序能够安装和运行，必须在目标计算机上安装 [.NET Framework 3.5 功能](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10)（此功能还包括 .NET Framework 2.0 和 3.0）。 打包这些应用程序后，还应该对其进行全面的测试。

+ __应用程序需要驱动程序__。 .MSIX 不支持驱动程序。 

+ __应用程序会写入 AppData 文件夹或注册表，目的是与其他应用共享数据__。 转换后，AppData 将重定向到本地应用数据存储，该存储是每个应用的专用应用商店。

  应用程序将写入 HKEY_LOCAL_MACHINE 注册表配置单元的所有条目都将重定向到隔离的二进制文件中，应用程序写入 HKEY_CURRENT_USER 注册表配置单元的任何条目都将按用户、按应用放入专用位置。 有关文件和注册表重定向的更多详细信息，请参阅[在桌面桥幕后](../desktop/desktop-to-uwp-behind-the-scenes.md)。 

 + __应用程序写入应用的安装目录__。 例如，应用程序写入与你的 exe 放置在同一个目录中的日志文件。 这不受支持，因为文件夹受到保护。 建议写入到其他位置，例如本地应用数据存储。 我们添加了一项功能，该功能允许在1809和更高版本中使用。

+ __应用程序使用当前工作目录__。 在运行时，打包的桌面应用程序不会获得先前在桌面 .LNK 快捷方式中指定的相同工作目录。 如果具有正确的目录对应用程序正常运行很重要，需要在运行时更改 CWD。

  > [!NOTE]
  > 如果应用需要写入安装目录或使用当前工作目录，则你还可以考虑将一个使用[包支持框架](https://github.com/microsoft/MSIX-PackageSupportFramework)的运行时修复程序添加到包中。 有关更多详细信息，请参阅[此文](../psf/package-support-framework.md)。  