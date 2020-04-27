---
description: 本文提供有关如何运行、调试和测试打包的桌面应用程序，使其做好部署准备的指导。
title: 运行、调试和测试打包的桌面应用（桌面桥）
ms.date: 02/03/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: f778c51afe1e25bbadca48dc2d9644e8645bdde3
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77072807"
---
# <a name="run-debug-and-test-an-msix-package"></a>运行、调试和测试 MSIX 包

运行打包的应用程序并查看其状况，无需为其签名。 然后，设置断点并单步执行代码。 准备好在生产环境中测试应用程序后，为应用程序签名，然后安装它。 本主题介绍如何执行以下各项操作。

<a id="run-app" />

## <a name="run-your-application"></a>运行应用程序

可运行应用程序以在本地进行测试，无需获得证书并为其签名。 运行应用程序的方式取决于用于创建包的工具。

### <a name="you-created-the-package-by-using-visual-studio"></a>包是使用 Visual Studio 创建的

将打包项目设置为启动项目，然后按 F5 启动应用。

### <a name="you-created-the-package-using-a-different-tool"></a>包是使用其他工具创建的

打开 Windows PowerShell 命令提示符，从包文件的根目录运行以下 cmdlet：

```
Add-AppxPackage –Register AppxManifest.xml
```
若要启动应用，请在 Windows“开始”菜单中找到它。

![开始菜单中打包的应用程序](images/converted-app-installed.png)

> [!NOTE]
> 打包的应用程序始终以交互用户的身份运行，任何安装已打包应用程序的驱动器必须以 NTFS 格式化。

## <a name="debug-your-app"></a>调试应用

调试应用程序的方式取决于用于创建包的工具。

如果包是使用 Visual Studio 2017 版本 15.4 和更高版本（包括 Visual Studio 2019）中提供的[新建打包项目](desktop-to-uwp-packaging-dot-net.md#new-packaging-project)创建的，则只需将打包项目设置为启动项目，然后按 F5 即可调试应用。

如果包是使用任何其他工具创建的，请执行以下步骤：

1. 确保至少启动打包的应用程序一次，以将其安装在本地计算机上。

   参阅前面的[运行应用](#run-app)部分。

2. 启动 Visual Studio。

   若要使用提升的权限调试应用程序，请使用“以管理员身份运行”选项启动 Visual Studio。 

3. 在 Visual Studio 中，选择“调试” **“其他调试目标”** “调试安装的应用包”。->  -> 

4. 在“安装的应用包”列表中选择你的应用包，然后选择“附加”按钮。  

#### <a name="modify-your-application-in-between-debug-sessions"></a>在不同的调试会话中修改应用程序

如果更改应用程序来修复 bug，请使用 MakeAppx 工具重新打包。 参阅[运行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

### <a name="debug-the-entire-application-lifecycle"></a>调试整个应用程序生命周期

在某些情况下，你可能希望更精细地控制调试过程，包括能够在应用程序启动前进行调试。

可以使用 [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 获取对应用程序生命周期的完全控制，包括暂停、恢复和终止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 已随附在 Windows SDK 中。

## <a name="test-your-app"></a>测试应用

若要部署打包的应用程序以在准备分发时进行端到端的生产测试，需要使用一个在部署应用的计算机上受信任的证书为包签名。

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>测试使用 Visual Studio 打包的应用程序

Visual Studio 使用测试证书来为应用程序签名。 可以在“创建应用包”向导生成的输出文件夹中找到该证书。  证书文件具有 *.cer* 扩展名，必须将该证书安装到用于测试应用程序的电脑上的“受信任人”证书存储中。  参阅[在 Visual Studio 中打包桌面或 UWP 应用](../package/packaging-uwp-apps.md#generate-an-app-package)。

### <a name="test-an-application-that-you-packaged-using-a-different-tool"></a>测试使用其他工具打包的应用程序

如果在 Visual Studio 外部打包了应用程序，可以使用签名工具来为应用程序包签名。 如果用于签名的证书在测试计算机上不受信任，则在安装应用包之前，需将该证书安装到“受信任人”证书存储中。 

#### <a name="sign-your-application-package"></a>为应用程序包签名

若要手动为应用程序包签名：

1. 创建证书。 参阅[创建证书](../package/create-certificate-package-signing.md)。

2. 将该证书安装到系统上的“受信任人”证书存储中。 

3. 使用该证书为应用程序签名，具体请参阅[使用 SignTool 为应用包签名](../package/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 确保证书上的发布者名称与应用的发布者名称匹配。

**相关示例**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>测试适用于 Windows 10 S 的应用程序

在发布应用之前，请确保它可以在运行 Windows 10 S 的设备上正常运行。实际上，如果你打算将应用程序发布到 Microsoft Store，则必须执行此操作，因为这是 Store 的一项要求。 无法在运行 Windows 10 S 的设备上正常运行的应用将不会通过认证。

请参阅[测试适用于 Windows 10 S 的 Windows 应用程序](desktop-to-uwp-test-windows-s.md)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器内运行另一个进程

你可以在指定应用包的容器内调用自定义进程。 这对于测试方案（例如，如果你有自定义测试工具并想要测试应用的输出）很有用。 若要执行此操作，请使用 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>后续步骤

有问题？ 请在 [MSIX 技术社区](https://techcommunity.microsoft.com/t5/msix/ct-p/MSIX)提问。
