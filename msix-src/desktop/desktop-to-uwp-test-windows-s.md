---
description: 本文介绍如何测试 Windows 应用，以确保其在以 S 模式运行 Windows 10 的设备上正常运行。
title: 测试适用于 Windows 10 S 的 Windows 应用
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10 S, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 4cceecd65452b96eed96692095947a7e17c6418e
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89089865"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>测试 Windows 应用是否适用于 S 模式下的 Windows 10

可以对 Windows 应用进行测试，以确保其在以 S 模式运行 Windows 10 的设备上正常运行。 事实上，如果准备将应用发布到 Microsoft Store，则必须这样做，因为这是 Microsoft Store 的一项要求。 若要测试应用，可以在运行 Windows 10 专业版的设备上应用 Windows Defender 应用程序控制 (WDAC) 策略。

此 WDAC 策略强制执行一些规则，应用必须遵守这些规则才能在 Windows 10 S 上运行。

> [!IMPORTANT]
>我们建议你将这些策略应用于虚拟机，但如果你想要将它们应用于本地计算机，则请确保在应用策略之前，先查看本主题“下一步，安装策略并重启系统”部分中的最佳实践指南。

<a id="choose-policy"></a>

## <a name="first-download-the-policies-and-then-choose-one"></a>首先，下载这些策略，然后从中选择一个

请在[此处](https://go.microsoft.com/fwlink/?linkid=849018)下载 WDAC 策略。

然后，选择对你最有意义的一个策略。 以下是每个策略的摘要。

|策略 |强制 |签名证书 |文件名 |
|--|--|--|--|
|审核模式策略 |日志问题/不阻止 |存储 |SiPolicy_Audit.p7b |
|生产模式策略 |是 |存储 |SiPolicy_Enforced.p7b |
|具有自签名应用的生产模式策略 |是 |AppX 测试证书  |SiPolicy_DevModeEx_Enforced.p7b |

建议从审核模式策略开始。 可以查看代码完整性事件日志，并使用该信息帮助你对应用进行调整。 然后，当你准备好进行最终测试时，请应用生产模式策略。

以下是有关每个策略的更多信息。

### <a name="audit-mode-policy"></a>审核模式策略
在此模式下，即使你的应用执行 Windows 10 S 上不支持的任务，应用也会运行。Windows 将任何可能已被阻止的可执行文件记录到代码完整性事件日志中。

可以通过打开“事件查看器”并浏览到此位置来找到这些日志：应用程序和服务日志->Microsoft->Windows->CodeIntegrity->Operational。

![代码完整性事件日志](images/code-integrity-logs.png)

此模式为安全模式，不会阻止你的系统启动。

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>（可选）查找调用堆栈中的特定故障点
若要在调用堆栈中找到阻止问题发生的特定点，请添加此注册表项，然后[设置内核模式调试环境](/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging)。

|键|名称|类型|值|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![注册表设置](images/ci-debug-setting.png)

### <a name="production-mode-policy"></a>生产模式策略
此策略强制执行与 Windows 10 S 匹配的代码完整性规则，使你可以模拟在 Windows 10 S 上运行。这是最严格的策略，非常适合用于最终生产测试。 在此模式下，你的应用受到的限制与在用户设备上受到的限制相同。 若要使用此模式，你的应用必须由 Microsoft Store 签名。

### <a name="production-mode-policy-with-self-signed-apps"></a>具有自签名应用的生产模式策略
此模式类似于生产模式策略，但它也允许通过使用 zip 文件中包含的测试证书进行签名的应用运行。 安装此 zip 文件的 AppxTestRootAgency 文件夹中包含的 PFX 文件。 然后，使用它对你的应用进行签名。 如此一来，你便可以快速进行循环访问，而无需应用商店签名。

由于你的证书的发布者名称必须与你的应用的发布者名称匹配，你将需要暂时将 Identity 元素的 Publisher 属性更改为“CN = Appx Test Root Agency Ex”。  完成测试后，你可以将该属性更改回其原始值。

## <a name="next-install-the-policy-and-restart-your-system"></a>接下来，安装策略并重启系统

我们建议将这些策略应用于虚拟机，因为这些策略可能导致启动失败。 这是因为这些策略会阻止未经 Microsoft Store 签名的代码执行，包括驱动程序。

如果想要将这些策略应用于本地计算机，最好从审核模式策略开始。 使用此策略，你可以查看代码完整性事件日志，以确保在强制执行的策略中不会阻止任何关键操作。

准备好应用策略时，找到所选策略的 .P7B 文件，将其重命名为 SIPolicy.P7B，然后将该文件保存到系统上的此位置：C:\Windows\System32\CodeIntegrity\\。

然后，重启系统。

>[!NOTE]
>若要从系统中删除策略，请先删除 .P7B 文件，然后重启系统。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**查看我们的应用咨询团队发布的详细博客文章**

请参阅[使用桌面桥移植和测试 Windows 10 上的经典桌面应用程序](/archive/blogs/appconsult/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge)。

**解让测试以 S 模式运行的 Windows 变得更轻松的工具**

请参阅[对 APPX 进行解压缩、修改、重新打包、签名](/archive/blogs/appconsult/unpack-modify-repack-sign-appx)。