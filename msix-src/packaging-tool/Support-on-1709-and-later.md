---
title: Windows 10 版 1709 和更高版本中的 MSIX 包支持
description: 在 1709 和更高版本中安装 MSIX 包。
author: c-don
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX 打包工具, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bb0a4aaab44f9eed791200c3d31b04dbfe242ebf
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "66400683"
---
# <a name="msix-package-support-on-windows-10-version-1709-and-later"></a>Windows 10 版 1709 及更高版本上的 MSIX 包支持

将现有的应用转换为 MSIX 后，你可能想要在 Windows 10 版本 1809（内部版本 17763）以前的 OS 版本中使用该应用。 本文介绍如何在 Windows 10 版本 1709（内部版本 16299）和更高版本中支持 MSIX 包。

## <a name="problem"></a>问题

你已使用 MSIX 打包工具将现有的应用转换为 MSIX，并且该应用可在 Windows 10 版本 1809 和更高版本中正常运行。 但是，既然你已知道我们即将从 Windows 10 版本 1709 开始添加对 MSIX 的支持，你希望在此版本或更高版本的 Windows 10 上运行 MSIX 应用。 目前，如果你尝试在装有 Windows 10 版本 1709 或 Windows 10 版本 1803 的计算机上安装 MSIX 包，将会收到以下错误消息：

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

5. 编辑完清单后，关闭该文件。 随后将返回到“包编辑器”。  请不要忘记按如下所示重新为编辑后的包签名：![签名](images/mpt_blog_9.jpg)

6. 完成更新后，保存所做的更改。
  ![保存](images/mpt_blog_10.jpg)

现在，便可以在运行 Windows 10 版本 1709 或更高版本的计算机上安装该应用了。

## <a name="troubleshooting-tips"></a>故障排除提示

目前，在运行 Windows 10 版本 1709 （内部版本 16299 至 17700）的计算机上，可以通过 PowerShell 安装 MSIX 应用。
具体而言，需要在 PowerShell 中运行以下命令：

```powershell
add-appxpackage <path to MSIX package>
```

![PowerShell 命令](images/mpt_blog_11.jpg)

也可以使用 Intune、SCCM 或打包管理器 API。