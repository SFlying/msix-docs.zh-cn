---
Description: 本文提供在将 MSIX 包部署到企业环境中的 Windows 设备时需要考虑的详细信息。  本文的目标读者是企业和 IT 专业人员。
title: 规划部署
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 48b9c6661e546f515bfc7de23bed30777498223f
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77074213"
---
# <a name="plan-for-your-deployment"></a>规划部署

无论目标是消费市场还是企业，成功分发应用程序的关键都是了解部署所针对的目标设备。 根据目标平台，可能需要解决其他一些依赖关系。 有些企业在整个组织中分发一种操作系统。 还有一些企业混合使用各种硬件和操作系统。 为了在混合环境中取得成功，必须创建一个可在所有操作系统上轻松安装，同时能够限制安装程序技术差异化的解决方案。 

所有开发人员还需要知道用作目标的最低受支持操作系统。  以操作系统的最小公分母作为目标可以获得最大的覆盖潜力，但早期的操作系统版本往往不支持生成应用程序时所用的某些 API 调用。

## <a name="msix-platform-support"></a>MSIX 平台支持
MSIX 已引入到 Windows 10 版本 1709 (10.0.16299.0) 和更高版本中。  这意味着，如果使用基本 MSIX 功能并以 Windows 10 版本 1709 或更高版本作为目标，MSIX 将会正常工作。  有关支持操作系统和支持功能的完整列表，请参阅[支持的平台](../supported-platforms.md)。

## <a name="services-packaged-in-msix"></a>在 MSIX 中打包的服务
在 MSIX 中打包服务的功能已引入到 Windows 10 Client 2004 (10.0.19041.0) 和更高版本中。 因此，如果应用程序使用 MSIX 中打包的服务，则它只能部署在这些操作系统上。 若要详细了解如何使用 MSIX 中的 MSIX 打包服务，请参阅[转换包含服务的安装程序](https://docs.microsoft.com/windows/msix/packaging-tool/convert-an-installer-with-services)。

## <a name="server-support-for-msix-packages"></a>Server 对 MSIX 包的支持
MSIX 未内置在 Windows Server 中。  但是，在安装 [AppInstaller 应用程序](https://www.microsoft.com/p/app-installer/9nblggh4nns1)时，支持在装有桌面体验内部版本 1709 和更高版本的 Windows 10 Server 上使用 MSIX。  如果以早期的 Server 内部版本为目标，则还必须安装 MSIX Core。  有关 MSIX Core 的信息，请参阅 [MSIX Core](../msix-core/msixcore.md)。

## <a name="windows-10-1703-and-earlier-support-for-msix-packages"></a>Windows 10 1703 和更低版本对 MSIX 包的支持
如果以低于 Windows 10 Client 1709 (10.0.16299.0) 的早期 Windows 版本为目标，则需要使用 [MSIX Core](https://docs.microsoft.com/windows/msix/msix-core/msixcore)。 在早期的 Windows 版本上安装 MSIX Core 可以部署和运行 MSIX 应用程序。 

有关支持操作系统和支持功能的完整列表，请参阅[支持的平台](../supported-platforms.md)。 

## <a name="upgrade-downgrade-and-architecture-considerations"></a>升级、降级和体系结构注意事项
重新安装原始包时，可以升级、降级或修复 MSIX 包。  为提高效率，在降级时，MSIX 将执行差异更新，即，不会重新下载旧的有效负载。

更新现有的包时，还应考虑到其他一些因素。  MSIX 捆绑包和 MSIX 可与特定的体系结构相关。  尽管可以跨体系结构升级和降级应用（如下表中所示），但无法重新安装不同体系结构的同一版本。  

|安装（版本） |  升级或重新安装版本  | 行为 |    结果|
| :---------------: | :-----------------------: | :----------------------:|    :----------------------:|  
| x86 (1.0)   |      x86 (1.0)              | 重新安装 | 支持
| x86 (1.0)   |      x86 (3.0)              | 升级 | 支持
| x86 (1.0)  |      x64 (1.0)              |  重新安装 |<b>不支持</b>
| x86 (1.0)  |      x64 (3.0)              |  升级 | 支持
| x86 (3.0)   |      x86 (1.0)              | 降级 | 支持
| x86 (3.0)  |      x64 (1.0)              |  降级 | 支持

### <a name="downgrade"></a>降级
卸载或降级 MSIX 时，MSIX 将保留用户的 appdata。  因此请务必注意，除非较新应用创建的数据可后向兼容，否则通过降级的应用访问数据可能会出现问题。  如果数据不能后向兼容，则最好是不要允许用户降级。

若要详细了解如何控制应用的更新设置，请参阅[在应用安装程序文件中配置更新设置](https://docs.microsoft.com/windows/msix/packaging-tool/convert-an-installer-with-services)

### <a name="msix-bundles"></a>MSIX 捆绑包
MSIX 捆绑包是设计用于包含多个体系结构的包。  另一方面，MSIX 包仅支持单一体系结构。  可以使用 MSIX 捆绑包来升级或降级 MSIX 包，但反之不然：  不能使用 MSIX 包来升级或降级 MSIX 捆绑包。 

若要详细了解如何创建捆绑包，请参阅[捆绑 MSIX 包](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages)
