---
Description: 本文提供在企业环境中管理 MSIX 应用程序部署所需的所有详细信息。  本文的目标读者是企业和 IT 专业人员。
title: 在企业环境中分发 MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, 部署, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: df42f17642e054ab720a6e0c81bf9639173a1dc6
ms.sourcegitcommit: f6cee51b46fc36a57b5cf9c8cb3fd24a40ae858a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391641"
---
#   <a name="msix-app-distribution"></a>MSIX 应用分发
可以使用 Microsoft Intune 和 Microsoft Endpoint Configuration Manager 等设备与应用程序管理工具，将 MSIX 打包格式传送到客户端设备。 

##  <a name="microsoft-endpoint-configuration-manager"></a>Microsoft Endpoint Configuration Manager 

由于 MSIX 是标准化的安装打包格式，因此将通过 Microsoft Endpoint Configuration Manager 中的创建应用程序向导自动检索有关应用程序的详细信息（发布者、应用程序名称和版本），并提供这些信息供检查。 同样，对 MSIX 应用程序使用的安装字符串和检测方法是一致的，由 Microsoft Endpoint Configuration Manager 创建应用程序向导自动配置。

在 Microsoft Endpoint Configuration Manager 中创建应用程序时，请选择应用程序类型：Windows 应用包（*.appx、*.appxbundle、*.msix、*.msixbundle）  。 有关如何通过 Microsoft Endpoint Configuration Manager 创建和部署应用程序的指南，请参阅[创建和部署应用程序](https://docs.microsoft.com/configmgr/apps/get-started/create-and-deploy-an-application)。

## <a name="microsoft-intune"></a>Microsoft Intune

Microsoft Intune 支持通过客户端应用模型将 MSIX 应用程序部署到客户端设备。 由于 MSIX 是标准化的安装打包格式，因此会在应用信息中自动填充有关应用程序的详细信息（应用程序名称、说明和发布者）。

MSIX 应用程序的安装已标准化。 因此，在将新的业务线应用添加到 Microsoft Intune 时，无需配置安装所需的无提示安装参数。 有关如何通过 Microsoft Intune 创建和部署应用程序的指南，请参阅[在 Intune 中创建业务线应用](https://docs.microsoft.com/mem/intune/apps/lob-apps-windows)。

## <a name="web-app-installer"></a>Web（应用安装程序）

可以使用 IIS 服务器部署 MSIX。  如果添加 ms-appinstaller 协议，将会创建更好的安装体验。  
有关 MSIX 文件的 IIS 分发，以及如何配置 IIS 服务器来支持 MSIX 应用分发，请参阅[从 IIS 服务器分发 Windows 10 应用](https://docs.microsoft.com/windows/msix/app-installer/web-install-iis)。

## <a name="microsoft-store-for-business"></a>适用于企业的 Microsoft Store

[适用于企业的 Microsoft Store](https://businessstore.microsoft.com/store) 是专门设计用于分发企业和教育应用的应用商店。 可以使用 Microsoft Store 来查找、获取、分发和管理适用于组织或学校的应用。  有关适用于企业的 Microsoft Store 的详细信息，请参阅[适用于企业和教育行业的 Microsoft Store](https://docs.microsoft.com/microsoft-store/)。

## <a name="app-center"></a>应用中心

利用[应用中心](https://appcenter.ms/)可以自动生成应用，在真实设备上测试该应用，以及将其分发给 beta 测试人员。  使用应用中心可以更频繁、以更高的质量且更自信地交付应用。  使用应用中心可以连接存储库，在几分钟内自动化生成，在云中的真实设备上进行测试，将应用分发给 beta 测试人员，以及通过崩溃和分析数据监视现实使用情况。 它是一站式的服务。


## <a name="deployment-image-servicing-and-management-dismexe-and-provisioning"></a>部署映像服务和管理 (DISM.exe) 与预配

### <a name="dism"></a>DISM
在部署之前，IT 专业人员可以使用部署映像服务和管理 (DISM) cmdlet 在 Windows 映像中安装、卸载和配置 MSIX 包。  
有关预配的详细信息，请参阅[部署映像服务和管理与预配](managing-your-msix-deployment-dism-provisioning.md)。

### <a name="provisioning"></a>设置
IT 专业人员可以使用预配来配置最终用户的设备，而无需重建映像。  IT 专业人员可在其最终用户系统上预先安装 MSIX 包。
有关预配的详细信息，请参阅[部署映像服务和管理与预配](managing-your-msix-deployment-dism-provisioning.md)。

## <a name="managing-your-msix-app"></a>管理 MSIX 应用

MSIX 包提供一套综合性的控制机制让 IT 专业人员控制其安装。  IT 专业人员能够指定如何以及何时可以升级、降级或卸载 MSIX 应用。  还可以使用 AppLocker 和组策略等现成的 Windows 服务来限制 MSIX 包。 

### <a name="prevent-msix-app-installs-through-applocker"></a>防止通过 AppLocker 安装 MSIX 应用

[AppLocker](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview) 支持允许或拒绝在企业设备上执行 MSIX 应用程序的功能。 可以通过基于 MSIX 应用属性定义规则来实现此目的。 这些属性包括：发布者名称、产品名称、文件名、文件版本、文件路径和文件哈希。 然后，将这些规则标识的 MSIX 应用配置为允许或拒绝执行。

在组织内部，能够利用 AppLocker 中的多种方法来控制哪些应用可以或者不可以在企业设备上执行。 有关完整列表，请参阅[使用 AppLocker 规则](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/applocker/working-with-applocker-rules)。

### <a name="manage-access-through-group-policy"></a>通过组策略管理访问权限

组策略针对 Active Directory 环境中的操作系统、应用程序和用户设置提供集中式管理和配置。 MSIX 包应用程序可以读取组策略注册表项并遵循组策略设置。  
若要详细了解 MSIX 支持以及组策略支持限制，请参阅[组策略和 MSIX 打包的应用](https://review.docs.microsoft.com/windows/msix/group-policy-msix)。

### <a name="manage-msix-updates"></a>管理 MSIX 更新

使用应用安装程序文件配置应用的更新行为。  IT 专业人员可以定义用户何时获取 MSIX 的更新，以及更新体验是否是无提示的。  可以要求用户在启动时更新，或者在以后更新。    

有关配置 MSIX 更新计划的详细信息，请参阅[在应用安装程序文件中配置更新设置](https://docs.microsoft.com/windows/msix/app-installer/update-settings)。

#### <a name="downgrades"></a>降级

MSIX 支持降级应用，因此，在安装同一应用的旧版本之前无需卸载该应用。 指定 ForceUpdateFromAnyVersion 即可将 MSIX 降级一个版本。 如果已部署的应用存在严重的 bug，则此功能很有用。  

有关 ForceUpdateFromAnyVersion 的详细信息，请参阅[在应用安装程序文件中配置更新设置](https://docs.microsoft.com/windows/msix/app-installer/update-settings)。

#### <a name="critical-updates"></a>关键更新

用户偶尔会忽略更新应用的提示。  使用 MSIX，IT 专业人员可以通过指定 UpdateBlocksActivation 将更新标记为关键，以强制在应用中安装该更新。

有关 UpdateBlocksActivation 的详细信息，请参阅[在应用安装程序文件中配置更新设置](https://docs.microsoft.com/windows/msix/app-installer/update-settings)。

## <a name="uninstall"></a>卸载

MSIX 提供可靠的安装和卸载体验。  由于 MSIX 包是容器化的包，因此卸载包时会删除所有应用程序项目，包括写入到 %ProgramFiles%WindowsApps 的所有文件，以及 AppData 文件夹中的任何系统文件，或者为应用程序创建的注册表设置。  卸载不会删除用户创建的任何文件。
