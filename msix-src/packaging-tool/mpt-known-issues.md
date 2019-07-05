---
title: 已知问题和疑难解答
description: 描述 MSIX 打包工具的已知问题并提供故障排除提示。
author: dianmsft
ms.author: diahar
ms.date: 02/26/2019
ms.topic: article
keywords: msix 打包工具, 已知问题, 故障排除
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6da45ad324e55dfbe3a95ec76993f28a38380da6
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555629"
---
# <a name="known-issues-and-troubleshooting"></a>已知问题和疑难解答

本文描述使用 MSIX 打包工具将应用转换为 MSIX 时存在的已知问题，并提供故障排除提示供用户参考。

## <a name="known-issues"></a>已知问题

### <a name="msix-packaging-tool-driver-considerations"></a>MSIX 打包工具驱动程序注意事项

MSIX 打包工具驱动程序以[按需功能 (FOD)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) 的形式通过 Windows 更新提供。如果计算机上已禁用 Windows 更新服务，或者 Windows Insider 外部测试环设置与计算机的 OS 内部版本不匹配，则无法安装该驱动程序。

### <a name="installing-msix-packaging-tool-driver-fod-on-windows-insider-builds"></a>在 Windows 预览体验内部版本上安装 MSIX 打包工具驱动程序 FOD

如果在 Windows 10 Insider Preview 内部版本上安装该驱动程序时遇到问题：

- 导航到“设置” > “更新和安全性” > “Windows 预览体验计划”。   
- 如果有消息指出 Insider Preview 内部版本 设置需要关注，请单击“修复”按钮以重新登录。  在设置更改生效之前，可能需要转到“Windows 更新”页并检查更新。
- 尝试再次运行该工具，以下载 MSIX 打包工具驱动程序。 如果仍遇到问题，请尝试将外部测试环更改为“预览体验成员 - 快”，安装最新的 Windows 更新，然后重试。

### <a name="installing-msix-packaging-tool-driver-fod-in-wsus"></a>在 WSUS 中安装 MSIX 打包工具驱动程序 FOD

使用 Windows Server Update Services (WSUS) 的组织必须采取措施来手动安装该驱动程序：

- [检查 Windows 版本](https://support.microsoft.com/help/13443/windows-which-operating-system)。 必须至少使用 Windows 10 版本 1809（内部版本 17763）。
- 下载适用于 [Windows 10 版本 1809](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) 的 FOD .cab 文件。
- 可以使用 [DISM 命令行选项](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options)安装单独获取的按需功能 (FOD) 包。 在权限提升的 PowerShell 窗口中键入：```Dism /Online /add-package /packagepath:(path)```

IT 管理员还可以创建[并列功能存储（共享文件夹）](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server)，以允许访问 MSIX 打包工具驱动程序 FOD。 可在此[博客文章](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404)的末尾找到更多详细信息。

如果你有权访问企业渠道或 OEM 渠道，则可以通过以下来源之一，从 Windows 10 按需功能媒体获取该驱动程序：

- [批量许可服务中心 (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx)：需要批量许可证访问权限。
- [OEM 门户](https://www.microsoftoem.com)：需要 OEM 访问权限。
- [MSDN 下载](https://my.visualstudio.com/Downloads/Featured)：需要 MSDN 订阅。

可以使用 DISM 命令行选项安装单独获取的按需功能包。

## <a name="other-known-issues"></a>其他已知问题

- 不支持在应用程序安装期间重启计算机。 如果可能，请忽略重启请求，或者向安装程序传递一个参数，用于指定不需要重启。
- 安装程序在安装之前，可能要求安装特定的框架或驱动程序。 若要在用于转换应用的计算机上查找框架和驱动程序，请使用以下查询：```driverquery /v | Out-File``` 或 ```driverquery /v | Out-File "path to text file"```
- 在转换期间，安装程序可能会运行服务。 在转换期间不会捕获服务。 因此，应用可能会安装，但可能会在运行时出现问题。

# <a name="troubleshooting"></a>疑难解答

## <a name="log-files"></a>日志文件

不管转换是否成功，每次转换都会生成日志文件。 可在以下位置找到这些日志文件： 

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

将会写入故障代码，指示转换期间出现的任何故障点。 错误代码易于理解。

### <a name="log-files-from-remote-devices-or-vms"></a>来自远程设备或 VM 的日志文件

如果在远程设备或 VM 上执行转换，我们建议复制该设备提供的日志文件，并将其附加为反馈项的一部分。 这有助于我们更有效地诊断和解决问题。 

可在以下位置找到远程转换生成的日志：`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

如果可以共享包含本地客户端以及远程服务器上发生的操作的整个 Logs 文件夹，则可为我们提供更大的便利。

## <a name="examples-of-failures-during-conversions"></a>转换期间发生的失败示例

### <a name="uninstallation-error---exit-code-259"></a>卸载错误 - 退出代码 259

某些安装程序可能会转换失败并出现退出代码 259。 这表示安装程序衍生了一个线程，但未等待该线程完成。 换而言之，主线程已完成安装，但它衍生了一个仍在运行的线程，因此退出并返回了错误 259。 我们建议对 setup.exe 使用适当的安装选项。

### <a name="internal-warning-messages"></a>内部警告消息

你可能会遇到类似于“[警告] W_COM_PUBFAIL_INPROC_SERVER_NOT_SUPPORTED”的警告消息。 
这表示 MSIX 打包工具检测到一个目前不与 MSIX 匹配的 COM 注册。 如果你可以正常转换应用，则有可能这些 COM 条目是不必要的。 但是，如果你看到缺失的行为，则表示在转换期间未捕获到此 COM 行为。

## <a name="sending-feedback"></a>发送反馈

发送反馈的最适当方式是通过**反馈中心**。
1. 打开“反馈中心”或按 **Windows 键 + F**。 
2. 提供标题和必要的步骤以重现问题。
3. 在“类别”下选择“应用”，然后选择“MSIX 打包工具”。   
4. 附加与转换相关的任何[日志文件](#log-files)。 可以在上面提供的文件夹中找到日志。
5. 附加转换的 MSIX 包（如果可能）。
6. 单击“提交”  。

也可以直接在 MSIX 打包工具中向我们发送反馈，方法是转到“设置”下的“反馈”选项卡。   

> [!NOTE]
> 可能需要等待 24 小时，我们才能收到你的反馈。 因此，如果使用 VM 来转换包，可能需要在转换后，持续打开 VM 并使其保持当前状态 24 小时。 
