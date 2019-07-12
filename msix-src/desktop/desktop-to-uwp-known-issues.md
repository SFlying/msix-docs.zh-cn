---
Description: 本文包含桌面桥的已知问题。
title: 打包桌面应用程序的已知的问题
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: e5abc7f51cd6b34f11c5cf063ecf2d755c56d61b
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828868"
---
# <a name="known-issues-with-packaged-desktop-apps"></a>打包桌面应用程序的已知的问题

本文包含为桌面应用程序创建一个 MSIX 包时可能发生的已知的问题。

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>你收到错误 MSB4018“GenerateResource”任务意外失败

在尝试将卫星集转换为包资源索引 (PRI) 文件时，会发生这种情况。

我们已注意到此问题，正在努力制定长期解决方案。 作为临时解决方法，你可以通过将此行 XML 添加到托管项目文件的第一个 PropertyGroup 元素中来禁用资源生成器：

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>带有错误代码 0x139 (KERNEL_SECURITY_CHECK_FAILURE) 的蓝屏

后安装或启动从 Microsoft Store 的某些应用程序时，你的计算机可能会意外重新启动并出现错误：**0x139 (内核\_安全\_检查\_失败)** 。

已知受影响的应用包括 Kodi、JT2Go、Ear Trumpet、Teslagrad 和其他应用。

[Windows 更新（版本 14393.351 - KB3197954）](https://support.microsoft.com/kb/3197954)于 2016 年 10 月 27 日发布，该更新包含解决此问题的重要修补程序。 如果你遇到此问题，请更新计算机。 如果由于计算机在你可以登录前便重新启动，从而无法更新电脑，应使用系统还原将系统恢复到早于你安装受影响应用之一时的时间点。 有关如何使用系统还原的信息，请参阅 [Windows 10 中的恢复选项](https://support.microsoft.com/help/12415/windows-10-recovery-options)。

如果更新未解决问题或者你不确定如何恢复电脑，请联系 [Microsoft 支持](https://support.microsoft.com/contactus/)。

如果是开发人员，可能需要阻止在不包含此更新的 Windows 版本上安装打包应用程序。 请注意，通过执行此操作您的应用程序将不能对尚未安装更新的用户。 若要限制对已安装此更新的用户应用程序的可用性，请按如下所示修改 AppxManifest.xml 文件：

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

可在以下位置找到关于 Windows 更新的详细信息：
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>对应用进行签名时可能会出现的常见错误

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>发布服务器和证书不匹配会导致 Signtool 错误"错误：SignerSign() Failed" (-2147024885/0x8007000b)

Windows 应用包清单中的发布者条目必须与要用于签名的证书的使用者匹配。  可使用以下任一方法查看证书的使用者。

**选项 1：Powershell**

运行以下 PowerShell 命令。 可以将 .cer 或 .pfx 用作证书文件，因为它们具有相同的发布者信息。

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**选项 2：文件资源管理器**

在文件资源管理器中双击证书、选择“详细信息”选项卡，然后在列表中选择“使用者”字段。   接着，就可以复制内容。

**选项 3:CertUtil**

运行**certutil**上的 PFX 文件和复制的命令行*主题*字段从输出。

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>错误的 PE 证书 (0x800700C1)

这可以在包包含具有损坏的证书的二进制文件。 下面是一些为何发生这种的情况的原因：

* 证书开头不是图像的末尾。  

* 证书的大小不是正数。

* 证书开始后不是`IMAGE_NT_HEADERS32`结构的 32 位可执行文件或之后`IMAGE_NT_HEADERS64`64 位的可执行文件的结构。

* 证书指针不正确地对齐 WIN_CERTIFICATE 结构。

若要查找包含错误的 PE 证书的文件，请打开**命令提示符**，并设置环境变量，名为`APPXSIP_LOG`为值 1。

```
set APPXSIP_LOG=1
```

然后，从**命令提示符下**，应用程序再次进行签名。 例如：

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

有关包含错误的 PE 证书的文件的信息将出现在**控制台窗口**。 例如：

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
