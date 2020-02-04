---
description: 本文提供了有关如何运行、调试和测试打包的桌面应用程序以使其可供部署的指南。
title: 运行、调试和测试打包的桌面应用（桌面桥）
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 7f84ce726518eee6e916ad7e7c73e236451a4fcd
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723344"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>运行、调试和测试打包的桌面应用程序

运行打包的应用程序并查看其外观，而无需对其进行签名。 然后，设置断点并单步执行代码。 准备好在生产环境中测试应用程序时，请对应用程序进行签名，然后再进行安装。 本主题演示如何执行以下各项操作。

<a id="run-app" />

## <a name="run-your-application"></a>运行应用程序

您可以运行应用程序以在本地测试它，而无需获取证书并对其进行签名。 运行应用程序的方式取决于用于创建包的工具。

### <a name="you-created-the-package-by-using-visual-studio"></a>使用 Visual Studio 创建程序包

将打包项目设置为启动项目，然后按 CTRL+F5 启动应用。

### <a name="you-created-the-package-using-a-different-tool"></a>使用其他工具创建了包

打开 Windows PowerShell 命令提示符，并从包文件的根目录中运行此 cmdlet：

```
Add-AppxPackage –Register AppxManifest.xml
```
要启动应用，请在 Windows“开始”菜单中找到它。

!["开始" 菜单中的打包应用程序](images/converted-app-installed.png)

> [!NOTE]
> 打包的应用程序始终以交互用户身份运行，安装打包应用程序的任何驱动器都必须设置为 NTFS 格式。

## <a name="debug-your-app"></a>调试你的应用

调试应用程序的方式取决于用于创建包的工具。

如果你使用 Visual Studio 2017 版本15.4 及更高版本（包括 Visual Studio 2019）中提供的[新打包项目](desktop-to-uwp-packaging-dot-net.md#new-packaging-project)创建了包，只需将打包项目设置为启动项目，然后按 F5 调试应用。

如果使用任何其他工具创建了包，请执行以下步骤：

1. 请确保至少启动一次打包应用程序，以便将其安装在本地计算机上。

   请参阅上述[运行应用](#run-app)部分。

2. 启动 Visual Studio。

   如果要使用提升的权限调试应用程序，请使用 "以**管理员身份运行**" 选项启动 Visual Studio。

3. 在 Visual Studio 中，选择**调试**->**其他调试目标**->**调试安装的应用程序包**。

4. 在**安装的应用程序包**列表中，选择你的应用包，然后选择**附加**按钮。

#### <a name="modify-your-application-in-between-debug-sessions"></a>在调试会话之间修改应用程序

如果更改应用程序以修复 bug，请使用 Makeappx.exe 工具将其重新打包。 请参阅[运行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

### <a name="debug-the-entire-application-lifecycle"></a>调试整个应用程序生命周期

在某些情况下，您可能需要对调试过程进行更精细的控制，包括在应用程序启动之前对应用程序进行调试的功能。

你可以使用[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)来完全控制应用程序生命周期，包括挂起、继续和终止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 包含在 Windows SDK 中。

## <a name="test-your-app"></a>测试应用程序

若要在准备分发时将打包应用程序部署到端到端生产测试，需要使用要部署应用的计算机上受信任的证书对包进行签名。

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>测试使用 Visual Studio 打包的应用程序

Visual Studio 使用测试证书对应用程序进行签名。 你会在**创建应用包**向导生成的输出文件夹中找到该证书。 证书文件的扩展名为 *.cer* ，你必须将该证书安装到你想要在其上测试应用程序的电脑上的 "**受信任人**" 证书存储中。 请参阅[旁加载包](../package/packaging-uwp-apps.md#sideload-your-app-package)。

### <a name="test-an-application-that-you-packaged-using-a-different-tool"></a>测试使用其他工具打包的应用程序

如果将应用程序打包到 Visual Studio 之外，则可以使用签名工具对应用程序包进行签名。 如果你在进行测试的计算机上未信任用于签名的证书，则在安装应用程序包之前，你需要将 "证书" 安装到 "受信任人" 证书存储。 

#### <a name="sign-your-application-package"></a>对应用程序包进行签名

手动对应用程序包进行签名：

1. 创建证书。 请参阅[创建证书](../package/create-certificate-package-signing.md)。

2. 将该证书安装到系统上的 "**受信任人**" 证书存储中。

3. 使用证书对应用程序进行签名，请参阅[使用 SignTool 对应用包进行签名](../package/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 确保证书上的发布者名称与应用的发布者名称匹配。

**相关示例**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>测试适用于 Windows 10 S 的应用程序

在发布你的应用程序之前，请确保它将在运行 Windows 10 S 的设备上正常运行。事实上，如果你计划将应用程序发布到 Microsoft Store，则必须执行此操作，因为它是存储要求。 无法在运行 Windows 10 S 的设备上正常运行的应用将不会通过认证。

请参阅[测试适用于 windows 10 的 windows 应用程序](desktop-to-uwp-test-windows-s.md)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器内运行另一个进程

你可以在指定应用包的容器内调用自定义进程。 这对于测试方案（例如，如果你有自定义测试工具并想要测试应用的输出）很有用。 若要执行此操作，请使用 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>后续步骤

有问题？ 咨询[.Msix 技术社区](https://techcommunity.microsoft.com/t5/msix/ct-p/MSIX)。
