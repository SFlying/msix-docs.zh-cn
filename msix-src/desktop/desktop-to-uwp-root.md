---
description: 本文介绍为现有 Windows 窗体、WPF 或者 Win32 应用或游戏创建新式 Windows 10 应用包的端到端过程。
title: 打包桌面应用程序
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf8ca8e769683ac2c66f558ef4e85f36671d42ce
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072607"
---
# <a name="package-desktop-applications-msix"></a>打包桌面应用程序 (MSIX)

使用现有的桌面应用程序，为 Windows 10 用户增加新式体验。 然后，将应用发布到 Microsoft Store，以拓展国际市场。 可以直接利用 Store 中内置的功能，以更简单的方式通过应用程序获取利润。 当然，并非一定要使用 Store。 完全可以使用现有的渠道。

为桌面应用程序创建 MSIX 包时，应用程序将获得一个标识，而桌面应用程序可以借助该标识访问 Windows 通用平台 (UWP) API。 可以使用这些 API 为具有吸引力的新式体验增加亮点，例如动态磁贴和通知。 仅当应用程序在 Windows 10 上运行时，才可使用简单的条件编译和运行时检查来运行 UWP 代码。

除了为 Windows 10 体验增加亮点的代码以外，应用程序还能保持不变，你可以继续将其分发给旧版 Windows 的用户。 在 Windows 10 上，应用程序将继续像如今的情形一样，以完全信任用户模式运行。

## <a name="prepare"></a>准备

首先，查看文章[准备打包桌面应用](desktop-to-uwp-prepare.md)，准备应用程序，然后解决应用程序存在的所有问题，最后为应用程序创建 Windows 应用包。 在创建包之前，可能不需要对应用程序进行很多更改。 但是，某些情况下可能需要在创建包之前对应用程序进行调整。

<a id="convert" />

## <a name="build-an-msix-from-source-code-using-visual-studio"></a>使用 Visual Studio 从源代码生成 MSIX

如果使用 Visual Studio 来维护应用程序，但应用程序没有安装程序或安装程序无法执行太多复杂任务，请考虑改用 Visual Studio。

使用 Visual Studio 可以轻松创建包。 将 **Windows 应用程序包项目**添加到解决方案，引用桌面项目，然后按 F5 调试应用。 下面是可以使用它执行的其他一些操作。

:heavy_check_mark:自动生成可视资产。

:heavy_check_mark:使用可视设计器对清单进行更改。

:heavy_check_mark:使用向导生成包。

:heavy_check_mark:基于在[合作伙伴中心](https://partner.microsoft.com/dashboard)保留的名称轻松为应用程序分配标识。

有关说明，请参阅[使用 Visual Studio 打包桌面应用程序](desktop-to-uwp-packaging-dot-net.md)。 下表显示了支持的 Visual Studio、Windows 10 版本以及包格式。

|  支持的 Visual Studio 版本 | 支持用于创建包的 OS 版本  | 支持用于安装包的 OS 版本  |  支持的包格式  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15.5 和更高版本       |  Windows 10 版本 1607 和更高版本           |  Windows 10 版本 1607 和更高版本            |  .msix（适用于 Windows 10 版本 1709 和更高版本）<br/>.appx（适用于 Windows 10 版本 1607 和更高版本）                 |


## <a name="add-modern-windows-10-experiences"></a>添加新式 Windows 10 体验

为桌面应用创建 MSIX 包后，可以使用 UWP API、包扩展和 UWP 组件来为引入注目的新式 Windows 10 体验增加亮点（例如动态磁贴和通知）。

### <a name="enhance-with-uwp-apis"></a>使用 UWP API 进行增强

将应用打包后，可以通过添加动态磁贴和推送通知等功能来为其增加亮点。 其中某些功能可以显著提高应用程序的参与度，并且只需花费很少的时间即可添加。 某些增强功能需要更多的代码。

请参阅[在桌面应用程序中使用 UWP API](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)。

### <a name="integrate-with-package-extensions"></a>集成包扩展

如果应用程序需要与系统集成（例如，建立防火墙规则），请在应用程序的包清单中描述集成任务，系统将完成其余操作。 对于其中的大多数任务，根本不必编写任何代码。 在清单中添加少量的 XML 后，可以执行一些操作，例如，在用户登录时启动进程、将应用程序集成到文件资源管理器中，以及为应用程序添加显示在其他应用中的打印目标列表。

请参阅[将桌面应用程序与包扩展集成](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions)。

### <a name="extend-with-uwp-components"></a>使用 UWP 组件进行扩展

某些 Windows 10 体验（例如，支持触摸的 UI 页面）必须在新式应用容器内部运行。 一般情况下，首先应确定是否可以通过 UWP API [增强](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)现有桌面应用程序来添加体验。 如果必须使用 UWP 组件来实现体验，可将 UWP 项目添加到解决方案，并使用应用服务在桌面应用程序与 UWP 组件之间通信。

请参阅[使用 UWP 组件扩展桌面应用程序](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend)。

## <a name="test"></a>测试

若要在准备分发时在现实设置中测试应用程序，最好是将应用程序签名，然后安装它。 请参阅[测试应用](desktop-to-uwp-debug.md#test-your-app)。

>[!IMPORTANT]
> 如果你打算将应用程序发布到 Microsoft Store，请确保应用程序能够在运行 S 模式 Windows 10 的设备上正常工作。 这是 Store 的一项要求。 请参阅[测试 Windows 应用是否适用于 S 模式下的 Windows 10](desktop-to-uwp-test-windows-s.md)。

## <a name="validate"></a>验证

若要为应用程序创造在 Microsoft Store 中发布的最佳机会或令其通过 [Windows 认证](https://go.microsoft.com/fwlink/p/?LinkID=309666)，请在提交应用程序进行认证之前先在本地进行验证和测试。

如果使用 DAC 打包应用，可以使用新的 ``-Verify`` 标志来根据打包的桌面应用程序和 Store 要求验证包。 请参阅[将应用打包、为应用签名并为 Store 提交做好准备](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters)。

如果使用 Visual Studio，则可以通过“创建应用包”向导验证应用程序。  请参阅[创建应用包上传文件](../package/packaging-uwp-apps.md#generate-an-app-package-upload-file-for-store-submission)。

若要手动运行该工具，请参阅 [Windows 应用认证工具包](/windows/uwp/debug-test-perf/windows-app-certification-kit)。

若要查看 Windows 应用认证用于验证应用的测试列表，请参阅 [Windows 桌面桥应用测试](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests)。

## <a name="distribute"></a>分发

可以通过将应用程序发布到 Microsoft Store 或将其旁加载到其他系统来分发应用程序。

请参阅[分发打包的桌面应用](/windows/apps/desktop/modernize/desktop-to-uwp-distribute)。

