---
description: 本文介绍了签名工具的已知问题和故障排除提示
title: SignTool 的已知问题和故障排除
ms.date: 02/05/2020
ms.topic: article
author: dianmsft
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: e010332612988673f9339917a3d0c0867a1cc818
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073693"
---
# <a name="known-issues-and-troubleshooting-for-signtool"></a>SignTool 的已知问题和故障排除
使用 **SignTool** 的最常见错误类型为内部错误，通常如下所示：

```syntax
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

如果错误代码开头为 0x8008，如 0x80080206 (APPX_E_CORRUPT_CONTENT)，则表示已签名的应用包无效。 如果你收到此类错误，必须重新生成软件包，并再次运行 **SignTool**。

**SignTool** 具有可用于显示证书错误和筛选的调试选项。 若要使用调试功能，则在 `/debug`（签名）（后跟完整的 `sign`SignTool**命令）后直接放置**（调试）选项。

```syntax
SignTool sign /debug [options]
``` 

更常见错误为 0x8007000B。 关于此类错误，可以在事件日志中查找更多信息。
 
请执行以下操作，以便在事件日志中查找更多信息：
- 运行 Eventvwr.msc
- 打开事件日志：事件查看器（本地）-> 应用程序和服务日志 -> Microsoft -> Windows -> AppxPackagingOM -> Microsoft-Windows-AppxPackaging/Operational
- 查找最新的错误事件

内部错误 0x8007000B 通常与以下值之一对应：

| **事件 ID** | **示例事件字符串** | **仅供参考** |
|--------------|--------------------------|----------------|
| 150          | 错误 0x8007000B：应用部件清单发布者名称 (CN=Contoso) 必须与签名证书的使用者名称 (CN=Contoso, C=US) 匹配。 | 应用清单发布者名称必须与签名证书的使用者名称完全匹配。               |
| 151          | 错误 0x8007000B：指定的签名哈希方法 (SHA512) 必须与应用包块映射中使用的哈希方法 (SHA256) 匹配。     | /fd 参数中指定的 hashAlgorithm 不正确。 使用与应用包块映射（创建应用包所用）匹配的 hashAlgorithm 重新运行 **SignTool**  |
| 152          | 错误 0x8007000B：应用包内容必须针对其块映射进行验证。                                                           | 应用包已损坏，需要重建以生成新的块映射。 有关创建应用包的详细信息，请参阅[使用 MakeAppx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md)。 |