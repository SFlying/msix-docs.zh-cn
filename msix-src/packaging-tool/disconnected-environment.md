---
title: 在断开连接的环境中使用 MSIX 打包工具
description: 本文介绍如何获取 .MSIX 打包工具在断开连接的环境中所需的所有资产。
ms.date: 02/05/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
ms.openlocfilehash: 7387efe58438fe6781fdfb1ad01b2a15087940e5
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090855"
---
# <a name="using-the-msix-packaging-tool-in-a-disconnected-environment"></a>在断开连接的环境中使用 MSIX 打包工具

虽然我们使用户能够非常轻松地通过 Microsoft store 获取 .MSIX 打包工具，但我们知道，并非每个人都有商店或连接环境，他们想要执行转换。 此功能仅适用于公共版本，不适用于我们的 [预览体验计划](insider-program.md) 版本。

## <a name="get-the-msix-packaging-tool"></a>获取 MSIX 打包工具

可以从适用于企业的 Microsoft Store [Web 门户](https://businessstore.microsoft.com/store)下载 MSIX 打包工具供脱机使用。 可在[此处](/microsoft-store/distribute-offline-apps)详细了解离线分发。

如果在使用打包工具的脱机副本时遇到问题，请确保你有该工具的[许可证脱机副本](/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)。 

有了应用程序的脱机版本后，可以使用 [PowerShell](/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) 将应用包和许可证添加到计算机。

#### <a name="example-of-offline-installation"></a>脱机安装的示例
```
PS C:\> Add-AppxProvisionedPackage -Path C:\offline -PackagePath C:\MSIX\MyPackage.msix -LicensePath C:\MSIX\MyLicense.xml
```

## <a name="get-the-msix-packaging-tool-driver"></a>获取 .MSIX 打包工具驱动程序

.MSIX 打包工具驱动程序将作为 [按需 (FOD) ](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) 包的功能提供 Windows 更新，如果在计算机上禁用了 Windows 更新服务或 Windows 预览体验航班环设置与计算机的操作系统版本不匹配，则将无法安装。

如果你所在的企业环境中 Windows Server Update Services (WSUS) 或 Systems Center (现在为 Microsoft 端点管理器) 则可能需要修改 [默认配置](/windows/deployment/update/fod-and-lang-packs)，或者只需手动下载并安装 FOD：

- 下载适用于[Windows 10 版本1809、x64](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab)或[windows 10，版本1809，x86](https://download.microsoft.com/download/9/9/4/9948d09d-af25-45a5-b01f-cc4bcf05f5bf/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)的 FOD 文件
- 下载适用于 [windows 10 版本1903、x64](https://download.microsoft.com/download/5/2/e/52ec35e9-3b50-47b2-879d-c815a93bc3fc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) 或 [windows 10 版本1903的 FOD 文件，X86](https://download.microsoft.com/download/2/c/3/2c3a78a2-4d64-426a-976d-dfe4805110cc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab) **说明：这也适用于 Windows 10，版本 1909**
- 可以使用 [DISM 命令行选项](/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options)安装单独获取的按需功能 (FOD) 包。 在权限提升的 PowerShell 窗口中键入：```Dism /Online /add-package /packagepath:(path)```

IT 管理员还可以创建[并列功能存储（共享文件夹）](/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server)，以允许访问 MSIX 打包工具驱动程序 FOD。 可在此[博客文章](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404)的末尾找到更多详细信息。

如果你有权访问企业渠道或 OEM 渠道，则可以通过以下来源之一，从 Windows 10 按需功能媒体获取该驱动程序：

- [批量许可服务中心 (VLSC) ](https://www.microsoft.com/Licensing/servicecenter/default.aspx)：需要批量许可证访问。
- [Oem 门户](https://www.microsoftoem.com)：需要 oem 访问权限。
- [Msdn 下载](https://my.visualstudio.com/Downloads/Featured)：需要 msdn 订阅。