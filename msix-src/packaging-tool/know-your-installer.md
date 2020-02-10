---
title: 了解安装程序
description: 本文列出了打包桌面应用程序之前需要了解的一些知识。 你可能不需要执行很多操作即可使应用为打包过程做好准备。
ms.date: 01/29/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 66577e142bfbc3c90ba9486b24463accf2c7f524
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073753"
---
# <a name="know-your-installer"></a>了解安装程序

本文列出了在将现有的安装程序转换为 .MSIX 之前需要了解的知识。 你可能无需执行任何操作即可为打包过程准备好应用程序，但如果以下任何项适用于你的应用程序，则需要在打包之前解决。

+ __应用程序具有服务__。 我们支持[使用服务转换应用程序](convert-an-installer-with-services.md)，但务必要记住转换服务的[限制](convert-an-installer-with-services.md#known-limitations)。 转换后，需要管理员提升才能安装包含服务的 .MSIX。 可以从 .MSIX 打包工具版本1.2019.1220.0 开始，使用服务转换应用程序，并且可以在 Windows 10 春季2020发行版中从部署 .MSIX。

+ __安装程序需要重新启动__。 如果安装程序需要[重新启动](support-restart.md)，则从版本1.2019.701.0 开始，.Msix 打包工具支持此项。 如果安装程序返回一个不常见的退出代码以指示它需要重新启动，则应将其添加到 .MSIX 打包工具设置的[重新启动退出代码](tool-best-practices.md#other-settings)部分。 

+ __你的 .net 应用程序需要 .NET Framework 早于4.6.2 的版本__。 如果正在打包 .NET 应用程序，我们建议将应用程序目标 .NET Framework 4.6.2 或更高版本。 Windows 10 版本1607（也称为周年更新）中首次引入了安装和运行打包桌面应用程序的功能，默认情况下，此 OS 版本包含 .NET Framework 4.6.2。 更高版本的操作系统包括 .NET Framework 的更高版本。 有关更高版本的 Windows 10 中包含的 .NET 版本的完整列表，请参阅[此文](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies)。

  在大多数情况下，适用于打包桌面应用程序中4.6.2 之前的 .NET Framework 的目标版本应能正常工作。 但是，如果面向的是比4.6.2 更早的版本，则应在将打包的桌面应用程序分发给用户之前对其进行完整的测试。

  + 4.0-4.6.1：面向这些版本的 .NET Framework 的应用程序应在4.6.2 或更高版本上运行，而不会出现任何问题。 因此，这些应用程序应在 Windows 10 版本1607或更高版本上安装和运行，并附带操作系统附带的 .NET Framework 版本。

  + 2.0 和3.5：在我们的测试中，以这些版本的 .NET Framework 为目标的打包桌面应用程序通常可以运行，但在某些情况下可能会出现性能问题。 为了安装和运行这些打包的应用程序，必须在目标计算机上安装[.NET Framework 3.5 功能](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10)（此功能还包括 .NET Framework 2.0 和3.0）。 打包后，还应彻底测试这些应用程序。

+ __应用程序需要驱动程序__。 .MSIX 不支持驱动程序。 

+ __应用程序将数据写入 AppData 文件夹或注册表，目的是与另一个应用程序共享数据__。 转换后，AppData 将重定向到本地应用数据存储，每个应用都是专用存储。

  应用程序写入 HKEY_LOCAL_MACHINE 注册表配置单元的所有项都将重定向到一个独立的二进制文件，并且应用程序写入 HKEY_CURRENT_USER 注册表配置单元的所有项都将放置到专用的每个用户、每个应用的位置。 有关文件和注册表重定向的更多详细信息，请参阅[在桌面桥幕后](../desktop/desktop-to-uwp-behind-the-scenes.md)。 

 + __您的应用程序将写入您的应用程序的安装目录__。 例如，你的应用程序将写入日志文件，并将其放入你的 exe 所在的同一目录中。 这不受支持，因为文件夹受到保护。建议写入到其他位置，例如本地应用数据存储。 我们添加了一项功能，该功能允许在1809和更高版本中使用。

+ __应用程序使用当前工作目录__。 在运行时，打包的桌面应用程序将不会获得你先前在桌面中指定的工作目录.LNK 快捷方式。 如果要使应用程序正常运行，则需要在运行时更改 CWD。

  > [!NOTE]
  > 如果你的应用程序需要写入安装目录或使用当前工作目录，则还可以考虑使用包[支持框架](https://github.com/microsoft/MSIX-PackageSupportFramework)将运行时链接添加到包。 有关更多详细信息，请参阅[此文](../psf/package-support-framework.md)。  