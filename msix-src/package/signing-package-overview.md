---
Description: 本文介绍了 Windows 10 应用的签名要求。
title: 为 Windows 10 应用包签名
ms.date: 07/03/2019
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 5068d3adc3926a589d1f440146098e1e19265671
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68685155"
---
# <a name="sign-a-windows-10-app-package"></a>为 Windows 10 应用包签名

在创建可部署的 Windows 10 应用包过程中，应用包签名是必须执行的步骤。 Windows 10 要求使用有效的代码签名证书为所有应用程序签名。

若要成功安装 Windows 10 应用程序，该包不仅需要签名，而且还需在设备上受信任。 这意味着，该证书必须链接到设备上的某个受信任根。 默认情况下，Windows 10 信任提供代码签名证书的大部分证书颁发机构的证书。 

|主题| 描述 |
|:---|:---|
|[签名先决条件](sign-app-package-using-signtool.md#prerequisites)| 此部分说明为 Windows 10 应用包签名所要满足的先决条件。 | 
|[使用 SignTool](sign-app-package-using-signtool.md#using-signtool)| 此部分介绍如何使用 Windows 10 SDK 中的 SignTool 为应用包签名。|

## <a name="timestamping"></a>时间戳

除了使用证书为应用包签名以外，决定代码签名证书有效性的另一个重要特征是**时间戳**。 时间戳可以保留签名，这样，即使是在证书过期之后，应用部署平台也会接受该应用包。 检查包时，可以使用时间戳根据包的签名时间验证包签名。 这样，即使证书不再有效，也会接受该包。 不带时间戳的包将会根据当前时间进行评估，如果证书不再有效，Windows 不会接受该包。 

下面是围绕使用/不使用时间戳进行应用签名的各种方案：

| |不使用时间戳为应用签名 | 使用时间戳为应用签名 |
|---|---------------------------------- | ------------------------------- |
| 证书有效 |将会安装应用 | 将会安装应用 |
| 证书无效（已过期） | 无法安装应用 | 将会安装应用，因为在签名时，证书的真实性已由时间戳机构验证 |

 > [!NOTE]
 > 如果应用已在设备上成功安装，则即使是在证书过期之后，该应用也会继续运行，而不管证书是否带有时间戳。 

## <a name="device-mode"></a>设备模式

Windows 10 允许用户在“设置”应用中选择运行设备的模式。 模式包括“Microsoft Store 应用”、“旁加载应用”和“开发人员模式”。 

“Microsoft Store 应用”是最安全的模式，因为它只允许从 Microsoft Store 安装应用。  Microsoft Store 中的应用会经历认证过程，确保应用可供安全使用。 

“旁加载应用”和“开发人员模式”对其他证书签名的应用的宽容度更高，前提是这些证书受信任并已链接到设备上的某个受信任根。   仅当你是开发人员并在生成或调试 Windows 10 应用时，才能选择“开发人员模式”。 可在[此处](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development)找到有关“开发人员模式”及其提供的功能的详细信息。 
