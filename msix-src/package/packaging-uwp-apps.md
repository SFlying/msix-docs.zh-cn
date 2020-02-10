---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: 打包 .MSIX 应用
description: 若要分发或销售 Windows 应用，需要为其创建应用包。
ms.date: 07/18/2019
ms.topic: article
keywords: windows 10, uwp, msix
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 18530e9f2b0c75c62bf41d5240350685ef7dbfad
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072959"
---
# <a name="package-a-desktop-or-uwp-app-in-visual-studio"></a>在 Visual Studio 中打包桌面或 UWP 应用

在分发您的应用程序之前，您需要将其打包。 本文介绍如何使用 Visual Studio 配置、创建和测试 .MSIX 包。

## <a name="types-of-app-packages"></a>应用包类型

- **应用包（. .msix 或 .appx）**  
    单个包，其中包含应用程序及其资源，面向单个设备体系结构。 例如，x64 或 x86 应用程序包。 若要以应用捆绑包为目标，需要为每个体系结构生成一个结构。 

- **应用捆绑包（. .msixbundle 或 .appxbundle）**  
    应用程序包可以包含多个应用包，每个包都构建为支持特定的设备体系结构。 例如，一个应用程序包可以包含三个独立的应用包，它们分别用于 x86、x64 和 ARM 配置。 应尽可能生成应用程序包，因为它们使你的应用能够在尽可能广泛的设备上使用。  

- **应用包上传文件（. msixupload 或 .appxupload）-仅限商店提交**  
    一个可以包含多个应用包或一个应用程序包以支持各种处理器体系结构的文件。 应用包上传文件还包含一个符号文件，用于分析应用在 Microsoft Store 中发布后的[应用性能](https://docs.microsoft.com/windows/uwp/publish/analytics)。 如果你正在将应用程序与 Visual Studio 一起打包到合作伙伴中心以便发布到 Microsoft Store，则将自动为你创建此文件。

以下是准备和创建应用包的步骤概述：

1. [在打包应用前](#before-packaging-your-app)。 请按照以下步骤操作，确保应用已准备好进行打包。

2. [配置项目](#configure-your-project)。 使用 Visual Studio 清单设计器配置程序包。 例如，添加磁贴图像并选择应用支持的方向。

3. [生成应用程序包](#generate-an-app-package)。 使用 Visual Studio 打包向导来创建应用包。


4. [运行、调试和测试打包的应用程序](../desktop/desktop-to-uwp-debug.md)。 通过 Visual Studio 或直接安装包来运行和调试应用程序包。


## <a name="before-packaging-your-app"></a>在打包应用之前

1. **测试您的应用程序。** 打包应用程序之前，请确保它在你打算支持的所有设备家族上按预期工作。 这些设备系列可能包括桌面设备、移动设备、Surface Hub、Xbox、IoT 设备或其他设备。 有关使用 Visual Studio 部署和测试应用的详细信息，请参阅[部署和调试 UWP 应用](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)（也适用于打包的桌面应用）。

2. **优化您的应用程序。** 可以使用 Visual Studio 的分析和调试工具来优化打包应用程序的性能。 例如，用于 UI 响应能力的时间线工具、内存使用工具、CPU 使用工具等。 有关这些工具的详细信息，请参阅[分析功能教程](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour)主题。

3. **检查 .NET Native 兼容性（适用于C# VB 和应用）。** 在通用 Windows 平台中，有一个本机编译器可以提升应用的运行时性能。 通过这项更改，你应在该编译环境中测试你的应用。 默认情况下，**Release** 版本配置会启用 .NET 本机工具链，因此请务必使用此 **Release** 配置测试应用并检查应用是否按预期运行。


## <a name="configure-your-project"></a>配置项目

应用清单文件 (Package.appxmanifest.xml) 是一个 XML 文件，其中包含创建应用包所需的属性和设置。 例如，应用清单文件中的属性描述了要用作应用的磁贴的映像和应用在用户旋转设备时支持的方向。

Visual Studio 的清单设计器让你能够更新清单文件，而无需编辑文件的原始 XML。

### <a name="configure-a-package-with-the-manifest-designer"></a>使用清单设计器配置包

1. 在**解决方案资源管理器**中，展开应用程序项目的项目节点。

2. 双击 **Package.appxmanifest** 文件。 如果清单文件已经在 XML 代码视图中打开，Visual Studio 会提示你关闭该文件。

3. 现在可以决定如何配置你的应用。 每个选项卡都包含应用配置的相关信息以及在必要时可获取详细信息的链接。


    ![Visual Studio 中的清单设计器](images/packaging-screen1.jpg)

    检查 "**视觉资产**" 选项卡上的应用所需的所有映像。

    你可以从**打包**选项卡中输入发布数据。 可在此处选择要使用哪个证书来签名你的应用。 必须使用证书对所有 .MSIX 应用进行签名。

    > [!NOTE]
    > 从 Visual Studio 2019 开始，已打包的桌面或 UWP 项目中不再生成临时证书。 若要创建或导出证书，请使用[本文](create-certificate-package-signing.md)中介绍的 PowerShell cmdlet。

    > [!IMPORTANT]
    > 如果你是在 Microsoft Store 中发布应用，将使用你的受信任的证书进行签名。 这让用户能够安装和运行你的应用，而不必安装关联的应用签名证书。

    如果要在设备上安装应用程序包，首先需要信任包。 要信任该程序包，必须在用户设备上安装证书。

4. 在你对应用进行必要的编辑后，请保存你的 **Package.appxmanifest** 文件。

如果通过 Microsoft Store 分发应用，则 Visual Studio 可以将你的包与应用商店相关联。 为此，请在解决方案资源管理器中右键单击项目名称，然后选择 "**发布**"->**将应用与应用商店关联**（Visual Studio 2019 版本16.3 之前，"**发布**" 菜单的名称为 "**存储**"）。 您也可以在 "**创建应用程序包**" 向导中执行此操作，如下一节中所述。 在关联应用时，会自动更新清单设计器的“打包”选项卡中的某些字段。

## <a name="generate-an-app-package"></a>生成应用程序包

可以通过使用 Microsoft Intune 和 Configuration Manager 等应用程序管理工具在网站上发布应用程序，而无需在应用商店中发布应用程序。还可以直接在本地或远程计算机上安装用于测试的 .MSIX 包。

### <a name="create-an-app-package-using-the-packaging-wizard"></a>使用打包向导创建应用包

> [!NOTE]
> 以下说明和屏幕截图介绍 Visual Studio 2019 版本16.3 中的处理过程。 如果使用的是早期版本，则某些 UI 的外观可能会有所不同。
> 如果正在打包桌面应用程序，请右键单击 "Windows 应用程序打包项目" 节点。

1. 在**解决方案资源管理器**中，打开应用程序项目的解决方案。

2. 右键单击该项目，然后选择 "**发布**->**创建应用包**" （Visual Studio 2019 版本16.3 之前，"**发布**" 菜单的名称为 "**存储**"）。

    ![上下文菜单，可导航到“创建应用包”](images/packaging-screen2.jpg)

3. 在向导的第一页中选择 "**旁加载**"，然后单击 "**下一步**"。

    ![显示的“创建程序包”对话框窗口](images/packaging-screen10.png
)

4. 在 "**选择签名方法**" 页上，选择是跳过打包签名还是选择证书进行签名。 你可以从本地证书存储中选择证书、选择证书文件或创建新证书。 若要将 .MSIX 包安装在最终用户的计算机上，必须使用计算机上受信任的证书对其进行签名。 

    ![显示的“创建程序包”对话框窗口](images/package-signing2.png)

5. 完成 "[使用 Visual Studio 创建应用包上传文件](#create-your-app-package-upload-file-using-visual-studio)" 部分中所述的 "**选择和配置包**" 页。

### <a name="install-your-app-package-by-double-clicking"></a>双击安装应用程序包

只需双击应用包文件即可安装应用程序包。 为此，请导航到应用包或应用捆绑包文件，然后双击该文件。 [应用程序安装程序](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root)将启动并提供基本应用程序信息、安装按钮、安装进度栏和任何相关错误消息。

> [!NOTE]
> 应用安装程序假定已使用设备上受信任的证书对包进行了签名。 如果没有，则需要将签名证书安装到设备上的 "受信任人" 或 "受信任的发行者" 证书颁发机构存储区。 如果你不确定如何执行此操作，请参阅[安装测试证书](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)。

### <a name="install-your-app-package-using-an-install-script"></a>使用安装脚本安装应用程序包

1. 打开 `*_Test` 文件夹。
2. 右键单击 **Add-AppDevPackage.ps1** 文件。 选择**使用 PowerShell 运行**并按照提示操作。  
    ![导航到 PowerShell 脚本的文件资源管理器](images/packaging-screen7.jpg)

    安装应用包后，PowerShell 窗口显示消息：**已成功安装你的应用。**

3. 单击“开始”按钮，搜索应用名称，然后启动它。

### <a name="next-steps-debug-and-test-your-app-package"></a>后续步骤：调试和测试应用程序包

有关如何在 Visual Studio 中调试应用程序或使用 Windows 调试工具的详细说明，请参阅[运行、调试和测试应用程序包](../desktop/desktop-to-uwp-debug.md)。

## <a name="generate-an-app-package-upload-file-for-store-submission"></a>为应用商店提交生成应用程序包上传文件

若要将应用分发到 Microsoft Store，建议生成应用程序包上传文件（. msixupload 或 .appxupload）。

> [!NOTE]
> 如果要手动创建应用程序包（. .msix 或 .appx）或应用捆绑包（. .msixbundle 或 .appxbundle），请参阅[使用 makeappx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md)。

虽然可将应用包或应用捆绑包仅提交给合作伙伴中心，但建议提交**应用包上传文件**。 你可以使用 Visual Studio 中的 "**创建应用包**" 向导创建应用包上传文件，也可以从现有应用包或应用捆绑中手动创建。

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>使用 Visual Studio 创建应用包上传文件

> [!NOTE]
> 以下说明和屏幕截图介绍 Visual Studio 2019 版本16.3 中的处理过程。 如果使用的是早期版本，则某些 UI 的外观可能会有所不同。

1. 在**解决方案资源管理器**中，打开适用于你的 UWP 应用项目的解决方案。

2. 右键单击该项目，然后选择 "**发布**->**创建应用包**" （Visual Studio 2019 版本16.3 之前，"**发布**" 菜单的名称为 "**存储**"）。 如果此选项已禁用或完全不显示，请检查该项目是否为通用 Windows 项目。  

    ![上下文菜单，可导航到“创建应用包”](images/packaging-screen2.jpg)

    将显示**创建应用包**向导。

3. 在第一个对话框中选择 " **Microsoft Store** "，然后单击 "**下一步**"。  

    ![显示的“创建程序包”对话框窗口](images/packaging-screen3.jpg)

    如果已将项目与应用商店中的应用相关联，则还可以选择为关联的应用商店应用创建包。 如果选择**旁加载**，Visual Studio 将不会生成适用于合作伙伴中心提交的应用包上传（. msixupload 或 .appxupload）文件。 如果只想为非存储分发创建 .MSIX 程序包或捆绑包，可以选择此选项。

4. 在下一页上，用开发人员帐户登录到合作伙伴中心。 如果你还没有开发者帐户，该向导将帮助你创建一个。

    ![使用显示的应用名称选择创建应用包窗口](images/packaging-screen4.jpg)

5. 从当前注册到你的帐户的应用列表中选择你的包的应用程序名称，或者保留一个新的应用程序名称（如果你尚未在合作伙伴中心保留一个）。  

6. 确保在 **Select and Configure Packages** 对话框中选择全部三种体系结构配置（x86、x64 和 ARM），以确保你的应用能够部署到最广泛的设备上。 在**生成应用程序包**列表框中，选择**始终**。 应用捆绑包（.appxbundle 或 .msixbundle）优于单个应用包文件，因为它包含为每种类型的处理器体系结构配置的应用包的集合。 选择生成应用捆绑包时，应用捆绑包将包含在最终的应用包上传（. .appxupload 或 msixupload）文件中，同时包含调试和崩溃分析信息。 如果你不确定该选择哪种体系结构，或者想了解有关各种设备使用哪种体系结构的详细信息，请参阅[应用包体系结构](device-architecture.md)。  

    ![使用显示的程序包配置创建应用包窗口](images/packaging-screen5.jpg)

7. 在发布应用后，包括公共符号文件以分析合作伙伴中心的[应用性能](https://docs.microsoft.com/windows/uwp/publish/analytics)。 配置任何其他详细信息，例如版本编号或包输出位置。

8. 单击**创建**生成应用包。 如果你在步骤3中选择了 "**要创建的包" 以上载到 Microsoft Store**选项，并且正在为合作伙伴中心提交创建包，则该向导将创建包上传（. .appxupload 或 msixupload）文件。 如果选择了 "我想要在步骤3中**创建用于旁加载的包**"，则向导将根据您在步骤6中所做的选择创建单个应用包或应用捆绑包。

9. 成功打包应用后，你将看到此对话框，你可以从指定的输出位置检索应用包上传文件。 此时，你可以在[本地计算机或远程计算机上验证应用包](#validate-your-app-package)，并[自动执行存储提交](#automate-store-submissions)。

    ![显示的程序包创建完成窗口，带有验证选项](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>手动创建应用包上传文件

1. 将以下文件放在文件夹中：

    - 一个或多个应用包（. .msix 或 .appx）或应用捆绑包（. .msixbundle 或 .appxbundle）。
    - .Appxsym 文件。 这是包含应用的公共符号的压缩 .pdb 文件，用于合作伙伴中心的[故障分析](https://docs.microsoft.com/windows/uwp/publish/health-report)。 您可以省略此文件，但如果您这样做，您的应用程序将不会有任何崩溃分析或调试信息。

2. 选择文件夹中的所有文件，右键单击文件，然后选择 "**发送到** -> **压缩（zipped）文件夹**"。

3. 将新的 zip 文件的扩展名从 .zip 更改为 msixupload 或 .appxupload。

## <a name="validate-your-app-package"></a>验证应用包

在将应用提交到合作伙伴中心以便在本地或远程计算机上进行认证之前验证应用。 你只能验证应用包的发布版本而不是调试版本。 有关将应用提交到合作伙伴中心的详细信息，请参阅[应用提交](https://docs.microsoft.com/windows/uwp/publish/app-submissions)。

### <a name="validate-your-app-package-locally"></a>本地验证应用程序包

1. 在 "**创建应用程序包**" 向导的 "最终**创建包**" 页中，保持选中 "**本地计算机**" 选项，然后单击 "**启动 Windows 应用程序认证包**"。 有关使用 Windows 应用认证工具包测试应用的详细信息，请参阅 [Windows 应用认证工具包](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)。

    Windows 应用程序认证工具包（WACK）执行各种测试并返回结果。 有关更具体的信息，请参阅 [Windows 应用认证工具包测试](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests)。

    如果你有想要用于测试的远程 Windows 10 设备，则需要在该设备上手动安装 Windows 应用认证工具包。 下一节将引导你完成这些步骤。 完成该操作后，你可以选择**远程计算机**并单击**启动 Windows 应用认证工具包**，以连接到远程设备并运行验证测试。

2. 当 WACK 完成并且你的应用已通过认证后，你就可以将你的应用程序提交到合作伙伴中心了。 请确保上载正确的文件。 文件的默认位置可以在解决方案的根文件夹中找到 `\[AppName]\AppPackages` 并且以 .appxupload 或 msixupload 文件扩展名结尾。 如果选择了已选择所有包体系结构的应用捆绑，则名称将采用格式 `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` 或 `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload`。

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>在远程 Windows 10 设备上验证应用包

1. 按照[启用设备进行开发](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)说明，启用 Windows 10 设备以进行开发。
    >[!IMPORTANT]
    > 你无法在适用于 Windows 10 的远程 ARM 设备上验证你的应用程序包。

2. 为 Visual Studio 下载和安装远程工具。 这些工具用于远程运行 Windows 应用认证工具包。 可以通过访问[远程计算机上的 RUN .msix applicationss](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015)，获取有关这些工具的详细信息，包括下载位置。

3. 下载所需的[Windows 应用程序认证工具包](https://go.microsoft.com/fwlink/p/?LinkID=309666)，并将其安装在远程 Windows 10 设备上。

4. 在向导的**程序包创建已完成**页面上，选择**远程计算机**选项按钮，然后选择**测试连接**按钮旁边的省略号按钮。
    >[!NOTE]
    > 仅当你至少选择了一个支持验证的解决方案配置时，"**远程计算机**" 选项按钮才可用。 有关使用 WACK 测试应用的详细信息，请参阅 [Windows 应用认证工具包](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)。

5. 指定你的子网内部的设备规格，或提供你的子网外部的设备的域名服务器 (DNS) 名称或 IP 地址。

6. 在**身份验证模式**列表中，如果你的设备没有要求你使用 Windows 凭据登录该设备，请选择**无**。

7. 选择**选择**按钮，然后选择**启动 Windows 应用认证工具包**按钮。 如果远程工具正在该设备上运行，Visual Studio 将与其连接，然后执行验证测试。 请参阅 [Windows 应用认证工具包测试](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests)。

## <a name="automate-store-submissions"></a>自动应用商店提交

从 Visual Studio 2019 开始，你可以通过选择 "[创建应用包" 向导](#create-your-app-package-upload-file-using-visual-studio)末尾的 " **Windows 应用程序认证包验证后自动提交到 Microsoft Store** " 选项，将生成的 .APPXUPLOAD 文件直接从 IDE 提交到 Microsoft Store。 此功能利用 Azure Active Directory 访问发布应用所需的合作伙伴中心帐户信息。 若要使用此功能，需要将 Azure Active Directory 与合作伙伴中心帐户关联，并检索提交所需的多个凭据。

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>将 Azure Active Directory 与合作伙伴中心帐户关联

在检索自动存储提交所需的凭据之前，必须先在[合作伙伴中心仪表板](https://partner.microsoft.com/dashboard)中执行这些步骤（如果尚未这样做）。

1. [将合作伙伴中心帐户与组织的 Azure Active Directory 相关联](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center)。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以在合作伙伴中心内创建新的 Azure AD 租户，无额外费用。

2. [将 Azure AD 应用程序添加到合作伙伴中心帐户](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account)。 此 Azure AD 应用程序表示将用于访问开发人员中心帐户的提交的应用或服务。 必须将此应用程序分配给**管理员**角色。 如果此应用程序已存在于你的 Azure AD 目录中，你可以在**添加 Azure AD 应用程序**页面上选择它，以将其添加到你的开发人员中心帐户。 如果没有此应用程序，你可以在“添加 Azure AD 应用程序”页面上创建新的 Azure AD 应用程序。

### <a name="retrieve-the-credentials-required-for-submissions"></a>检索提交所需的凭据

接下来，你可以检索提交所需的合作伙伴中心凭据： **Azure 租户 id**、**客户端 Id**和**客户端密钥**。

1. 请参阅[合作伙伴中心仪表板](https://partner.microsoft.com/dashboard)，并通过 Azure AD 凭据进行登录。

2. 在 "合作伙伴中心" 仪表板上，选择 "齿轮" 图标（位于仪表板的右上角附近），然后选择 "**开发人员设置**"。

3. 在左侧窗格的 "**设置**" 菜单中，单击 "**用户**"。

4. 单击你的 Azure AD 应用程序的名称以打开应用程序的设置。 在此页上，复制 "**租户 id** " 和 "**客户端 id** " 值。

5. 在 "**密钥**" 部分中，单击 "**添加新项**"。 在下一个屏幕上，复制与客户端密码对应的**密钥**值。 离开此页面后，你将无法再次访问此信息，因此请确保不会丢失它。 有关详细信息，请参阅[管理 Azure AD 应用程序的密钥](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application)。

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>在 Visual Studio 中配置自动存储提交

完成前面的步骤后，可以在 Visual Studio 2019 中配置自动存储提交。

1. 在 "[创建应用程序包" 向导](#create-your-app-package-upload-file-using-visual-studio)的末尾，选择 "**自动提交到 Windows 应用认证工具包验证后的 Microsoft Store** ，然后单击"**重新配置**"。

2. 在 "**配置 Microsoft Store 提交设置**" 对话框中，输入 AZURE 租户 Id、客户端 Id 和客户端密钥。

    ![配置 Microsoft Store 提交设置](images/packaging-screen8.jpg)

    > [!Important]
    > 可以将你的凭据保存到你的配置文件中，以便在将来提交时使用

3. 单击“确定”。

WACK 测试完成后，将启动该提交。 可以在 "**验证和发布**" 窗口中跟踪提交进度。

![验证和发布进度](images/packaging-screen9.jpg)

