---
description: 本文提供了有关如何从网络共享注册程序包布局的指南
title: 从网络共享注册包布局
ms.date: 02/06/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: ca179a49a282d2bee6217a142ef94409e2161c8d
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091225"
---
# <a name="registering-a-package-layout-from-a-network-share"></a>从网络共享注册包布局

由于需要复制程序包、存储库、生成文件夹等，协作式和多设备开发可能非常耗时。如果你是在 Windows 10 版本 1709 及更高版本上开发，则可以利用某个功能将程序包布局生成为一个网络共享，然后直接通过网络在远程设备上注册该布局。

多人可以参与网络共享上的单个应用程序包布局，其他协作者和注册了该应用程序的用户将可以看到更改。 如果要为多个设备生成应用，可以利用此功能，并避免将应用复制到每个设备进行测试。

## <a name="prerequisites"></a>必备条件

1. 你的设备必须运行 Windows 10 创意者更新预览体验内部版本 14965 或更高版本。

2. 你需要在所有设备上启用开发者模式和设备发现。

3. 协作者需要对生成文件夹具有读取和写入访问权限。

4. 用户只需要对生成文件夹具有读取访问权限。

## <a name="in-visual-studio"></a>在 Visual Studio 中

如果是在 Visual Studio 中进行开发，则可以按照[此处](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#advanced-remote-deployment-options)所述步骤进行操作。

## <a name="from-the-command-line"></a>从命令行中

如果不是在 Visual Studio 中进行开发，并且使用命令行工具，则可以使用 [WinDeployAppCmd](/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool)。 下面是有关如何在命令行窗口中执行此操作的示例：

```
WinAppDeployCmd.exe registerfiles -remotedeploydir <network path> -ip <IP Address> -pin <target machine PIN>
```
- 网络路径 – 应用的松散文件的路径

- IP 地址 – 在此处输入目标计算机的 IP 地址

- PIN - 与目标设备建立连接时所需的 PIN。 （如果需要身份验证，将提示你使用 -pin 选项重试。）单击此处以了解如何获取 PIN。
 

你还可以构建一个完全打包的应用程序，用于在测试和验证期间访问网络共享中的文件。 有关[示例](https://github.com/AppInstaller/Windows-appsample-marble-maze)，请参阅此示例。