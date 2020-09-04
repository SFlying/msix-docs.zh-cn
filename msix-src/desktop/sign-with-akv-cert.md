---
title: 使用 Azure Key Vault 为包签名
description: 本文介绍了如何使用 Azure Key Vault 中的证书为应用包签名。
ms.date: 05/07/2020
ms.topic: article
keywords: windows 10, msix, uwp, azure key vault, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: ac69ae105dbd1fff5d64c20f50af644f9d27504f
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090425"
---
# <a name="sign-packages-with-azure-key-vault"></a>使用 Azure Key Vault 为包签名

在 Studio 2019 版本 16.6 预览版 3 及更高版本中，可在开发和测试场景中使用 Azure Key Vault 中存储的证书对通用 Windows 平台 (UWP) 和桌面应用包进行签名。 该工具会从 Azure Key Vault 中提取公钥和私钥，并将它们加载到开发计算机上的证书存储中，以便通过 SignTool.exe 对包进行签名。

> [!IMPORTANT]
> 本文所述的流程仅适用于开发和测试场景。 就用于分发的私钥而言，该流程不被认为是最佳做法。 要确保最佳安全做法，用于分发的私钥应仅由连续集成和持续部署 (CI/CD) 平台推荐的工具进行处理。

## <a name="prerequisites"></a>先决条件

- 一个 Azure 帐户。 如果还没有 Azure 帐户，请在[此处](https://azure.microsoft.com/free/)开始。
- 一个 Azure Key Vault。 有关详细信息，请参阅[创建 Key Vault](/azure/key-vault/secrets/quick-create-portal#create-a-vault)。
- 一个导入到 Azure Key Vault 的有效包签名证书。 Azure Key Vault 生成的默认证书不能用于代码签名。 要详细了解如何创建包签名证书，请参阅[为包签名创建证书](../package/create-certificate-package-signing.md)。

## <a name="import-a-certificate-to-your-key-vault"></a>将证书导入 Key Vault

将证书添加到 Key Vault 非常简单。 在本例中，我们添加一个名为 UwpSigningCert.pfx 的有效 UWP 代码签名证书。

1. 在 Key Vault 属性页上，选择“证书”。
2. 单击“生成/导入”。
3. 在“创建证书”屏幕上，选择以下值：
    - **证书创建方法**：导入
    - **证书名称**：UwpSigningCert
    - **上传证书文件**：UwpSigningCert.pfx
    - **解密证书**：如果证书受密码保护，请在“密码”字段提供。
4. 单击“创建”。

> [!NOTE]
> Windows 将不信任该自签名证书，除非该证书已导入且受到管理员的信任。 确保所有证书（包括自签名证书）安全。

## <a name="configure-the-access-policies-for-your-key-vault"></a>为 Key Vault 配置访问策略

可使用访问策略控制谁有权访问 Key Vault 的内容。 Key Vault 访问策略分别授予对密钥、机密和证书的权限。 可仅授权用户访问密钥，但禁止访问机密。 密钥、机密和证书的访问权限在保管库级别进行管理。 有关详细信息，请参阅 [Azure Key Vault 安全性](/azure/key-vault/general/overview-security#identity-and-access-management)。

> [!NOTE]
> 在 Azure 订阅中创建 Key Vault 时，它会自动与订阅的 Azure Active Directory 租户相关联。 任何尝试管理或检索 Key Vault 中的内容的用户都必须经过 Azure AD 授权。

1. 在 Key Vault 属性页上，选择“访问策略”。
2. 选择“+添加访问策略”。
3. 单击“密钥权限”下拉列表，然后勾选“密钥管理操作”下的“获取”和“列出”框   。
4. 单击“选择主体”，搜索要向其授予访问权限的用户，然后单击“选择” 。
5. 单击“添加”。
6. 请确保单击“保存”来保存所作的更改。

> [!NOTE]
> 建议不要授权用户直接访问 Key Vault。 理想情况是，应将用户添加到 Azure AD 组，转而向该组授予对 Key Vault 的访问权限。

## <a name="select-a-certificate-from-your-key-vault-in-visual-studio"></a>从 Visual Studio 中的 Key Vault 选择一个证书

通过 Visual Studio 中的“创建应用包”向导，可选择要用于对应用包进行签名的证书。 可通过 Azure Key Vault 选择包签名证书。 必须提供包含该证书的 Key Vault 的 URI，而且 Visual Studio 中授权的 Microsoft 帐户必须具有访问它的适当权限。

1. 在 Visual Studio 中打开“UWP 应用程序”项目或“Windows 应用程序打包项目”桌面 。
2. 选择“发布” -> “包” -> “创建应用包…”，打开“创建应用包”向导   。
3. 在“选择分发方法”页面上，选择“旁加载” 。
4. 在“选择签名方法”页面上，单击“从 Azure Key Vault 中选择…” 。
5. 在“从 Azure Key Vault 选择证书”对话框出现后，使用帐户选取器选择已为其配置访问策略的帐户。
6. 输入 Key Vault 的 URI。 URI 可在 Key Vault 的“概述”页面上找到，它通过 DNS 名称进行标识 。
7. 单击“查看元数据”按钮。
8. 证书加载完毕后，从列表中选择所需的证书，例如 UwpSigningCert。
9. 单击“确定”。

> [!NOTE]
> 该证书将导入到你的本地证书存储中，它将在此处用于包签名。

## <a name="decrypt-your-certificate-with-a-password-from-azure-key-vault"></a>使用 Azure Key Vault 中的密码来解密证书

如果使用受本地密码保护的证书 (.pfx) 来签名应用包，则可能很难管理用来解密它的密码。 在“创建应用包”向导中导入证书时，系统将提示你手动输入密码。 此外，也可选择 Azure Key Vault 中的密码。

1. 在 Visual Studio 中打开“UWP 应用程序”项目或“Windows 应用程序打包项目”桌面 。
2. 选择“发布” -> “包” -> “创建应用包…”，打开“创建应用包”向导   。
3. 在“选择分发方法”页面上，选择“旁加载” 。
4. 在“选择签名方法”页面上，单击“从文件中选择…” 。
5. 在“证书受密码保护”对话框出现后，单击“从 Key Vault 中选择密码” 。
6. 在“从 Azure Key Vault 选择密码”对话框出现后，使用帐户选取器选择已为其配置访问策略的帐户。
7. 输入 Key Vault 的 URI。 URI 可在 Key Vault 的“概述”页面上找到，它通过 DNS 名称进行标识 。
8. 单击“查看元数据”按钮。
9. 密码加载完毕后，从列表中选择所需的密码，例如 UwpSigningCertPassword。
10. 单击“确定”。