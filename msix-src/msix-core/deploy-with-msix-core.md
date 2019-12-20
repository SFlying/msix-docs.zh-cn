---
title: 使用 .MSIX Core 部署 .MSIX 包
description: 介绍如何使用 msixmgr 工具通过 .MSIX Core 部署 .MSIX 包。
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 98ca9fce99139608da8e59108d68756717c605ae
ms.sourcegitcommit: 0412ba69187ce791c16313d0109a5d896141d44c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303353"
---
# <a name="deploy-an-msix-package-with-msix-core"></a>使用 .MSIX Core 部署 .MSIX 包

[.Msix Core](msixcore.md)引入了 .msix 部署来选择以前版本的 Windows。 首先，请确保已在目标设备上安装 .MSIX Core。

## <a name="msi-installation"></a>MSI 安装

建议使用我们提供的 MSI 安装程序安装 .MSIX Core，因为它们会自动将 msixmgr 添加到搜索路径，并将 .MSIX 扩展与安装程序相关联。

你可以从我们的 "[发布" 页](https://github.com/microsoft/msix-packaging/releases)上的 "**资产**" 部分下载以下特定于体系结构的 MSI 安装程序：

* **msixmgrSetup-x64**
* **msixmgrSetup-86**

> [!NOTE]
> 确保为设备的体系结构选择了正确的安装程序。 这会影响安装程序将存储重要文件的位置。 根据安装程序的版本，文件的名称可能会更改。

## <a name="installing-your-certificate"></a>安装证书

需要对 .MSIX 包进行签名。 安装任何 .MSIX 包之前，请确保已安装用于对包进行签名的证书。 你可以使用普通工作流从管理工具中安装证书来实现此目的。

如果要手动安装证书，可以从提升的命令提示符运行以下命令：

```PowerShell
certutil -addstore root <insert certificate.cert>
```

> [!NOTE]
> 在所有情况下，应在 "受信任的根证书颁发机构" 下添加受信任的证书。

## <a name="using-the-command-line"></a>使用命令行

安装工具 msixmgr 后，可通过搜索、安装和删除在此计算机上管理 .MSIX 包。 命令行实用工具 msixmgr 适用于系统管理员。 在从管理员提示符运行时，它最有用。 从常规命令提示符运行时，不是所有的命令都会显示在控制台上。 有关详细信息，请参阅下方。

### <a name="install"></a>“安装”

使用命令提示符或 PowerShell，导航到包含 msixmgr 的目录，并运行以下命令以安装 .MSIX 包。 也可以将 `-quietUX` 参数添加到命令的末尾，以使用户看不到安装程序 UI。

```PowerShell
msixmgr.exe -AddPackage C:\NotePadPlus\notepadplus.msix -quietUX
```

> [!NOTE]
> 这种情况下，下面的示例使用**notepadplus. .msix**。 这是我们的[示例包](https://github.com/microsoft/msix-packaging/tree/master/MsixCore/Tests)之一。

### <a name="querying-for-a-specific-msix-package"></a>查询特定的 .MSIX 包

还可以通过**packageFullName**、 **packageFamilyName**和/或使用通配符搜索特定包。 支持的通配符是 * （匹配任意字符）和？（匹配单个字符）。

```PowerShell
msixmgr.exe -FindPackage notepadplus_0.0.0.1_???__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *epadplus_8wekyb3d8bbw?
```

### <a name="uninstall"></a>卸载

若要卸载，请使用以下命令：

```PowerShell
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe -quietUX
```