---
description: 本文介绍如何使用 Device Guard 签名对 .MSIX 包进行签名，使企业能够保证应用程序来自受信任的源。
title: 使用 Device Guard 签名功能对 MSIX 包进行签名
ms.date: 08/27/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 8b3324b396cb08344ab677dfa95f481b4800113d
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090975"
---
# <a name="sign-an-msix-package-with-device-guard-signing"></a>使用 Device Guard 签名功能对 MSIX 包进行签名

> [!IMPORTANT]
> 我们引入了新版本的 Device Guard 签名服务 (DGSS) ，以使其更易于自动化。 新版本的服务将于2020年9月开始使用 (DGSS v2) ，并将在12月2020结束后转换为 DGSS v2。 2020年12月结束时，当前版本的 DGSS 服务的现有基于 web 的机制将会停用，不再可用。 请计划在9月到12月2020之间迁移到服务的新版本。
>
> 下面是我们对服务所做的主要更改： 
> - 使用该服务的方法将更改为基于 PowerShell cmdlet 的更多自动化易懂方法。 这些 cmdlet 将以 NuGet 下载的形式提供。
> - 为了实现所需的隔离，你将需要从 DGSS v2 获取新的 CI 策略 (并根据需要对其进行) 签名。 
> - DGSS v2 将不支持下载用于对文件进行签名的叶证书 (但是，根证书仍可用于下载) 。  请注意，用于对文件进行签名的证书可以轻松地从签名的文件本身提取。  因此，在2020年12月结束后，DGSS v1 终止后，将无法再下载用于对文件进行签名的叶证书。
>
> 以下 PowerShell cmdlet 将提供以下功能：
> - 获取 CI 策略
> - 签署 CI 策略
> - 为目录签名 
> - 下载根证书
> - 下载签名操作的历史记录 
>
> 我们将在2020年9月之前共享详细的说明和 NuGet 位置。 如有任何疑问，请通过与我们联系 DGSSMigration@microsoft.com 获取有关迁移的详细信息。  


[Device guard 签名](/microsoft-store/device-guard-signing-portal) 是一项 device guard 功能，可用于商业和教育 Microsoft Store。 它使企业能够保证每个应用都来自受信任的来源。 从 Windows 10 预览体验版18945开始，可以使用 Windows SDK 中的 SignTool，通过设备保护签名对 .MSIX 应用进行签名。 此功能支持使你能够轻松地将设备防护登录纳入 .MSIX 包生成和签名工作流。

Device Guard 签名需要 Microsoft Store for Business 中的权限，并使用 Azure Active Directory (AD) 身份验证。 若要使用 Device Guard 签名对 .MSIX 包进行签名，请按照下列步骤操作。

1. 如果尚未这样做，请 [注册 Microsoft Store For Business 或 Microsoft Store 教育](/microsoft-store/sign-up-microsoft-store-for-business)。
    > [!NOTE]
    > 只需使用此门户配置 Device Guard 签名的权限。
2. 在 Microsoft Store for Business (或 Microsoft Store 教育) 中，为自己分配执行 Device Guard 签名所需权限的角色。
3. 使用正确的设置在 [Azure 门户](https://portal.azure.com/) 中注册应用，以便可以将 Azure AD 身份验证用于企业的 Microsoft Store。
4. 获取 JSON 格式的 Azure AD 访问令牌。
5. 运行 SignTool，使用 Device Guard 签名对 .MSIX 包进行签名，并传递在上一步中获取的 Azure AD 访问令牌。

以下部分更详细地介绍了这些步骤。

## <a name="configure-permissions-for-device-guard-signing"></a>配置 Device Guard 签名的权限

若要在 Microsoft Store for Business 或 Microsoft Store 教育中使用 Device Guard 签名，需要 **Device guard 签名者** 角色。 这是可以签名的最小特权角色。 其他角色（如 **全局管理员** 和 **计费帐户所有者** ）也可以对其进行签名。 

 > [!NOTE]
 > 当你作为应用进行签名时，将使用 Device Guard 签名者角色。 当你作为已登录用户进行签名时，将使用全局管理员和计费帐户所有者。

确认或重新分配角色：

1. 请登录[适用于企业的 Microsoft Store](https://businessstore.microsoft.com/)。
2. 选择 " **管理** "，然后选择 " **权限**"。
3. 查看 **角色**。

有关详细信息，请参阅[适用于企业和教育的 Microsoft Store 中的角色和权限](/microsoft-store/roles-and-permissions-microsoft-store-for-business)。

## <a name="register-your-app-in-the-azure-portal"></a>在 Azure 门户中注册你的应用

使用正确的设置注册应用，以便可以将 Azure AD 身份验证用于业务 Microsoft Store：

1. 登录到 [Azure 门户](https://portal.azure.com/) 并按照 [快速入门：向 Microsoft 标识平台注册应用程序](/azure/active-directory/develop/quickstart-register-app) 中的说明注册将使用 Device Guard 签名的应用程序。

    > [!NOTE]
    > 在 " **重定向 URI** " 部分中，建议选择 " **公用客户端 (移动 & 桌面) **"。 否则，如果你选择 " **Web** " 作为 "应用类型"，则在此过程中稍后获取 Azure AD 访问令牌时，你将需要提供 [客户端密码](/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-credentials-to-your-web-application) 。

2. 注册应用后，在应用的 "Azure 门户中，单击" **api 权限**"，在" **我的组织使用的 api** "下，并添加适用于 **企业的 Windows 应用商店 API**的权限。

3. 接下来，选择 " **委托的权限** "，然后选择 " **user_impersonation**"。

## <a name="get-an-azure-ad-access-token"></a>获取 Azure AD 访问令牌

接下来，获取 JSON 格式的 Azure AD 应用的 Azure AD 访问令牌。 您可以使用各种编程和脚本语言来实现此目的。 有关此过程的详细信息，请参阅 [使用 OAuth 2.0 代码授予流授予对 Azure Active Directory web 应用程序的访问权限](/azure/active-directory/develop/v1-protocols-oauth-code)。 由于访问令牌将在一小时后过期，因此我们建议你同时检索 [刷新令牌](/azure/active-directory/develop/v1-protocols-oauth-code#refreshing-the-access-tokens) 和访问令牌。

> [!NOTE]
> 如果在 Azure 门户中将应用注册为 **Web** 应用，则在请求令牌时必须提供客户端机密。 有关详细信息，请参阅上一部分。

下面的 PowerShell 示例演示如何请求访问令牌。

```powershell
function GetToken()
{

    $c = Get-Credential -Credential $user
    
    $Credentials = New-Object System.Management.Automation.PSCredential -ArgumentList $c.UserName, $c.password
    $user = $Credentials.UserName
    $password = $Credentials.GetNetworkCredential().Password
    
    $tokenCache = "outfile.json"

    #replace <application-id> and <client_secret-id> with the Application ID from your Azure AD application registration
    $Body = @{
      'grant_type' = 'password'
      'client_id'= '<application-id>'
      'client_secret' = '<client_secret>'
      'resource' = 'https://onestore.microsoft.com'
      'username' = $user
      'password' = $password
    }

    $webpage = Invoke-WebRequest 'https://login.microsoftonline.com/common/oauth2/token' -Method 'POST'  -Body $Body -UseBasicParsing
    $webpage.Content | Out-File $tokenCache -Encoding ascii
}
```

> [!NOTE]
> 我们建议你保存 JSON 文件以供以后使用。

## <a name="sign-your-package"></a>对包进行签名

拥有 Azure AD 访问令牌后，就可以使用 SignTool 通过 Device Guard 签名对包进行签名。 有关使用 SignTool 对包进行签名的详细信息，请参阅 [使用 SignTool 对应用包进行签名](/windows/uwp/packaging/sign-app-package-using-signtool?context=%252fwindows%252fmsix%252frender#prerequisites)。

以下命令行示例演示如何使用 Device Guard 签名对包进行签名。

```cmd
signtool sign /fd sha256 /dlib DgssLib.dll /dmdf <Azure AAD in .json format> /t <timestamp-service-url> <your .msix package>
```

> [!NOTE]
> * 建议你在对包进行签名时使用一个时间戳选项。 如果不应用 [时间戳](signing-package-overview.md#timestamping)，签名将在一年后过期，应用将需要重新签名。
> * 确保包清单中的发布者名称与用来对包进行签名的证书匹配。 利用此功能，它将是你的叶证书。 例如，如果 "叶证书" 是 " **公司**名称"，则清单中的发布者名称必须是 **CN = "公司**名称"。 否则，签名操作将失败。
> * 仅支持 SHA256 算法。
> * 当你用 Device Guard 签名对包进行签名时，你的包将不是通过 Internet 发送的。

## <a name="test"></a>测试

若要测试 Device Guard 签名，请从业务门户 Microsoft Store 下载证书。

1. 请登录[适用于企业的 Microsoft Store](https://businessstore.microsoft.com/)。
2. 选择 " **管理** "，然后选择 " **设置**"。
3. 查看 **设备**。
4. 查看 **下载你的组织的根证书以便与 Device Guard 一起使用**
5. 单击 "**下载**" 

在设备上将根证书安装到 **受信任的根** 证书颁发机构。 安装新签名的应用，以验证是否已使用 Device Guard 签名成功对应用进行签名。

## <a name="common-errors"></a>常见错误

下面是你可能会遇到的常见错误。

* 0x800700d：此常见错误表示 Azure AD JSON 文件的格式无效。
* 在下载 Device Guard 签名的根证书之前，你可能需要接受 Microsoft Store for Business 的条款和条件。 可以通过在门户中获取免费应用来完成此操作。