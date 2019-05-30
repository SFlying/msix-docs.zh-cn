---
title: 使用 MSIX SDK 将在非 Windows 10 平台上的 MSIX 包分发
description: 使用 SDK 来的跨平台使用的包 MSIX 程序包开发人员指南
author: mcleanbyron
ms.author: mcleans
ms.date: 12/13/2018
ms.topic: article
ms.custom: RS5
ms.openlocfilehash: 64a780bbc7ef4edc6e2d0c829c7304fdf39762f7
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795230"
---
# <a name="use-the-msix-sdk-to-distribute-an-msix-package-on-non-windows-10-platforms"></a>使用 MSIX SDK 将在非 Windows 10 平台上的 MSIX 包分发

MSIX SDK 开发人员提供分发到客户端设备，而不考虑客户端设备上的操作系统平台的包内容的通用方法。 这使开发人员能够一次而不是让其应用程序内容打包到为每个平台的包。

若要充分利用 MSIX SDK 和分发到多个平台在包内容的能力，我们提供一种方法来指定想将包解压缩到的目标平台。 这意味着您可以确保包的内容，从包仅按需提取。

下表显示了在清单中声明的目标设备系列。

<table class="tg">
  <tr>
    <th class="tg-yw4l">平台</th>
    <th class="tg-yw4l">系列</th>
    <th class="tg-yw4l" colspan="3">目标设备系列</th>
    <th class="tg-yw4l">说明</th>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="6">Windows 10</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-031e" rowspan="24"><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>Platform.All<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br></td>
    <td class="tg-baqh" rowspan="6">Windows.Universal</td>
    <td class="tg-yw4l">Windows.Mobile</td>
    <td class="tg-yw4l">移动设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l">桌面设备</td>
    <td class="tg-yw4l">Windows.Desktop</td>
    <td class="tg-yw4l">PC</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xbox</td>
    <td class="tg-yw4l">Windows.Xbox</td>
    <td class="tg-yw4l">Xbox 控制台</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Surface Hub</td>
    <td class="tg-yw4l">Windows.Team</td>
    <td class="tg-yw4l">大屏幕 Win 10 设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l">HoloLens</td>
    <td class="tg-yw4l">Windows.Holographic</td>
    <td class="tg-yw4l">VR/AR 耳机</td>
  </tr>
  <tr>
    <td class="tg-yw4l">IoT</td>
    <td class="tg-yw4l">Windows.IoT</td>
    <td class="tg-yw4l">IoT 设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="4">iOS</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-yw4l" rowspan="4">Apple.Ios.All</td>
    <td class="tg-yw4l">Apple.Ios.Phone</td>
    <td class="tg-yw4l">iPhone, Touch</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Tablet</td>
    <td class="tg-yw4l">Apple.Ios.Tablet</td>
    <td class="tg-yw4l">iPad mini，iPad，iPad Pro</td>
  </tr>
  <tr>
    <td class="tg-yw4l">TV</td>
    <td class="tg-yw4l">Apple.Ios.TV</td>
    <td class="tg-yw4l">Apple 电视</td>
  </tr>
  <tr>
    <td class="tg-yw4l">观看</td>
    <td class="tg-yw4l">Apple.Ios.Watch</td>
    <td class="tg-yw4l">iWatch</td>
  </tr>
  <tr>
    <td class="tg-yw4l">MacOS</td>
    <td class="tg-yw4l">桌面设备</td>
    <td class="tg-baqh" colspan="2">Apple.MacOS.All</td>
    <td class="tg-yw4l">MacBook Pro，MacBook 以无线方式，Mac Mini iMac</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="5">Android</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-yw4l" rowspan="5">Google.Android.All</td>
    <td class="tg-yw4l">Google.Android.Phone</td>
    <td class="tg-yw4l">面向任何风格的 Android 的移动设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Tablet</td>
    <td class="tg-yw4l">Google.Android.Tablet</td>
    <td class="tg-yw4l">Android 平板电脑</td>
  </tr>
  <tr>
    <td class="tg-yw4l">桌面设备</td>
    <td class="tg-yw4l">Google.Android.Desktop</td>
    <td class="tg-yw4l">Chromebook</td>
  </tr>
  <tr>
    <td class="tg-yw4l">TV</td>
    <td class="tg-yw4l">Google.Android.TV</td>
    <td class="tg-yw4l">Android 大屏幕设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l">观看</td>
    <td class="tg-yw4l">Google.Android.Watch</td>
    <td class="tg-yw4l">Google 齿轮设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="2">Windows</td>
    <td class="tg-yw4l">7</td>
    <td class="tg-baqh" colspan="2">Windows7.Desktop</td>
    <td class="tg-yw4l">Windows 7 设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l">8</td>
    <td class="tg-baqh" colspan="2">Windows8.Desktop</td>
    <td class="tg-yw4l">Windows 8/8.1 设备</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="5">Web</td>
    <td class="tg-yw4l">Microsoft</td>
    <td class="tg-yw4l" rowspan="5">Web.All</td>
    <td class="tg-yw4l">Web.Edge.All</td>
    <td class="tg-yw4l">边缘 web 引擎应用</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Android</td>
    <td class="tg-yw4l">Web.Blink.All</td>
    <td class="tg-yw4l">闪烁 web 引擎应用</td>
  </tr>
    <tr>
    <td class="tg-yw4l">镶边</td>
    <td class="tg-yw4l">Web.Chromium.All</td>
    <td class="tg-yw4l">引擎的 chrome web 应用</td>
  </tr>
  <tr>
    <td class="tg-yw4l">iOS</td>
    <td class="tg-yw4l">Web.Webkit.All</td>
    <td class="tg-yw4l">Webkit web 引擎应用</td>
  </tr>
  <tr>
    <td class="tg-yw4l">MacOS</td>
    <td class="tg-yw4l">Web.Safari.All</td>
    <td class="tg-yw4l">Safari web 引擎应用</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Linux</td>
    <td class="tg-yw4l">Any/All</td>
    <td class="tg-baqh" colspan="2">Linux.All</td>
    <td class="tg-yw4l">所有 Linux 发行版</td>
  </tr>
</table>

在应用包清单文件中，您将需要包含相应的目标设备系列，如果想要仅在特定平台和设备上提取的包内容。 如果您喜欢 bulid 的方式，它支持所有平台和设备类型的包，请选择**Platform.All**作为目标设备系列。 同样，如果你想要仅支持 web 应用中的包，选择**Web.All**。

## <a name="sample-manifest-file-appxmanifestxml"></a>示例清单文件 (AppxManifest.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
         IgnorableNamespaces="mp uap uap3">

  <Identity Name="BestAppExtension"
            Publisher="CN=awesomepublisher"
            Version="1.0.0.0" />

  <mp:PhoneIdentity PhoneProductId="56a6ecda-c215-4864-b097-447edd1f49fe" PhonePublisherId="00000000-0000-0000-0000-000000000000"/>

  <Properties>
    <DisplayName>Best App Extension</DisplayName>
    <PublisherDisplayName>Awesome Publisher</PublisherDisplayName>
    <Description>This is an extension package to my app</Description>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>

  <Resources>
    <Resource Language="x-generate"/>
  </Resources>

  <Dependencies>
    <TargetDeviceFamily Name="Platform.All" MinVersion="0.0.0.0" MaxVersionTested="0.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="App">
      <uap:VisualElements
          DisplayName="Best App Extension"
          Description="This is the best app extension"
          BackgroundColor="white"
          Square150x150Logo="images\squareTile-sdk.png"
          Square44x44Logo="images\smallTile-sdk.png"
          AppListEntry="none">
      </uap:VisualElements>

      <Extensions>
        <uap3:Extension Category="Windows.appExtension">
          <uap3:AppExtension Name="add-in-contract" Id="add-in" PublicFolder="Public" DisplayName="Sample Add-in" Description="This is a sample add-in">
            <uap3:Properties>
               <!--Free form space-->
            </uap3:Properties>
          </uap3:AppExtension>
        </uap3:Extension>
      </Extensions>

    </Application>
  </Applications>
</Package>
```

## <a name="platform-version"></a>平台版本
在上述示例清单文件中，以及平台名称，也有参数，以指定**MinVersion**并**MaxVersionTested**在 Windows 10 平台上使用这些参数。 在 Windows 10 中，包将仅部署 Windows 10 操作系统版本大于 MinVersion 上。 在其他非 Windows 10 平台，MinVersion 和 maxversiontested 赋予参数不用于使是否提取包内容的声明。

如果你想要使用为所有平台 （Windows 10 和 Windows 10） 的包，我们建议使用 MinVersion 和 maxversiontested 赋予参数来指定你想在应用程序处理 Windows 10 OS 版本。 因此在清单**依赖项**部分应如下所示：
```xml
  <Dependencies>
    <TargetDeviceFamily Name="Platform.All" MinVersion="0.0.0.0" MaxVersionTested="0.0.0.0"/>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.14393.0" MaxVersionTested="10.0.16294.0"/>
  </Dependencies>
```

**MinVersion**并**MaxVersionTested**是清单中的必填的字段，它们必须符合四 notation(#.#.#.#)。 如果您仅使用仅非 Windows 10 平台 MSIX 打包 SDK，可以只需使用"0.0.0.0"作为**MinVersion**并**MaxVersionTested**作为版本。

## <a name="how-to-effectively-use-the-same-package-on-all-platforms-windows-10-and-non-windows-10"></a>如何有效地在所有平台 （Windows 10 和 Windows 10） 上使用同一个包

若要充分利用 MSIX 打包 SDK，将需要用于生成包将部署等 Windows 10 上和在同一时间在其他平台上受支持的应用程序包的方式。 在 Windows 10 中，可以生成将包作为*应用扩展*。 有关应用扩展以及它们如何帮助使您的应用程序扩展的详细信息，请参阅[应用程序扩展插件简介](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)博客文章。

在本文前面部分所示的清单文件示例，您会注意到**属性**中的元素**AppExtension**元素。 不没有在清单文件的此部分中执行任何验证。 这允许开发人员指定之间扩展和主机/客户端应用程序所需元数据。
