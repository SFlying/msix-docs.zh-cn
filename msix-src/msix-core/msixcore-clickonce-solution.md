---
title: 从源代码通过 MSIX Core 创建 MSIX 包
description: 介绍如何使用 .MSIX Core 从源代码生成 .MSIX 包。
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10、windows 7、windows 8、Windows Server、uwp、.msix、msixcore、1709、1703、1607、1511、1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 4266b860b749c93f6916352185160a6ec5ecbbfa
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090275"
---
# <a name="create-an-msix-package-with-msix-core-from-source-code"></a>从源代码通过 MSIX Core 创建 MSIX 包

[.Msix Core](msixcore.md) 引入了 .msix 部署来选择以前版本的 Windows。 可以利用 .MSIX 核心安装程序，使用 ClickOnce 创建应用程序。 这样，用户就可以下载 setup.exe，并通过 .MSIX 核心安装程序安装 .MSIX 应用程序。

## <a name="host-your-app-on-a-web-server"></a>在 web 服务器上托管你的应用程序

若要使用 .MSIX 核心安装程序准备好应用程序，需要在 web 服务器上托管应用程序包。 本部分提供有关如何在 [Azure](#azure)上设置 web 应用、 [Internet Information Services (IIS) ](#internet-information-services-iis)和 [Amazon Web Services (AWS) ](#amazon-web-services-aws)的详细信息。

### <a name="azure"></a>Azure

若要使用此选项，必须拥有 Azure 订阅。 若要获取一个帐户，请参阅 [Azure 帐户页](https://azure.microsoft.com/free/)。

#### <a name="create-an-azure-web-app"></a>创建 Azure Web 应用

若要开始操作，请转到 " [Azure 门户" 页](https://portal.azure.com/) ，然后执行以下步骤：

1. 单击“创建资源”。
2. 单击 " **web** "，然后选择 " **web 应用**"。
3. 在 " **实例详细信息**" 下，创建一个唯一的应用名称，然后为应用选择适当的设置。 例如，你将需要在 **代码或 Docker 容器** 与 **运行时堆栈**之间进行选择。 否则，保留所有其他默认值。
4. 单击 " **创建** " 并完成向导。

#### <a name="host-the-app-package-and-the-web-page"></a>承载应用包和网页

1. 创建 web 应用后，选择该应用。
2. 在 " **开发工具**" 下，单击 **应用服务编辑器**。
3. 在编辑器中，有一个默认的 **hostingstart.html** 文件。 右键单击文件资源管理器的空白区域，然后选择 " **上传文件** " 以开始上传应用程序包。
4. 再次右键单击 "文件资源管理器" 面板的空白区域，然后选择 " **新建文件** " 以创建新文件。 将该文件命名为默认 HTML 页面所需的名称。

#### <a name="configure-the-web-app-for-app-package-mime-types"></a>为应用包 MIME 类型配置 web 应用

向 web 应用添加一个名为 Web.config 的新文件。 打开 Web.config 文件，并将以下 XML 添加到该文件。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extensions-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
    </staticContent>
  </system.webServer>
</configuration>
```

### <a name="internet-information-services-iis"></a>Internet 信息服务 (IIS)

IIS 是一个可选的 Windows 功能。 若要安装 IIS：

1. 单击 " **启动** "，然后搜索 **"打开或关闭 Windows 功能"**。
2. 选择 **Internet Information Services**。 
3. 另外，请确保安装 **ASP.NET 4.5** 或更高版本。 在 " **Windows 功能**" 对话框中，展开 " **Internet Information Services**  ->  **World Wide Web 服务**  ->  **应用程序开发功能**"，然后选择大于或等于**ASP.NET 4.5**的**ASP.NET**版本。
4. 单击 **"确定"** 开始安装。

Visual Studio 2017 (或更高版本) 和 Web 开发工具是必需的。 如果已安装 Visual Studio 2017 或更高版本，请确保已安装 ASP.NET 和 Web 开发工作负荷。 否则，请从 [此处](/visualstudio/install/install-visual-studio)安装 Visual Studio。

#### <a name="build-a-web-app"></a>构建 Web 应用

以管理员身份启动 Visual Studio，并使用空项目模板创建新的 **Visual c # Web 应用程序** 项目。

#### <a name="configure-iis-with-your-web-app"></a>将 IIS 配置为 Web 应用

1. 在 **解决方案资源管理器**中，右键单击根项目，然后选择 " **属性**"。 
2. 在 "属性" 中，选择 " **Web** " 选项卡。 
3. 在 " **服务器** " 部分，从下拉菜单中选择 " **本地 IIS** "，然后单击 " **创建虚拟目录**"。

#### <a name="add-the-app-package-to-the-web-application"></a>将应用程序包添加到 web 应用程序

将要分发的应用包添加到 web 应用程序：

1. 在 **解决方案资源管理器**中，右键单击项目节点。
2. 选择 "**添加**  ->  **新文件夹**"，并将文件夹命名为 "**包**"。 
3. 若要将应用包添加到文件夹，请右键单击 "包" 文件夹，然后选择 "**添加**  ->  **现有项**"。 浏览到应用包位置。

#### <a name="create-a-web-page"></a>创建 Web 页

根据需要创建 HTML 页面或任何其他 web 应用。 添加新 setup.exe 的链接。

#### <a name="configure-the-web-app-for-app-package-mime-types"></a>为应用包 MIME 类型配置 web 应用

从 "解决方案资源管理器" 打开 **Web.config** 文件，并在元素中添加以下 XML <configuration> 。

```xml
<system.webServer>
  <!--This is to allow the web server to serve resources with the appropriate file extensions-->
  <staticContent>
    <mimeMap fileExtension=".appx" mimeType="application/appx" />
    <mimeMap fileExtension=".msix" mimeType="application/msix" />
  </staticContent>
</system.webServer>
```

### <a name="amazon-web-services-aws"></a>Amazon Web Services (AWS)

若要使用此选项，必须具有 AWS 成员身份。 有关详细信息，请参阅 [AWS 帐户详细](https://aws.amazon.com/free/)信息。

#### <a name="create-an-amazon-s3-bucket-and-upload-your-msix-packages-and-web-pages"></a>创建 Amazon S3 存储桶并上传 .MSIX 包和网页

Amazon 简单存储服务 (S3) 是用于收集、存储和分析数据的 AWS 产品。 使用 S3 存储桶，可以方便地托管 Windows 10 应用包和网页以进行分发。

1. 登录到 AWS。 在 " **服务** " 下找到 " **S3**"。
2. 选择 " **创建存储桶** "，然后输入你的网站的 **存储桶名称** 。 按照提示设置属性和权限的对话框。 若要确保你的 Windows 10 应用可以从你的网站分发，请为你的 bucket 启用 " **读取** " 和 " **写入** " 权限，然后选择 " **授予此 bucket 的公共读取**权限"。 单击 " **创建存储桶** " 完成此步骤。
3. 完成后，将 .MSIX 包和网页上传到 S3 存储桶。

#### <a name="configure-the-web-app-for-app-package-mime-types"></a>为应用包 MIME 类型配置 web 应用

使用 web 服务接口（如 [S3 browser](https://s3browser.com/features-content-mime-types-editor.aspx)）   添加新的 **默认 HTTP 标头**。

1. 导航到 " **工具** "，然后选择 " **默认 HTTP 标头**"。
2. 在 **默认的 HTTP 标头** 对话框中，单击 " **添加**"。
3. 在 " **添加新的默认 HTTP 标头** " 对话框中，指定 bucket 名称、文件名、标头名称和标头值，然后单击 " **添加新标头**"。
    * **Bucket 名称**： .msix
    * **文件名**： *. .msix
    * **标头名称**： content-type
    * **标头值**： application/.msix

> [!NOTE]
> AWS 具有一些必须遵循的严格指导原则。 例如，Bucket 名称必须是唯一的，因此，如果使用上面的示例，则需要更改存储桶名称。

## <a name="use-the-msix-core-installer-to-build-the-clickonce-application"></a>使用 .MSIX 核心安装程序生成 ClickOnce 应用程序

查找应用程序应用程序 ClickOnce setup.exe。

### <a name="run-url-command-to-create-new-setupexe"></a>运行 URL 命令以创建新 setup.exe

请确保按照说明在 Visual Studio 中克隆、构建和发布 .MSIX Core 解决方案。

导航到下载 setup.exe 文件的目录，并运行以下命令：

```PowerShell
setup-exe - url=<location of your msix in the webservice>
```

### <a name="sign-the-application"></a>签署应用程序

由于上一步骤创建了新的 setup.exe，因此你将需要再次对应用进行签名，以验证你是应用程序的受信任发布者，并建立应用程序的完整性。 你可以使用 [SignTool](/dotnet/framework/tools/signtool-exe) 并提供你的证书。

### <a name="distribute-the-application-to-your-users"></a>向用户分发应用程序

你现在可以使用其网站上的链接或下载按钮指向新 setup.exe。 .MSIX Core 面向 Windows 10 版本1703及更早版本上的用户。 [应用程序安装程序](../app-installer/installing-windows10-apps-web.md)是适用于 Windows 1709 或更高版本上的 .msix 包的理想安装过程。 应用安装程序可优化使用者端的磁盘空间，并可直接从 HTTP 位置安装应用。 .MSIX 核心将检测使用者是否在 Windows 1709 或更高版本上，并将其重定向到应用安装程序。 

在 Microsoft Edge 中，可以调用 [getHostEnvironmentValue ( # B1 ](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/mt795399(v=vs.85)) 方法，返回值中的 *os-生成* 字段将指定该用户的操作系统版本。 然后，你可以在此处提示安装过程，使用适用于 windows 10 版本1703和) 更低版本的 .MSIX Core (、适用于 Windows 10 版本1709和) 更高版本的应用程序安装 (。

## <a name="user-experience"></a>用户体验

用户只需从开发人员的网页下载并运行 setup.exe。

* 如果用户 setup.exe 运行时尚未安装 .MSIX 核心安装程序，则用户将看到 ClickOnce 提示符，并单击 " **安装** " 以安装 .msix Core 安装程序。 安装程序将自动启动并显示开发人员的查询字符串中指定的 .MSIX 包的安装屏幕，以便用户可以安装应用程序。
* 如果 .MSIX 核心安装程序已在用户运行 setup.exe 时安装，则 .MSIX Core 安装程序会自动启动，并显示在用户安装应用程序的查询字符串中指定的 .MSIX 包的安装屏幕。