---
title: 从 IIS 服务器分发 Windows 10 应用
description: 本教程演示如何设置 IIS 服务器，验证你的 web 应用程序是否可以托管应用包，并有效地调用和使用应用安装程序。
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10，uwp，应用安装程序，AppInstaller，旁加载，相关集，可选程序包，IIS 服务器
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 386a8fe4323b7c0a7f1898e282d93912ff28e906
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090345"
---
# <a name="distribute-a-windows-10-app-from-an-iis-server"></a>从 IIS 服务器分发 Windows 10 应用

本教程演示如何设置 IIS 服务器，验证你的 web 应用程序是否可以托管应用包，并有效地调用和使用应用安装程序。

通过应用安装程序，开发人员和 IT 专业人员可以通过在各自的内容分发网络 (CDN) 上托管应用的方式来分发 Windows 10 应用。 这种方式适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业。 

## <a name="setup"></a>安装

若要成功完成本教程，你将需要以下各项：

1. Visual Studio 2017  
2. Web 开发工具和 IIS 
3. Windows 10 应用包-将分发的应用包

可选：GitHub 上的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)。 如果你没有要使用的应用包，但仍想要了解如何使用此功能，这会很有帮助。

## <a name="step-1---install-iis-and-aspnet"></a>步骤 1-安装 IIS 和 ASP.NET 

[Internet Information Services](https://www.iis.net/) 是一种可以通过 "开始" 菜单安装的 Windows 功能。 在 " **开始" 菜单** 中，搜索 **打开或关闭 Windows 功能**。

找到并选择 " **Internet Information Services** " 以安装 IIS。

> [!NOTE]
> 不需要选中 Internet Information Services 下的所有复选框。 只有在您检查 **Internet Information Services** 时选择的才足够。

还需要安装 ASP.NET 4.5 或更高版本。 若要安装它，请查找 **Internet Information Services > World Wide Web 服务-> 应用程序开发功能**。 选择一个大于或等于 ASP.NET 4.5 的 ASP.NET 版本。

![安装 ASP.NET 功能的屏幕截图](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>步骤 2-安装 Visual Studio 2017 和 Web 开发工具 

[安装 Visual Studio 2017](/visualstudio/install/install-visual-studio) （如果尚未安装）。 如果已有 Visual Studio 2017，请确保已安装以下工作负荷。 如果你的安装中没有工作负荷，请使用 "开始" 菜单中的 Visual Studio 安装程序 (") "。  

在安装过程中，请选择 " **ASP.NET" 和 "Web 开发** " 以及你感兴趣的任何其他工作负荷。 

安装完成后，启动 Visual Studio 并创建一个新项目 (**文件**"  ->  **新建项目**") 。

## <a name="step-3---build-a-web-app"></a>步骤 3-构建 Web 应用

以**管理员身份**启动 Visual Studio 2017，并使用**空**项目模板创建新的**Visual c # Web 应用程序**项目。 

![创建新的 web 项目的屏幕截图](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>步骤 4-通过 Web 应用配置 IIS 

在解决方案资源管理器中，右键单击根项目，然后选择 " **属性**"。

在 "web 应用属性" 中，选择 " **web** " 选项卡。在 " **服务器** " 部分中，从下拉菜单中选择 " **本地 IIS** "，然后单击 " **创建虚拟目录**"。 

![项目属性中的 "web" 选项卡的屏幕截图](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>步骤 5-将应用包添加到 web 应用程序 

将要分发的应用包添加到 web 应用程序。 如果没有可用的应用包，你可以使用 GitHub 上提供的 [初学者项目包](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) 的一部分的应用包。 该应用包签名所用的证书 (MySampleApp.cer) 也随 GitHub 上的示例提供。 安装应用之前，必须先将证书安装到设备 (步骤 9) 。

在初学者项目 web 应用程序中，已将一个新文件夹添加到名为的 **包** ，其中包含要分发的应用包。 若要在 Visual Studio 中创建文件夹，请在解决方案资源管理器中右键单击项目节点，选择 "**添加**" "  ->  **新建文件夹**" 并将其命名为 "**包**"。 若要将应用包添加到文件夹，请右键单击 "**包**" 文件夹，然后选择 "**添加**  ->  **现有项 ...** " 并浏览到应用包位置。 

![添加包的屏幕截图](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>步骤 6-创建网页

此示例 web 应用使用简单的 HTML。 根据需要，你可以根据需要随意构建你的 web 应用。 

右键单击 "解决方案资源管理器" 的根项目，选择 "**添加**  ->  **新项**"，然后从**Web**部分添加一个新的**HTML 页**。

创建 HTML 页面后，右键单击 "解决方案资源管理器中的 HTML 页面，然后选择" **设为起始页**"。  

双击 HTML 文件以在代码编辑器窗口中将其打开。 在本教程中，将只使用网页中所需的元素来成功调用应用安装程序应用程序以安装 Windows 10 应用。 

将以下 HTML 代码包含在网页中。 成功调用应用安装程序的关键是使用应用安装程序向操作系统注册的自定义方案： `ms-appinstaller:?source=` 。 有关更多详细信息，请参阅下面的代码示例。

> [!NOTE]
> 确保自定义方案之后指定的 URL 路径与 VS 解决方案的 "web" 选项卡中的项目 Url 匹配。
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.msixbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>步骤 7-为应用包 MIME 类型配置 web 应用

从 "解决方案资源管理器" 打开 **Web.config** 文件，并在元素中添加以下行 `<configuration>` 。 

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
</system.webServer>
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>步骤 8-为应用安装程序添加环回例外

由于网络隔离，应用安装程序等 Windows 10 应用程序限制为使用 IP 环回地址，如 http://localhost/ 。 使用本地 IIS 服务器时，必须将应用程序安装程序添加到环回豁免列表。 

为此，请以**管理员身份**打开**命令提示符**，然后输入以下命令：
```Command Line
CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

若要验证是否已将应用添加到免除列表中，请使用以下命令在环回豁免列表中显示应用： 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

你应 `microsoft.desktopappinstaller_8wekyb3d8bbwe` 在列表中找到。

完成通过应用安装程序安装应用程序的本地验证后，可以通过以下操作删除在此步骤中添加的环回豁免：

```Command Line
CheckNetIsolation.exe LoopbackExempt -d -n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## <a name="step-9---run-the-web-app"></a>步骤 9-运行 Web 应用 

通过单击 VS 功能区上的 "运行" 按钮生成并运行 web 应用程序，如下图所示：

![在 Visual Studio 中运行 web 应用的屏幕截图](images/run.png)

将在浏览器中打开网页：

![从网页安装应用程序的屏幕截图](images/web-page.png)

单击网页中的链接以启动应用安装程序应用程序并安装 Windows 10 应用包。


## <a name="troubleshooting-issues"></a>对问题进行故障排除

### <a name="not-sufficient-privilege"></a>权限不足 

如果在 Visual Studio 中运行 web 应用时显示错误，如 "你没有足够的权限访问计算机上的 IIS 网站"，则需以管理员身份运行 Visual Studio。 关闭 Visual Studio 的当前实例，并将其重新打开为管理员。

### <a name="set-start-page"></a>设置起始页 

如果运行 web 应用会导致浏览器加载 HTTP 403.14-禁止错误，这是因为 web 应用没有定义的起始页。 请参阅本教程中的步骤6，了解如何定义起始页。