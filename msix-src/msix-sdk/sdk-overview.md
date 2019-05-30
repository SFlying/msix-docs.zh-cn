---
title: MSIX SDK
description: MSIX SDK 是一个开放源代码项目，允许开发人员在所有平台上普遍使用 MSIX 包格式。
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10，uwp msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e34bccadc54960ce7a55d62b2e1c999759fecc7d
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "58900299"
---
# <a name="msix-sdk"></a>MSIX SDK 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging" data-linktype="external">GitHub 上的 MSIX SDK</a></p></div>

MSIX SDK 是一个开放源代码项目，允许开发人员在所有平台上普遍使用 MSIX 包格式。 这允许开发人员在所有平台上生成一致的体验，为其用户和分发使用同一个包的体验。 SDK 提供了开发人员可以将其应用程序内容打包并生成应用包清单，它可以针对所选的平台的方式的指南。 这使开发人员能够一次而不是让其应用程序内容打包到为每个平台的包。

SDK 提供了 Api 所需验证，验证，然后将包内容从 MSIX 包解压缩。 使用项目，应用程序开发人员无需担心如何，包是否已被篡改或如果它可以是受信任。 应用内容可解包之前，它将执行防篡改保护和签名验证检查。

可由任何跨平台客户端应用程序，允许第三方生成插件或扩展 SDK。 客户端应用程序开发人员可以使用可在 Windows 10 平台的应用程序扩展模型，并在非 Windows 10 平台上使用 MSIX SDK。 使用 SDK 的帮助，第三方开发人员构建应用扩展和客户端应用程序的插件不需要生成每个平台的特定包。 相反，他们构建 Windows 10 受支持的一个程序包和所有其他平台。 使用 SDK，应用程序开发人员可以选择特定的平台支持。

MSIX 包的主要差异之一是清单文件。 清单文件包含有关包的所有元数据，并指定客户端应用程序可以访问以便做出适当的选择，如适用性或可支持性的所有密钥信息。 清单文件允许客户端应用程序开发人员和第三方开发人员更多的选项和灵活地进行通信的要求、 可用性和支持等特征。

## <a name="get-more-info"></a>获取更多信息

MSIX sdk [GitHub 上的开放源代码项目](https://github.com/Microsoft/msix-packaging)。 GitHub 存储库包含的完整源代码和有关如何生成每个平台的二进制文件的说明。

如果有任何反馈，请将其提交上[MSIX 技术社区站点](https://techcommunity.microsoft.com/t5/MSIX/ct-p/MSIX)。 如果有问题或标识 SDK 中的 bug，您可以发布到[问题页](https://github.com/Microsoft/msix-packaging/issues)MSIX SDK GitHub 存储库。