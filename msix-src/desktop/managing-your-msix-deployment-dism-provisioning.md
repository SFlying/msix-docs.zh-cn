---
Description: 本文介绍了为所有用户安装 MSIX 应用的不同方法。
title: 为所有用户安装应用
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e20d16fb5c25bc76ecccab9dd6a2b59cc1dc39a6
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073973"
---
# <a name="install-your-app-for-all-users-with-dism"></a>使用 DISM 为所有用户安装应用

有许多工具可用于为所有用户安装 MSIX 应用。 [部署映像服务和管理 (DISM.exe)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism---deployment-image-servicing-and-management-technical-reference-for-windows) 是一个命令行工具，可用来维修和准备 Windows 映像，包括那些用于 Windows PE、Windows 恢复环境 (Windows RE) 和 Windows 安装程序的映像。 DISM 可用来维修 Windows 映像 (.wim) 或虚拟硬盘（.vhd 或 .vhdx）。

Windows 预配可使 IT 专业人员轻松配置最终用户设备，而无需进行映像处理。 使用 Windows 预配，IT 专业人员可以轻松指定将设备注册到管理中所需的配置和设置，然后在几分钟内将该配置应用到目标设备。 例如，为某个设备上的所有用户安装应用。 

从 Windows 版本 1809 开始，IT 专业人员可以通过预配进行预安装。   预配的应用将安装到中心位置 %ProgramFiles%\WindowsApps，并且将立即可供注册用户使用。 未将 MSIX 注册到其帐户的用户将无权访问。  
有关预配程序包的更多详细信息，请参阅[针对 Windows 10 预配程序包](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-packages)。

## <a name="user-preferences-when-provisioning"></a>预配时的用户首选项

预配会遵守用户设置，因此，当用户卸载预配的应用后，重新获得预配的应用的唯一方法是用户重新安装它。 在 Windows 10 版本 2004 中，预配的应用将自动覆盖用户设置，并在重新预配时重新安装该应用。

你还应当注意防止用户安装和更新预配的 MSIX 程序包的不同版本。 如果用户通过从应用商店安装同一程序包来升级或降级某个预配的应用，可能会出现意外结果。
有关 MSIX 预配的更多详细信息，请参阅[在配置管理器中创建 Windows 应用程序](https://docs.microsoft.com/configmgr/apps/get-started/creating-windows-applications)。

## <a name="removing-apps-for-all-users"></a>为所有用户删除用户

除了为所有用户安装应用之外，你还可以为所有用户卸载应用。 执行此操作的方法是使用包管理器 API，[PackageManager.RemovePackageAsync 方法](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.removepackageasync)。

另外，还可以使用 **AppxRemovePackageForAllUsers(PCWSTR packageFullName, BOOL* reserved)()* * 方法，可以在 Windows 10 版本 1703 中引入的 **AppxDeploymentClient.dll** 中找到此方法。 但是，我们建议改用 **PackageManager.RemovePackageAsync**。
