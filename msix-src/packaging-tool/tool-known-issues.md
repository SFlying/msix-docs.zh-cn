---
title: .MSIX 打包工具的已知问题和故障排除提示
description: 介绍了已知问题，并提供了使用 .MSIX 打包工具将应用程序转换为 .MSIX 时要考虑的故障排除提示。
ms.date: 02/03/2020
ms.topic: article
keywords: msix 打包工具, 已知问题, 故障排除
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4ae2efbc30106e8a5cf6a2861f7577ecae73048a
ms.sourcegitcommit: e650c86433c731d62557b31248c7e36fd90b381d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/02/2020
ms.locfileid: "82726399"
---
# <a name="known-issues-and-troubleshooting-tips-for-the-msix-packaging-tool"></a>MSIX 打包工具的已知问题和故障排除提示

本文描述使用 MSIX 打包工具将应用转换为 MSIX 时存在的已知问题，并提供故障排除提示供用户参考。 如果需要在[断开连接的环境](disconnected-environment.md)中获取 .Msix 打包工具或驱动程序，请查看其他文档。

## <a name="known-issues"></a>已知问题

### <a name="getting-the-latest-insider-preview-build-of-the-msix-packaging-tool"></a>获取 .MSIX 打包工具的最新预览版本

如果已选择加入我们的[预览体验计划](insider-program.md)，请确保具有正确版本的 .Msix 打包工具：
- 请参阅 .MSIX 打包工具中的 "**关于**" 部分，查看你所在的版本。
- 请[在此处](insider-program.md#current-insider-preview-build)确定最新的有问必答预览版本，并确认已安装该版本的 .Msix 打包工具。 请确保注册了试验的 MSA 是登录到 Microsoft Store 的帐户。 
- 通过计算机上的 Microsoft Store 手动更新 .MSIX 打包工具。 如果此选项可供你使用，请打开应用商店，转到 "**下载" 和 "更新**"，然后单击 "**获取更新**"。
- 若要安装 .MSIX 打包工具以供脱机使用，请按照[以下说明进行操作](disconnected-environment.md#get-the-msix-packaging-tool)，以确保通过脱机过程获得最新的应用。

如果你有兴趣加入我们的预览体验计划，请单击[此处](https://aka.ms/MSIXPackagingPreviewProgram)。

### <a name="minimum-version"></a>最低版本

需要注意一些功能，这些功能会自动更改 .MSIX 包中的最小版本支持。 

#### <a name="enforce-microsoft-store-versioning-requirements"></a>强制 Microsoft store 版本控制要求
如果使用早于**1.2019.701.0**的[.msix 打包工具](tool-overview.md)版本转换现有的安装程序，则该工具将强制实施 Microsoft Store 版本控制要求，或使用其他工具来创建未将最低版本设置为10.0.16299.0 的包（Windows 10，版本1709）。 将应用部署到 Windows 10 版本1709或更高版本时，这将导致错误消息。

若要解决此问题，请打开 **.Msix 打包工具**并通过**包编辑器**编辑应用。 打开清单并将`MinVersion` `TargetDeviceFamily`元素的属性设置为 "10.0.16299.0"。

```xml
<Dependencies>
    <TargetDeviceFamily> Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested = "10.0.17763.0" />
</Dependencies>
```

#### <a name="msix-with-services"></a>.MSIX 服务
在 .MSIX 打包工具的1.2019.1220.0 版本中，我们添加了有关[使用服务创建 .msix 包](convert-an-installer-with-services.md)的支持。 由于服务支持的操作系统限制，.MSIX 打包工具会自动将 .MSIX 包支持的最低版本更改为10.0.19025.0。 这意味着，你不能在低于 Windows 10 版本2004的操作系统上安装 .MSIX 和服务，但你可以使用 .MSIX 打包工具向下创建 Windows 10 1809。 如果需要在较低的操作系统上安装此应用，请相应地更新最小版本，但请注意，对服务的支持将不起作用。

### <a name="frameworks-and-drivers"></a>框架和驱动程序

如果应用需要框架，请确保在转换的监视阶段安装框架。 浏览日志以确保发生这种情况。 如果你的应用程序需要安装驱动程序，则需要评估是否需要此驱动程序才能正常运行。 .MSIX 当前不支持驱动程序安装。

### <a name="remote-machine"></a>远程计算机
如果正在使用远程 VM 进行转换时遇到问题，请参阅[远程计算机转换的设置说明](remote-conversion-setup.md)。

### <a name="issues-during-conversion"></a>转换过程中的问题
- 某些安装程序可能会转换失败并出现退出代码 259。 这表示安装程序衍生了一个线程，但未等待该线程完成。 换而言之，主线程已完成安装，但它衍生了一个仍在运行的线程，因此退出并返回了错误 259。 我们建议对 setup.exe 使用适当的安装选项。

### <a name="issues-during-signing"></a>签名过程中的问题

#### <a name="bad-pe-certificate-0x800700c1"></a>错误的 PE 证书（0x800700C1）

当包包含具有损坏证书的二进制文件时，会出现此问题。 若要解决此问题，请`dumpbin.exe /headers`使用命令转储文件头，并检查是否有错误的元素。 手动重写标头以解决问题。 通常，.MSIX 打包工具会自动检测错误的标头。 如果此问题仍然存在，请提供文件反馈。 可在[此处](../desktop/desktop-to-uwp-known-issues.md#bad-pe-certificate-0x800700c1)找到详细信息。

#### <a name="device-guard-signing"></a>Device Guard 签名

请确保按照下列[步骤](../package/signing-package-device-guard-signing.md)操作，并且要在 Microsoft Store for Business 中分配适当的角色。

#### <a name="expired-certificate"></a>证书已过期

- 在对包进行签名时使用时间戳。
- 您可以使用有效的签名或时间戳证书来让步。

您可以使用[批处理转换脚本](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts)来让步您的应用程序。

## <a name="troubleshooting"></a>疑难解答

### <a name="log-files"></a>日志文件

不管转换是否成功，每次转换都会生成日志文件。 可在以下位置找到这些日志文件：

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

将会写入故障代码，指示转换期间出现的任何故障点。 错误代码易于理解。

#### <a name="log-files-from-remote-devices-or-vms"></a>来自远程设备或 VM 的日志文件

如果在远程设备或 VM 上执行转换，我们建议复制该设备提供的日志文件，并将其附加为反馈项的一部分。 这有助于我们更有效地诊断和解决问题。

可在以下位置找到远程转换生成的日志：`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

如果你可以共享整个日志文件夹，该文件夹将包含本地客户端和远程服务器上发生的操作，则更有利。

### <a name="common-problems"></a>常见问题

#### <a name="makeprimanifest-translation-errors"></a>Makepri.exe/Manifest 转换错误

当包的清单出现问题时，会出现此错误。 若要确定问题，请切换到 "包编辑器" 并打开清单。 打开清单时，可以确定问题并提供正确的修补程序。

#### <a name="file-not-found"></a>找不到文件

该文件可以是打开的或不存在的。 若要解决此问题，请添加相应的文件或关闭当前正在使用的文件。 请注意，如果它处于打开`File not Found`状态，则不会收到错误消息。 相反，您会收到`Access Denied`或`File in Use`错误。

#### <a name="file-type-associations"></a>文件类型关联

有关文件类型关联（FTA）的问题与包不同。 .MSIX 打包工具支持双击安装的文件关联。 例如，如果你的应用程序具有上下文菜单，则它不会自动添加，因此你需要将其手动添加到清单中。 有关示例，请参阅[desktop4： FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus)清单元素。

#### <a name="shortcuts-with-arguments"></a>带参数的快捷方式

.MSIX 当前不支持具有参数的快捷方式。 如果检测到安装程序包含这些内容，则 .MSIX 将创建不带参数的磁贴。

#### <a name="install-directory"></a>安装目录

这更常见的情况是使用辅助驱动器执行应用转换的用户。 如果选择更改安装位置，则会更改所有文件所在的根目录。 这意味着 .MSIX 打包工具需要知道所有这些文件的位置，并将在转换过程中捕获这些文件。 

你可以通过使用包支持框架写入来安装目录修补程序来解决此问题。 默认情况下，我们已将此项作为功能添加到 .MSIX 工具中，这将导致1809。 如果你的应用程序在1709中不起作用，并且是1809，则这可能是个问题。

## <a name="sending-feedback"></a>发送反馈

发送反馈的最佳方式是通过[**反馈中心**](https://www.microsoft.com/p/feedback-hub/9nblggh4r32n?activetab=pivot:overviewtab)。

1. 打开“反馈中心”或按 **Windows 键 + F**。****
2. 提供标题和必要的步骤以重现问题。
3. 在“类别”下选择“应用”，然后选择“MSIX 打包工具”。************
4. 附加与转换相关的任何[日志文件](#log-files)。 可以在上面提供的文件夹中找到日志。
5. 附加转换的 MSIX 包（如果可能）。
6. 单击“提交”  。

也可以直接在 MSIX 打包工具中向我们发送反馈，方法是转到“设置”下的“反馈”选项卡。******** 

> [!NOTE]
> 可能需要等待 24 小时，我们才能收到你的反馈。 因此，如果使用 VM 来转换包，可能需要在转换后，持续打开 VM 并使其保持当前状态 24 小时。 此外，还可以手动将转换日志附加到反馈。
