---
Description: 可以使用 Azure Pipelines 为 MSIX 项目创建自动化生成。
title: 设置 CI/CD 管道以自动完成 MSIX 生成和部署
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 42f2486f0575cb1d7d15614ce65765388eaaa940
ms.sourcegitcommit: e650c86433c731d62557b31248c7e36fd90b381d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/02/2020
ms.locfileid: "82726444"
---
# <a name="set-up-a-cicd-pipeline-to-automate-your-msix-builds-and-deployments"></a>设置 CI/CD 管道以自动完成 MSIX 生成和部署

可以使用 Azure Pipelines 为 MSIX 项目创建自动化生成。 本文介绍如何在 Azure DevOps 中执行此操作。 此外，介绍如何使用命令行执行这些任务，以便可与任何其他生成系统相集成。

## <a name="create-a-new-azure-pipeline"></a>创建新的 Azure 管道

首先[注册 Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)（如果尚未这样做）。

接下来，创建一个可用于生成源代码的管道。 有关生成一个用于生成 GitHub 存储库的管道的教程，请参阅[创建第一个管道](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)。 Azure Pipelines 支持[此文](https://docs.microsoft.com/azure/devops/pipelines/repos)中列出的存储库类型。

若要设置实际生成管道，请浏览到 Azure DevOps 门户 (dev.azure.com/<organization>)，并创建一个新项目。 如果没有帐户，可以免费创建一个。 登录并创建项目后，可将源代码推送到系统设置的 Git 存储库 (https://<organization>@dev.azure.com/\<组织名称>/<project>/_git/<project>)，或使用任何其他提供程序，例如 GitHub。 在门户中依次单击“管道”按钮、“新建管道”创建新管道时，需要选择存储库的位置。

在接下来的“配置”屏幕上，应选择“现有 Azure Pipelines YAML 文件”选项，然后选择存储库中签入的 YAML 文件的路径。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>将项目证书添加到安全文件库

> [!NOTE]
>应尽量避免将证书提交到存储库，Git 默认会忽略这些证书。 为了管理敏感文件（例如证书）的安全处理，Azure DevOps 支持[安全文件](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)功能。

若要为自动生成上传证书：

1. 在 Azure Pipelines 中，展开导航窗格中的“管道”并单击“库”。  
2. 依次单击“安全文件”选项卡、“+ 安全文件”。  

  

3. 浏览到证书文件并单击“确定”。 
4. 上传证书后，选择该证书以查看其属性。 在“管道权限”下，将“授权在所有管道中使用”切换开关置于启用状态。  

5. 如果证书中的私钥包含密码，则我们建议将密码存储在 [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) 中，然后将密码链接到某个[变量组](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 可以使用该变量来从管道访问密码。 请注意，只有私钥支持密码；当前不支持使用本身受密码保护的证书文件。

> [!NOTE]
> 从 Visual Studio 2019 开始，不再在 MSIX 项目中生成临时证书。 若要创建或导出证书，请使用[此文](/windows/msix/package/create-certificate-package-signing)中所述的 PowerShell cmdlet。


## <a name="configure-the-build-in-your-yaml-file"></a>在 YAML 文件中配置生成

下表列出了可以定义用来设置生成管道的各个 MSBuild 参数。

|**MSBuild 参数**|**值**|**描述**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定义要存储生成的项目的文件夹。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 用于定义要包含在捆绑包中的平台。 |
| AppxBundle | 始终 | 使用 .msix/.appx 文件为指定的平台创建 .msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 生成要旁加载的 .msixupload/.appxupload 文件和 **_Test** 文件夹。 |
| UapAppxPackageBuildMode | CI | 仅生成 .msixupload/.appxupload 文件。 |
| UapAppxPackageBuildMode | SideloadOnly | 仅生成要旁加载的 **_Test** 文件夹。 |
| AppxPackageSigningEnabled | true | 启用包签名。 |
| PackageCertificateThumbprint | 证书指纹 | 此值**必须**与签名证书中的指纹匹配，或者为空字符串。 |
| PackageCertificateKeyFile | 路径 | 要使用的证书的路径。 此值是从安全文件元数据中检索的。 |
| PackageCertificatePassword | 密码 | 证书中私钥的密码。 建议将密码存储在 [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) 中，并将密码链接到[变量组](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 可将变量传递到此参数。 |



在使用 MSBuild 命令行生成打包项目（就像在 Visual Studio 中使用向导生成项目一样）之前，生成过程可以通过编辑 Package.appxmanifest 文件中 Package 元素的 Version 属性，来对生成的 MSIX 包进行版本控制。 在 Azure Pipelines 中，可以使用某个表达式来实现此目的，该表达式可以设置每次生成都会递增的计数器变量。也可以通过某个 PowerShell 脚本来实现此目的，该脚本使用 .NET 中的 System.Xml.Linq.XDocument 类来更改该属性的值。 

用于定义 MSIX 生成管道的示例 YAML 文件

```yml
pool: 
  vmImage: windows-2019
  
variables:
  buildPlatform: 'x86'
  buildConfiguration: 'release'
  major: 1
  minor: 0
  build: 0
  revision: $[counter('rev', 0)]
  
steps:
 - powershell: |
     # Update appxmanifest. This must be done before the build.
     [xml]$manifest= get-content ".\Msix\Package.appxmanifest"
     $manifest.Package.Identity.Version = "$(major).$(minor).$(build).$(revision)"    
     $manifest.save("Msix/Package.appxmanifest")
  displayName: 'Version Package Manifest'
  
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp
     /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:AppxPackageOutput=$(Build.ArtifactStagingDirectory)\MsixDesktopApp.msix /p:AppxPackageSigningEnabled=false'
  displayName: 'Package the App'
  
- task: DownloadSecureFile@1
  inputs:
    secureFile: 'certificate.pfx'
  displayName: 'Download Secure PFX File'
  
- script: '"C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\signtool"
    sign /fd SHA256 /f $(Agent.TempDirectory)/certificate.pfx /p secret $(
    Build.ArtifactStagingDirectory)/MsixDesktopApp.msix'
  displayName: 'Sign MSIX Package'
  
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
```




下面是 YAMl 文件中定义的不同生成任务的细分：

### <a name="configure-package-generation-properties"></a>配置包生成属性

以下定义设置生成组件的目录、平台，并定义是否生成捆绑包。 

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\"
/p:UapAppxPackageBuildMode=SideLoadOnly
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Never
```

### <a name="configure-package-signing"></a>配置包签名

若要为 MSIX（或 APPX）包签名，管道需要检索签名证书。 为此，请在 VSBuild 任务的前面添加 DownloadSecureFile 任务。
这样，就可以通过 ```signingCert``` 访问签名证书。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

接下来，更新 MSBuild 任务以引用签名证书：

```yml
- task: MSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Never 
                  p:UapAppxPackageBuildMode=SideLoadOnly 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> PackageCertificateThumbprint 参数被有意设置为空字符串，目的是引以注意。 如果在项目中设置指纹，但该指纹与签名证书不匹配，则生成将会失败并出现错误：`Certificate does not match supplied signing thumbprint`。

### <a name="review-parameters"></a>查看参数

使用 `$()` 语法定义的参数是在生成定义中定义的变量，在其他生成系统中将会更改。


若要查看所有预定义的变量，请参阅[预定义的生成变量](https://docs.microsoft.com/azure/devops/pipelines/build/variables)。

## <a name="configure-the-publish-build-artifacts-task"></a>配置“发布生成项目”任务

默认的 MSIX 管道不会保存生成的项目。 若要将发布功能添加到 YAML 定义，请添加以下任务。

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

可以在生成结果页的“项目”选项中查看生成的项目。 


## <a name="appinstaller-file-for-non-store-distribution"></a>非 Store 分发内容的 AppInstaller 文件


如果在 Store 的外部分发应用程序，可以利用 AppInstaller 文件来安装和更新包

一个可在 \\server\foo 上查找更新文件的 .appinstaller 文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller xmlns="http://schemas.microsoft.com/appx/appinstaller/2018"
              Version="1.0.0.0"
              Uri="\\server\foo\MsixDesktopApp.appinstaller">
  <MainPackage Name="MyCompany.MySampleApp"
               Publisher="CN=MyCompany, O=MyCompany, L=Stockholm, S=N/A, C=Sweden"
               Version="1.0.0.0"
               Uri="\\server\foo\MsixDesktopApp.msix"
               ProcessorArchitecture="x86"/>
  <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0" />
  </UpdateSettings>
</AppInstaller>
```

UpdateSettings 元素用于告知系统何时检查更新，以及是否强制用户更新。 可以在 bit.ly/2TGWnCR 上的文档中找到完整的架构参考，包括每个 Windows 10 版本支持的命名空间。

如果将 .appinstaller 文件添加到打包项目，并将其“打包操作”属性设置为“内容”，将“复制到输出目录”属性设置为“如果较新则复制”，则可以将另一个 PowerShell 任务添加到 YAML 文件，以便更新 root 和 MainPackage 元素的 Version 属性，并将更新的文件保存到暂存目录：

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $doc = [System.Xml.Linq.XDocument]::Load(
    "$(Build.SourcesDirectory)/Msix/Package.appinstaller")
  $version = "$(major).$(minor).$(build).$(revision)"
  $doc.Root.Attribute("Version").Value = $version;
  $xName =
    [System.Xml.Linq.XName]
      "{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Version").Value = $version;
  $doc.Save("$(Build.ArtifactStagingDirectory)/MsixDesktopApp.appinstaller")
displayName: 'Version App Installer File'
```

然后，将 .appinstaller 文件分发给最终用户，并让他们双击此文件而不是 .msix 文件来安装打包的应用。

## <a name="continuous-deployment"></a>持续部署

应用安装程序文件本身是未编译的 XML 文件，生成后可按需编辑该文件。 这样，在将软件部署到多个环境，以及想要将生成管道与发布过程区分开来时，就可以轻松地使用该文件。

如果在 Azure 门户中使用“空作业”模板创建发布管道，并使用最近设置的生成管道作为要部署的项目的源，则可以将 PowerShell 任务添加到发布阶段，以动态更改 .appinstaller 文件中两个 Uri 属性的值，来反映应用发布到的位置。

一个用于修改 .appinstaller 文件中的 URI 的发布管道任务

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $fileShare = "\\filesharestorageccount.file.core.windows.net\myfileshare\"
  $localFilePath =
    "$(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop\MsixDesktopApp.appinstaller"
  $doc = [System.Xml.Linq.XDocument]::Load("$localFilePath")
  $doc.Root.Attribute("Uri").Value = [string]::Format('{0}{1}', $fileShare,
    'MsixDesktopApp.appinstaller')
  $xName =
    [System.Xml.Linq.XName]"{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Uri").Value = [string]::Format('{0}{1}',
    $fileShare, 'MsixDesktopApp.appx')
  $doc.Save("$localFilePath")
displayName: 'Modify URIs in App Installer File'
```

在上述任务中，URI 设置为 Azure 文件共享的 UNC 路径。 由于在安装和更新应用时，OS 将在此位置查找 MSIX 包，因此，我还将另一个命令行脚本添加到了发布管道，该脚本首先将云中的文件共享映射到生成代理上的本地 Z:\ 驱动器，然后使用 xcopy 命令将 .appinstaller 和 .msix 文件复制到该驱动器：

```yml
- script: |
  net use Z: \\filesharestorageccount.file.core.windows.net\myfileshare
    /u:AZURE\filesharestorageccount
    3PTYC+ociHIwNgCnyg7zsWoKBxRmkEc4Aew4FMzbpUl/
    dydo/3HVnl71XPe0uWxQcLddEUuq0fN8Ltcpc0LYeg==
  xcopy $(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop Z:\ /Y
  displayName: 'Publish App Installer File and MSIX package'
```

如果你托管了自己的本地 Azure DevOps Server，当然可以将文件发布到自己的内部网络共享。

如果选择发布到 Web 服务器，可以通过在 YAML 文件中提供一些附加的参数，来告知 MSBuild 生成版本受控的 .appinstaller 文件和一个 HTML 页面，该页面包含下载链接，以及一些有关打包的应用的信息：

```yml
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:GenerateAppInstallerFile=True
/p:AppInstallerUri=http://yourwebsite.com/packages/ /p:AppInstallerCheckForUpdateFrequency=OnApplicationRun /p:AppInstallerUpdateFrequency=1 /p:AppxPackageDir=$(Build.ArtifactStagingDirectory)/'
  displayName: 'Package the App'
  ```
  
生成的 HTML 文件包含一个超链接，该链接以不区分浏览器的 ms-appinstaller 协议激活方案为前缀：

```html
<a href="ms-appinstaller:?source=
  http://yourwebsite.com/packages/Msix_x86.appinstaller ">Install App</a>
```

如果设置发布管道用于将放置文件夹的内容发布到 Intranet 或任何其他网站，且 Web 服务器支持字节范围请求并已正确配置，则最终用户可以使用此链接直接安装应用，而无需先下载 MSIX 包。


