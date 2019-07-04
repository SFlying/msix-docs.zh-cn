---
Description: 运行打包的应用并查看其状况，无需对其进行签名。 然后，设置断点并单步执行代码。 准备好在生产环境中测试应用后，对应用进行签名，然后安装它。
title: 运行、调试和测试打包的桌面应用（桌面桥）
ms.date: 08/31/2017
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, uwp, msix
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 3c186fe3baf543a459e9b65003d41ec52f4def2d
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555554"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>运行、 调试和测试打包桌面应用程序

运行打包的应用程序并查看其外观而无需对其进行签名。 然后，设置断点并单步执行代码。 如果你已准备好在生产环境中测试你的应用程序，应用程序进行签名，然后安装它。 本主题演示如何执行以下各项操作。

<a id="run-app" />

## <a name="run-your-application"></a>运行应用程序

你可以运行你的应用程序进行测试本地而无需获取证书并对其进行签名。 运行应用程序的方式取决于什么工具，用于创建包。

### <a name="you-created-the-package-by-using-visual-studio"></a>使用 Visual Studio 创建程序包

将打包项目设置为启动项目，然后按 CTRL+F5 启动应用。

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>你以手动方式或使用 Desktop App Converter 创建程序包

打开 Windows PowerShell 命令提示符，并从输出文件夹的 **PackageFiles** 子文件夹运行此 cmdlet：

```
Add-AppxPackage –Register AppxManifest.xml
```
要启动应用，请在 Windows“开始”菜单中找到它。

![在开始菜单中的打包应用程序](images/converted-app-installed.png)

> [!NOTE]
> 打包的应用程序始终运行以交互式的用户，并安装到打包的应用程序的任何驱动器必须格式化为 NTFS 格式。

## <a name="debug-your-app"></a>调试应用

如何调试应用程序依赖于什么工具，用于创建包。

如果你使用 Visual Studio 2017 15.4 版中提供的[新的打包项目](desktop-to-uwp-packaging-dot-net.md#new-packaging-project)创建了程序包，则只需将打包项目设置为启动项目，然后按 F5 即可调试应用。

如果你使用任何其他工具创建了程序包，请按照以下步骤操作。

1. 请确保启动打包的应用程序至少一次，以便在本地计算机上安装。

   请参阅上述[运行应用](#run-app)部分。

2. 启动 Visual Studio。

   如果你想要调试应用程序使用提升的权限，通过使用启动 Visual Studio**以管理员身份运行**选项。

3. 在 Visual Studio 中，选择**调试**->**其他调试目标**->**调试安装的应用程序包**。

4. 在**安装的应用程序包**列表中，选择你的应用包，然后选择**附加**按钮。

#### <a name="modify-your-application-in-between-debug-sessions"></a>修改你在调试会话期间的应用程序

如果应用程序来修复 bug 进行更改，重新使用 MakeAppx 工具将其打包。 请参阅[运行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

### <a name="debug-the-entire-application-lifecycle"></a>调试整个应用程序生命周期

在某些情况下，可能希望更精细地控制调试过程，包括开始之前调试你的应用程序的能力。

可以使用[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)能够完全控制应用程序生命周期，包括挂起、 恢复和终止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 包含在 Windows SDK 中。

## <a name="test-your-app"></a>测试应用

中实际设置测试你的应用程序，如准备进行分发，它是应用程序进行签名，然后将其安装。

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>测试使用 Visual Studio 打包应用程序

Visual Studio 对您的应用程序通过使用测试证书进行签名。 你会在**创建应用包**向导生成的输出文件夹中找到该证书。 证书文件有 *.cer*扩展，您就会安装到该证书**受信任的根证书颁发机构**将存储在你想要测试你的应用程序上的 PC 上。 请参阅[旁加载包](/windows/uwp/packaging/packaging-uwp-apps.md#sideload-your-app-package)。

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>测试使用 Desktop App Converter (DAC) 打包的应用程序

如果使用 Desktop App Converter 打包你的应用程序，则可以使用``sign``参数自动使用生成的证书签署你的应用程序。 必须安装该证书，然后再安装应用。 请参阅[运行打包的应用](desktop-to-uwp-run-desktop-app-converter.md#run-app)。


### <a name="manually-sign-apps-optional"></a>手动对应用进行签名（可选）

此外可以手动注册你的应用程序。 下面是操作方法

1. 创建证书。 请参阅[创建证书](/windows/uwp/packaging/create-certificate-package-signing.md)。

2. 将该证书安装到系统上**受信任根**或**受信任人**证书存储区中。

3. 通过使用该证书来签署应用程序，请参阅[签名应用程序包使用 SignTool](/windows/uwp/packaging/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 确保证书上的发布者名称与应用的发布者名称匹配。

**相关的示例**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>测试应用程序的 Windows 10 S

发布应用之前，请确保设备上的运行 Windows 10 s。 它也将正常运行事实上，如果你打算发布到 Microsoft Store 应用程序，必须这样做是因为它是存储要求。 无法在运行 Windows 10 S 的设备上正常运行的应用将不会通过认证。

请参阅[测试 Windows 应用程序的 Windows 10 S](desktop-to-uwp-test-windows-s.md)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器内运行另一个进程

你可以在指定应用包的容器内调用自定义进程。 这对于测试方案（例如，如果你有自定义测试工具并想要测试应用的输出）很有用。 若要执行此操作，请使用 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
