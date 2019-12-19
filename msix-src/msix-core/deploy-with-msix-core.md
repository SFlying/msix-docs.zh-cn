---
title: 使用 .MSIX Core 部署 .MSIX 包
description: 本文提供了有关如何利用 .MSIX 核心引导程序的分步说明，该程序使用 ClickOnce 创建一个应用程序，该应用程序将允许用户仅下载 setup.exe 并通过 .MSIX 核心安装程序安装其 .MSIX 应用。
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 03d9c1630ffa9b956cdfd432199500672e18966a
ms.sourcegitcommit: 8aafaa9ac4087ef2e95030343add8fe2ee1cccc9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/18/2019
ms.locfileid: "75186743"
---
# <a name="deploy-an-msix-package-with-msix-core"></a>使用 .MSIX Core 部署 .MSIX 包
如概述中所述，.MSIX Core 将 .MSIX 部署支持引入了 Windows 7 SP1、Windows 8.1、当前支持的 Windows Server （带有桌面体验）以及1709之前的 Windows 10 版本（秋季更新）。 要开始，必须确保在操作系统上安装 .MSIX Core。

为方便起见，我们提供了 MSI 安装程序。 若要查找所选的安装程序，请切换到 "[发布" 页](https://github.com/microsoft/msix-packaging/releases)，并在 "**资产**" 下找到以下内容：

1. msixmgrSetup-x64
2. msixmgrSetup-86

## <a name="msi-installation"></a>MSI 安装 
建议安装 MSI，因为它会自动将 msixmgr 添加到搜索路径，并将 .MSIX 扩展与安装程序相关联。

下载 " **msixmgrSetup-x64** " 或 " **msixmgrSetup-x86** " （具体取决于你的体系结构），并将其安装在 Windows 设备上。 

> [!NOTE]
> 为您的体系结构选取正确的安装程序十分重要。 这会影响安装程序将存储重要文件的位置。 根据安装程序的版本，文件的名称可能会更改。 

## <a name="installing-your-certificate"></a>安装证书
需要对 .MSIX 包进行签名。 安装任何 .msix 包之前，请确保已安装用于对包进行签名的证书。 你可以使用普通工作流从管理工具中安装证书来实现此目的。 

如果要手动安装证书，可以从提升的命令提示符运行以下命令： 
```
certutil -addstore root <insert certificate.cert>
```
> [!NOTE]
> 在所有情况下，应在 "受信任的根证书颁发机构" 下添加受信任的证书。

## <a name="using-the-command-line"></a>使用命令行
安装工具 msixmgr 后，可通过搜索、安装和删除在此计算机上管理 .MSIX 包。 命令行实用工具 msixmgr 适用于系统管理员。 从管理提示符运行时，它最有用。 从常规命令提示符运行时，不是所有的命令都会显示在控制台上。 有关详细信息，请参阅下方。

### <a name="install"></a>“安装”
使用命令提示符或 PowerShell，导航到包含可执行文件的目录，并运行以下命令以安装 notepadplus. .msix。 还可以将-quietUX 参数添加到命令的末尾，以使用户看不到安装程序 UI。 例如： 
```
msixmgr.exe -AddPackage C:\SomeDirectory\notepadplus.msix -quietUX
```
### <a name="querying-for-a-specific-msix-package"></a>查询特定的 .MSIX 包
还可以通过 packageFullName、packageFamilyName 和/或使用通配符搜索特定包。 支持的通配符是 * （匹配任意字符）和？（匹配单个字符）。 
```
msixmgr.exe -FindPackage notepadplus_0.0.0.1_???__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *epadplus_8wekyb3d8bbw?
```
### <a name="uninstall"></a>卸载
若要卸载，请使用以下命令： 
```
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe -quietUX
```
> [!NOTE]
> 上面的命令使用**notepadplus。 .msix**是我们的[示例包](https://github.com/microsoft/msix-packaging/tree/master/MsixCore/Tests)之一。