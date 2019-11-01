---
title: .MSIX 打包工具的已知问题和故障排除提示
description: 介绍了已知问题，并提供了使用 .MSIX 打包工具将应用程序转换为 .MSIX 时要考虑的故障排除提示。
ms.date: 10/04/2019
ms.topic: article
keywords: msix 打包工具, 已知问题, 故障排除
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6267957f49cd7a0ca01d9f4e4095bb500d33af69
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328364"
---
# <a name="known-issues-and-troubleshooting-tips-for-the-msix-packaging-tool"></a>.MSIX 打包工具的已知问题和故障排除提示

本文描述使用 MSIX 打包工具将应用转换为 MSIX 时存在的已知问题，并提供故障排除提示供用户参考。

## <a name="known-issues"></a>已知问题

### <a name="getting-the-latest-insider-preview-build-of-the-msix-packaging-tool"></a>获取 .MSIX 打包工具的最新预览版本

如果已选择加入我们的[预览体验计划](insider-program.md)，请确保具有正确版本的 .Msix 打包工具：
- 请参阅 .MSIX 打包工具中的 "**关于**" 部分，查看你所在的版本。

- 请[在此处](insider-program.md#current-insider-preview-build)确定最新的有问必答预览版本，并确认已安装该版本的 .Msix 打包工具。 请确保注册了试验的 MSA 是登录到 Microsoft Store 的帐户。 
- 通过计算机上的 Microsoft Store 手动更新 .MSIX 打包工具。 如果此选项可供你使用，请打开应用商店，转到 "**下载" 和 "更新**"，然后单击 "**获取更新**"。
- 若要安装 .MSIX 打包工具以供脱机使用，请按照[以下说明进行操作](#getting-the-msix-packaging-tool-for-offline-use)，以确保通过脱机过程获得最新的应用。

如果你有兴趣加入我们的预览体验计划，请单击[此处](https://aka.ms/MSIXPackagingPreviewProgram)。

### <a name="msix-packaging-tool-driver-considerations"></a>MSIX 打包工具驱动程序注意事项

.MSIX 打包工具驱动程序作为 Windows 更新的[功能按需（FOD）](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)包提供，如果在计算机上禁用了 Windows 更新服务，或 Windows 预览体验航班环设置与操作系统内部版本不匹配，则将无法安装计算机.

### <a name="installing-msix-packaging-tool-driver-fod-on-windows-insider-builds"></a>在 Windows 预览体验内部版本上安装 MSIX 打包工具驱动程序 FOD

如果在 Windows 10 Insider Preview 内部版本上安装该驱动程序时遇到问题：

- 导航到“设置” > “更新和安全性” > “Windows 预览体验计划”。
- 如果有消息指出 Insider Preview 内部版本 设置需要关注，请单击“修复”按钮以重新登录。 在设置更改生效之前，可能需要转到“Windows 更新”页并检查更新。
- 尝试再次运行该工具，以下载 MSIX 打包工具驱动程序。 如果仍遇到问题，请尝试将外部测试环更改为“预览体验成员 - 快”，安装最新的 Windows 更新，然后重试。

### <a name="installing-msix-packaging-tool-driver-fod-in-wsus"></a>在 WSUS 中安装 MSIX 打包工具驱动程序 FOD

使用 Windows Server Update Services (WSUS) 的组织必须采取措施来手动安装该驱动程序：

- [检查 Windows 版本](https://support.microsoft.com/help/13443/windows-which-operating-system)。 必须至少使用 Windows 10 版本 1809（内部版本 17763）。
- 下载适用于 [Windows 10 版本 1809](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) 的 FOD .cab 文件。
- 可以使用 [DISM 命令行选项](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options)安装单独获取的按需功能 (FOD) 包。 在权限提升的 PowerShell 窗口中键入：```Dism /Online /add-package /packagepath:(path)```

IT 管理员还可以创建[并列功能存储（共享文件夹）](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server)，以允许访问 MSIX 打包工具驱动程序 FOD。 可在此[博客文章](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404)的末尾找到更多详细信息。

如果你有权访问企业渠道或 OEM 渠道，则可以通过以下来源之一，从 Windows 10 按需功能媒体获取该驱动程序：

- [批量许可服务中心（VLSC）](https://www.microsoft.com/Licensing/servicecenter/default.aspx)：需要批量许可证访问。
- [Oem 门户](https://www.microsoftoem.com)：需要 oem 访问权限。
- [Msdn 下载](https://my.visualstudio.com/Downloads/Featured)：需要 msdn 订阅。

可以使用 DISM 命令行选项安装单独获取的按需功能包。

### <a name="getting-the-msix-packaging-tool-for-offline-use"></a>获取 MSIX 打包工具供脱机使用

可以从适用于企业的 Microsoft Store [Web 门户](https://businessstore.microsoft.com/store)下载 MSIX 打包工具供脱机使用。 可在[此处](https://docs.microsoft.com/microsoft-store/distribute-offline-apps)详细了解离线分发。 如果在使用打包工具的脱机副本时遇到问题，请确保你有该工具的[许可证脱机副本](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)。 

有了应用程序的脱机版本后，可以使用 [PowerShell](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) 将应用包和许可证添加到计算机。

### <a name="frameworks-and-drivers"></a>框架和驱动程序

如果应用需要框架，请确保在转换的监视阶段安装框架。 浏览日志以确保发生这种情况。 如果你的应用程序需要安装驱动程序，则需要评估是否需要此驱动程序才能正常运行。 .MSIX 当前不支持驱动程序安装。

### <a name="remote-machine"></a>远程计算机
如果正在使用远程 VM 进行转换时遇到问题，请参阅[远程计算机转换的设置说明](remote-conversion-setup.md)。

### <a name="issues-during-conversion"></a>转换过程中的问题
- 在转换期间，安装程序可能会运行服务。 在转换期间不会捕获服务。 因此，你的应用程序可能会安装，但可能会遇到问题。
- 某些安装程序可能会转换失败并出现退出代码 259。 这表示安装程序衍生了一个线程，但未等待该线程完成。 换而言之，主线程已完成安装，但它衍生了一个仍在运行的线程，因此退出并返回了错误 259。 我们建议对 setup.exe 使用适当的安装选项。

### <a name="issues-during-signing"></a>签名过程中的问题

#### <a name="bad-pe-certificate-0x800700c1"></a>错误的 PE 证书（0x800700C1）

当包包含具有损坏证书的二进制文件时，会出现此问题。 若要解决此问题，请使用 `dumpbin.exe /headers` 命令转储文件头，并检查是否有错误的元素。 手动重写标头以解决问题。 通常，.MSIX 打包工具会自动检测错误的标头。 如果此问题仍然存在，请提供文件反馈。 可以在[此处](../desktop/desktop-to-uwp-known-issues.md#bad-pe-certificate-0x800700c1)查找详细信息。

#### <a name="device-guard-signing"></a>Device Guard 签名

请确保按照下列[步骤](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)操作，并且要在 Microsoft Store for Business 中分配适当的角色。

#### <a name="expired-certificate"></a>过期的证书

- 在对包进行签名时使用时间戳。
- 您可以使用有效的签名或时间戳证书来让步。

您可以使用[批处理转换脚本](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts)来让步您的应用程序。

## <a name="troubleshooting"></a>“疑难解答”

### <a name="log-files"></a>日志文件

不管转换是否成功，每次转换都会生成日志文件。 可在以下位置找到这些日志文件：

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

将会写入故障代码，指示转换期间出现的任何故障点。 错误代码易于理解。

#### <a name="log-files-from-remote-devices-or-vms"></a>来自远程设备或 VM 的日志文件

如果在远程设备或 VM 上执行转换，我们建议复制该设备提供的日志文件，并将其附加为反馈项的一部分。 这有助于我们更有效地诊断和解决问题。

可在以下位置找到远程转换生成的日志：`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

如果可以共享包含本地客户端以及远程服务器上发生的操作的整个 Logs 文件夹，则可为我们提供更大的便利。

### <a name="common-problems"></a>常见问题

#### <a name="makeprimanifest-translation-errors"></a>Makepri.exe/Manifest 转换错误

当包的清单出现问题时，会出现此错误。 若要确定问题，请切换到 "包编辑器" 并打开清单。 打开清单时，可以确定问题并提供正确的修补程序。

#### <a name="file-not-found"></a>找不到文件

该文件可以是打开的或不存在的。 若要解决此问题，请添加相应的文件或关闭当前正在使用的文件。 请注意，如果它处于打开状态，则不会收到 `File not Found` 错误。 相反，您将收到 `Access Denied` 或 `File in Use` 错误。

#### <a name="file-type-associations"></a>文件类型关联

有关文件类型关联（FTA）的问题与包不同。 .MSIX 打包工具支持双击安装的文件关联。 例如，如果你的应用程序具有上下文菜单，则它不会自动添加，因此你需要将其手动添加到清单中。 有关示例，请参阅[desktop4： FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus)清单元素。

#### <a name="shortcuts-with-arguments"></a>带参数的快捷方式

.MSIX 当前不支持具有参数的快捷方式。 如果检测到安装程序包含这些内容，则 .MSIX 将创建不带参数的磁贴。

#### <a name="install-directory"></a>安装目录

这更常见的情况是使用辅助驱动器执行应用转换的用户。 如果选择更改安装位置，则会更改所有文件所在的根目录。 这意味着 .MSIX 打包工具需要知道所有这些文件的位置，并将在转换过程中捕获这些文件。 

## <a name="sending-feedback"></a>发送反馈

发送反馈的最佳方式是通过[**反馈中心**](https://www.microsoft.com/p/feedback-hub/9nblggh4r32n?activetab=pivot:overviewtab)。

1. 打开“反馈中心”或按 **Windows 键 + F**。
2. 提供标题和必要的步骤以重现问题。
3. 在“类别”下选择“应用”，然后选择“MSIX 打包工具”。
4. 附加与转换相关的任何[日志文件](#log-files)。 可以在上面提供的文件夹中找到日志。
5. 附加转换的 MSIX 包（如果可能）。
6. 单击**提交**。

也可以直接在 MSIX 打包工具中向我们发送反馈，方法是转到“设置”下的“反馈”选项卡。 

> [!NOTE]
> 可能需要等待 24 小时，我们才能收到你的反馈。 因此，如果使用 VM 来转换包，可能需要在转换后，持续打开 VM 并使其保持当前状态 24 小时。 此外，还可以手动将转换日志附加到反馈。
