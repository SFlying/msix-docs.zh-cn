---
title: Windows 10 版本 1709年及更高版本上 MSIX 程序包支持
description: 安装 MSIX 包 1709年及更高版本。
author: c-don
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX、 MPT、 MSIX 打包工具，1709年版本 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bb0a4aaab44f9eed791200c3d31b04dbfe242ebf
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400683"
---
# <a name="msix-package-support-on-windows-10-version-1709-and-later"></a>Windows 10 版本 1709年及更高版本上 MSIX 程序包支持

如果现有应用程序转换为 MSIX，可能想要在 Windows 10，版本 1809年之前的操作系统的版本 （版本 17763） 中使用应用程序。 本文介绍如何在 Windows 10 版本 1709年 （内部版本 16299） 中支持 MSIX 包及更高版本。

## <a name="problem"></a>问题

因此到 MSIX 使用 MSIX 打包工具转换现有的应用和 Windows 10，版本 1809年及更高版本上运行良好。 但既然您知道我们将添加 MSIX 开始生成 Windows 10 版本 1709年的支持，你想要在此或任何更高版本的 Windows 10 上运行 MSIX 应用。 目前，如果只是尝试使用 Windows 10 1709年版或 Windows 10 版本 1803年的计算机上安装您 MSIX 的程序包，您将收到此错误消息：

![PowerShell MSIX 安装](images/mpt_blog_0.jpg)

## <a name="solution"></a>解决方案

我们正在努力更新 MSIX 打包工具来处理此操作，但在此期间下面是你可以如何将您的应用程序设置为在这些版本上运行。

1. 打开 MSIX 打包工具，然后单击**包编辑器**。
  ![open](images/mpt_blog_1.jpg)

2. 浏览到 MSIX 包，然后单击**打开程序包**。
  ![打开包](images/mpt_blog_3.jpg)

3. 在底部**包信息**选项卡上，在**清单文件**，单击**打开的文件**。
  ![打开清单](images/mpt_blog_4.jpg)

4. 在清单文件中，编辑**MinVersion**等于版本 16299，如下所示。
  ![edit manifest2](images/mpt_blog_7.jpg)

5. 完成后编辑清单，关闭文件。 这将使你返回到**包编辑器**。 不要忘记重新编辑对包进行签名，如下所示：![登录](images/mpt_blog_9.jpg)

6. 已完成更新后，保存所做的更改。
  ![save](images/mpt_blog_10.jpg)

此时，您可以在运行 Windows 10 版本 1709年或更高版本的计算机上安装应用。

## <a name="troubleshooting-tips"></a>疑难解答提示

现在，在运行 Windows 10 版本 1709 （内部版本 16299） 来构建 17700，计算机上你可以安装 MSIX 应用通过 PowerShell。
具体而言，需要在 PowerShell 中的此命令：

```powershell
add-appxpackage <path to MSIX package>
```

![PowerShell 命令](images/mpt_blog_11.jpg)

此外可以使用 Intune、 SCCM 或打包管理器 API。