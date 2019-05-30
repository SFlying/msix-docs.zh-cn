---
title: 已知问题和疑难解答
description: 介绍了已知的问题并提供故障排除提示 MSIX 打包工具。
author: dianmsft
ms.author: diahar
ms.date: 02/26/2019
ms.topic: article
keywords: msix 打包工具，已知问题进行故障排除
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 79b06fc4e3e0ff0600f6e2ab85ec27c9e3a5840c
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59468289"
---
# <a name="known-issues-and-troubleshooting"></a>已知问题和疑难解答

本文介绍了已知的问题，并提供故障排除技巧以将您的应用程序转换为 MSIX 使用 MSIX 打包工具时，请考虑。

## <a name="known-issues"></a>已知问题

### <a name="msix-packaging-tool-driver-considerations"></a>MSIX 打包工具驱动程序注意事项

MSIX 打包工具驱动程序作为提供[功能上需 (FOD)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)从 Windows 更新包和将无法安装如果在计算机上禁用 Windows Update 服务或 Windows 预览体验航班环设置是否没有匹配项 OS生成的计算机。

### <a name="installing-msix-packaging-tool-driver-fod-on-windows-insider-builds"></a>安装 MSIX 打包工具驱动程序在 Windows 预览体验 FOD 生成

如果遇到安装在 Windows 10 Insider Preview 版本的驱动程序的问题：

- 导航到**设置** > **更新和安全** > **Windows 预览体验计划**。
- 如果看到 Insider Preview 生成设置需要关注的消息，请单击**修复我**按钮以重新登录。 您可能需要转到 Windows 更新页面并检查有更新，才能设置更改生效。
- 尝试再次运行工具以下载 MSIX 打包工具驱动程序。 如果仍然遇到问题，请尝试更改为快速深入了解的航班环，安装最新的 Windows 更新，然后重试。

### <a name="installing-msix-packaging-tool-driver-fod-in-wsus"></a>在 WSUS 中安装驱动程序 MSIX 打包工具 FOD

使用 Windows Server Update Services (WSUS) 的组织必须采取操作来手动安装驱动程序：

- [检查你的 Windows 版本](https://support.microsoft.com/help/13443/windows-which-operating-system)。 您必须至少是在 Windows 10，版本 1809年 （内部 17763）。
- 下载的 FOD.cab 文件[Windows 10，版本 1809年](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab)。
- 可以使用安装单独获取需 (FOD) 包的功能[DISM 命令行选项](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options)。 在提升的 PowerShell 窗口中键入： ```Dism /Online /add-package /packagepath:(path)```

IT 管理员还可以创建[并排功能存储区 （共享文件夹）](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server)以允许访问 MSIX 打包工具驱动程序 FOD。 可以找到更多详细信息，在底部[博客文章](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404)。

否则，如果你有权访问企业版或 OEM 通道可以获取该驱动程序从 Windows 10 功能上来自以下源之一的需媒体：

- [批量许可服务中心 (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx):批量许可证访问是必需的。
- [OEM 门户](https://www.microsoftoem.com):OEM 访问是必需的。
- [MSDN 下载](https://my.visualstudio.com/Downloads/Featured):MSDN 订阅是必需的。

可使用 DISM 命令行选项安装单独获取按需包的功能。

## <a name="other-known-issues"></a>其他已知问题

- 不支持在应用程序安装期间重启计算机。 如有可能忽略的重新启动请求或将参数传递给安装程序不需要重新启动。
- 安装程序可能需要特定框架或在安装之前安装驱动程序。 若要查找的框架和用于将应用转换的计算机上的驱动程序，请使用以下查询：```driverquery /v | Out-File```或 ```driverquery /v | Out-File "path to text file"```

# <a name="troubleshooting"></a>疑难解答

## <a name="log-files"></a>日志文件

指示在转换成功，会为每个转换生成日志文件。 它们可在此处找到： 

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

失败代码编写，并在转换过程指示失败的任何点。 错误代码是为了用户友好。

### <a name="log-files-from-remote-devices-or-vms"></a>远程设备或虚拟机中的日志文件

如果在远程设备或虚拟机上执行转换，我们建议你复制日志文件从该设备，并将它们作为反馈项目的一部分附加。 这将有助于我们诊断并更有效地解决问题。 

您将找到远程转换生成的日志： `%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

它将甚至更有利，如果可以共享将包含本地客户端与服务器还远程操作发生的整个日志文件夹。

## <a name="examples-of-failures-during-conversions"></a>转换过程中失败的示例

### <a name="uninstallation-error---exit-code-259"></a>卸载错误-退出代码 259

某些安装程序可能会失败，退出代码为 259 转换。 这指示安装程序生成一个线程的并且不会等待其完成。 换而言之，在主线程完成安装，但它已退出，错误 259 因为生成仍在运行的线程。 我们建议对 setup.exe 使用恰当的安装选项。

### <a name="internal-warning-messages"></a>内部警告消息

如可能遇到的警告消息： **[警告] W_COM_PUBFAIL_INPROC_SERVER_NOT_SUPPORTED**。
这表示 MSIX 打包工具检测到不立即具有与 MSIX COM 注册。 如果将转换应用且其工作原理，很可能这些是不必要的 COM 条目。 但是，如果您看到缺少行为，这意味着在转换过程中未捕获此 COM 行为。

## <a name="sending-feedback"></a>发送反馈

您的反馈的最佳方法是通过**反馈中心**。
1. 打开**反馈中心**或键入**Windows + F**。
2. 提供一个标题和必要的步骤以重现问题。
3. 下**类别**，选择**应用**，然后选择**MSIX 打包工具**。
4. 将任何附加[日志文件](#log-files)与转换相关联。 可以在上面提供的文件夹中找到日志。
5. （如果可能） 将附加转换后的 MSIX 包。
6. 单击“提交”  。

您可以还向我们发送反馈直接从 MSIX 打包工具通过转到**反馈**选项卡上的**设置**。 

> [!NOTE]
> 可能需要 24 小时后，你的反馈，我们获取。 因此如果使用 VM 来转换你的包，您可能想要转换后的 24 小时内保持 VM 上并处于其当前状态。 
