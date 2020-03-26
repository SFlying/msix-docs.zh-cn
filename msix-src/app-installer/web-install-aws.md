---
title: 从 AWS Web 服务分发 Windows 10 应用
description: 设置 AWS web 服务器以通过应用安装程序应用验证应用安装的教程
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10、Windows 10、UWP、应用安装程序、AppInstaller、旁加载、相关集、可选包、AWS
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 4e999f4939432c6490da380c722c31ae87d669d8
ms.sourcegitcommit: f5936c95c0f5b6f080e51b8d47a7cd62ccf6a600
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80241954"
---
# <a name="distribute-a-windows-10-app-from-an-aws-web-service"></a>从 AWS Web 服务分发 Windows 10 应用

通过应用安装程序，开发人员和 IT 专业人员可以通过在各自的内容分发网络 (CDN) 上托管应用的方式来分发 Windows 10 应用。 这种方式适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业。

本主题概述了配置 Amazon Web Services （AWS）网站以托管 Windows 10 应用包的步骤，以及如何使用应用安装程序应用安装应用程序包。

## <a name="setup"></a>安装

要成功学习此教程，你将需要以下内容：
 
1. AWS 订阅 
2. 网页
3. Windows 10 应用包-将分发的应用包

可选：GitHub 上的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)。 如果没有应用包或网页可以使用，但仍想学习如何使用此功能，这个初学者项目很有用。

本教程将介绍如何在 AWS 上设置网页和主机包。 这将需要 AWS 订阅。 根据操作的规模，可以使用其免费成员身份来完成本教程。 

## <a name="step-1---aws-membership"></a>步骤 1-AWS 成员身份
若要获取 AWS 成员身份，请访问[AWS 帐户详细信息页](https://aws.amazon.com/free/)。 可以使用免费会员资格获得此教程。

## <a name="step-2---create-an-amazon-s3-bucket"></a>步骤 2-创建 Amazon S3 存储桶

Amazon 简单存储服务（S3）是用于收集、存储和分析数据的 AWS 产品。 使用 S3 存储桶，可以方便地托管 Windows 10 应用包和网页以进行分发。 

用凭据登录到 AWS 后，请在 "`Services` 查找" `S3`。 

选择 "**创建存储桶**"，然后输入你的网站的**存储桶名称**。 按照提示设置属性和权限的对话框。 若要确保你的 Windows 10 应用可以从你的网站分发，请为你的 bucket 启用 "**读取**" 和 "**写入**" 权限，然后选择 "**授予此 bucket 的公共读取**权限"。

![设置 Amazon S3 存储桶上的权限](images/aws-permissions.png) 

查看摘要，确保所选选项已反映。 单击 "**创建存储桶**" 完成此步骤。 

## <a name="step-3---upload-windows-10-app-package-and-web-pages-to-an-s3-bucket"></a>步骤 3-将 Windows 10 应用包和网页上传到 S3 存储桶

你已创建一个 Amazon S3 存储桶，你将能够在 Amazon S3 视图中看到它。 下面是演示存储桶外观的示例：

![Amazon S3 bucket 视图的屏幕截图](images/aws-post-create.png)

现在，我们已准备好上传要在 Amazon S3 存储桶中托管的应用包和网页。 

单击新创建的存储桶上传内容。 Bucket 当前为空，因为尚未上传任何内容。 单击 "**上传**" 按钮，然后选择要上传的应用程序包和网页文件。

> [!NOTE]
> 如果没有可用的应用包，可以使用 GitHub 上提供的[初学者项目](https://github.com/AppInstaller/MySampleWebApp)库中包含的应用包。 该应用包签名所用的证书 (MySampleApp.cer) 也随 GitHub 上的示例提供。 在安装应用之前，必须在设备上安装该证书。

![上载应用包 UX 的屏幕截图](images/aws-upload-package.png)

与创建 Amazon S3 存储桶的权限类似，存储桶中的内容还必须具有对此对象权限的**读取**、**写入**和**授予公共读取访问**权限。

如果要测试是否上传网页，但没有，则可以使用[初学者项目](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html)中的示例 html 页面（.html）。

> [!IMPORTANT]
> 在上传网页之前，请确认网页中的应用程序包引用正确。 

若要获取应用程序包引用，请先上传应用程序包，然后复制应用包 URL。 编辑 html 网页以反映正确的应用程序包路径。 有关更多详细信息，请参阅代码示例。 

选择上传的应用程序包文件，获取应用程序包的引用链接。 

**将链接复制**到应用包，并在网页中添加引用。 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.msixbundle"> Install My Sample App</a>
    </body>
</html>
```
将 html 文件上传到 Amazon S3 存储桶。 请记住，将权限设置为**read**允许读**写**访问。

## <a name="step-4---test"></a>步骤 4-测试

将网页上传到 Amazon S3 存储桶中后，通过选择上传的 html 文件来获取指向网页的链接。

使用链接打开网页。 由于我们设置权限以授予对应用包和网页的公共访问权限，因此任何具有网页链接的用户都可以使用应用安装程序访问它和安装 Windows 10 应用包。 请注意，应用安装程序属于 Windows 10 平台。 作为开发人员，无需向应用程序添加任何其他代码或功能即可使用应用程序安装程序。 

## <a name="troubleshooting"></a>故障排除

### <a name="app-installer-fails-to-install"></a>应用安装程序无法安装 

如果设备上未安装用于对应用程序包进行签名的证书，应用安装将失败。 若要解决此问题，需要在安装应用之前安装该证书。 如果要托管公用分发的应用包，建议使用证书颁发机构颁发的证书对应用包进行签名。 

