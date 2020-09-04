---
Description: 本文提供在企业环境中管理 MSIX 应用程序部署所需的所有详细信息。  本文的目标读者是企业和 IT 专业人员。
title: 管理 MSIX 部署
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 2ecd18c4bf7cc48bca8f9cea39dedd4a042e41fc
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090455"
---
# <a name="msix-validation-and-troubleshooting"></a>MSIX 验证和故障排除
尽管 MSIX 的安装成功率为 99%，但有时你需要能够排查安装问题。

## <a name="know-the-application"></a>了解应用程序
了解你要安装的应用程序及其工作方式对于排查用户体验问题有很大的帮助。  例如，应用程序是否有某些可能会导致问题的限制？  用户是否有权访问应用程序所需的资源？  应用程序是否存在当前操作系统未满足的依赖项？

## <a name="test-your-application"></a>测试应用程序
在部署应用程序之前，请确保你已测试了应用程序。  Windows SDK 提供了一个可在发布之前查明常见问题的工具，即 Windows 应用认证工具包。  
若要安装最新 Windows SDK，请[转到此处](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。
若要详细了解 Windows 应用认证工具包，请参阅 [Windows 应用认证工具包](/windows/uwp/debug-test-perf/windows-app-certification-kit)。

## <a name="flight-your-application"></a>对应用程序进行外部测试
另一种在早期发现问题的好方法是对应用程序进行外部测试。  如果是通过 Windows 应用商店或适用于企业的 Microsoft Store 进行部署，则可以使用外部测试版来为部分个人部署你的应用程序以进行额外的真实环境测试。  
若要详细了解外部测试，请参阅[程序包外部测试](/windows/uwp/publish/package-flights?context=%252fwindows%252fmsix%252frender)。

## <a name="device-portal-and-debugging"></a>设备门户和调试
有时候，必须在用户的环境中与应用程序进行交互，以便充分了解问题。  Windows 提供了一个强大的工具（[设备门户桌面](/windows/uwp/debug-test-perf/device-portal-desktop)），它允许你连接到设备并与应用程序进行远程交互。  
若要详细了解设备门户，请参阅[设备门户桌面版](/windows/uwp/debug-test-perf/device-portal-desktop)。
若要详细了解如何调试 MSIX 程序包，请参阅[运行、调试和测试打包的桌面应用程序](./desktop-to-uwp-debug.md)。

## <a name="installation-issues"></a>安装问题
当安装出现问题时，可以调查 AppInstaller 提供的项目。  首先，AppInstaller 提供了失败错误代码。  如果任何失败的错误代码不足以确定错误所在，AppInstaller 还在事件查看器中记录了所有交互。  可以在以下位置找到日志：应用程序和服务日志->Microsoft->Windows->AppxDeployment-Server。

可以使用事件查看器或 Powershell 访问那些事件。 若要详细了解如何查看事件，请参阅[对 Windows 应用的打包、部署和查询进行故障排除](/windows/win32/appxpkg/troubleshooting)。

若要详细了解 AppInstaller 故障排除，请参阅[解决 Appinstaller 问题](../app-installer/troubleshoot-appinstaller-issues.md)。


## <a name="next-steps"></a>后续步骤

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums//home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

