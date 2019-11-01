---
title: Windows 10 版 1709 和更高版本中的 MSIX 包支持
description: 本文介绍如何在 Windows 10 版本1709（版本16299）和更高版本中支持 .MSIX 包。
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX 打包工具, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4dbb6fdb5893835475d79a8052e35fc902680425
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328754"
---
# <a name="msix-package-support-on-windows-10-version-1709-and-later"></a>Windows 10 版 1709 及更高版本上的 MSIX 包支持

将现有的应用转换为 MSIX 后，你可能想要在 Windows 10 版本 1809（内部版本 17763）以前的 OS 版本中使用该应用。 本文介绍如何在 Windows 10 版本 1709（内部版本 16299）和更高版本中支持 MSIX 包。

## <a name="msix-packaging-tool-version-120197010-and-later"></a>.MSIX 打包工具版本1.2019.701.0 及更高版本

使用 .MSIX 打包工具在版本1.2019.701.0 和更高版本上转换应用时，打包工具可以选择将最低版本设置为1709。  可以通过设置以下工具来启用此功能。

1. 启动 .MSIX 打包工具

2. 单击右上角的 "**设置**" 齿轮

3. 在导航窗格中选择 "**工具默认值**"

4. 取消选中 "**强制 Microsoft Store 版本控制要求**

5. 单击 "**保存设置**"

这会将最低版本设置为 Windows 1709 以供将来转换。  在包编辑器中编辑包时，它不会更改最低版本。  


## <a name="problem"></a>问题

使用 .MSIX 打包工具在1.2019.701.0 之前，将现有应用转换为 .MSIX，并强制实施 Microsoft Store 版本控制 requiremnts，或使用其他工具创建未将最低版本设置为10.0.16299.0 的包（Windows 10 1709 build数字）。  需要在 Windows 10 1709 或任何更高版本的 Windows 10 上运行 .MSIX 应用。 目前，如果你尝试在装有 Windows 10 版本 1709 或 Windows 10 版本 1803 的计算机上安装 MSIX 包，将会收到以下错误消息：

![PowerShell MSIX 安装](images/mpt_blog_0.jpg)

## <a name="solution"></a>解决方案

我们正在努力更新 MSIX 打包工具以处理此错误，但在此期间，可以采取以下措施将应用设置为在这些版本中运行。

1. 打开 MSIX 打包工具并单击“包编辑器”。
  ![open](images/mpt_blog_1.jpg)

2. 浏览到你的 MSIX 包，然后单击“打开包”。
  ![打开包](images/mpt_blog_3.jpg)

3. 在“包信息”选项卡底部的“清单文件”下，单击“打开文件”。
  ![打开清单](images/mpt_blog_4.jpg)

4. 在清单文件中，编辑 **MinVersion** 使其等于 16299，如下所示。
  ![编辑清单 2](images/mpt_blog_7.jpg)

5. 编辑完清单后，关闭该文件。 随后将返回到“包编辑器”。 请不要忘记按如下所示重新为编辑后的包签名：![签名](images/mpt_blog_9.jpg)

6. 完成更新后，保存所做的更改。
  ![保存](images/mpt_blog_10.jpg)

现在，便可以在运行 Windows 10 版本 1709 或更高版本的计算机上安装该应用了。

## <a name="troubleshooting-tips"></a>疑难解答提示

目前，在运行 Windows 10 版本 1709 （内部版本 16299 至 17700）的计算机上，可以通过 PowerShell 安装 MSIX 应用。
具体而言，需要在 PowerShell 中运行以下命令：

```powershell
add-appxpackage <path to MSIX package>
```

![PowerShell 命令](images/mpt_blog_11.jpg)

也可以使用 Intune、SCCM 或打包管理器 API。
