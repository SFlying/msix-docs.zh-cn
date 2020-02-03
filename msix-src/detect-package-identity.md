---
title: 检测包标识和运行时上下文
description: 描述应用如何确定其在 Win 1709 或更高版本上是否作为 .MSIX 包发货。
author: Huios
ms.date: 01/23/2020
ms.topic: article
keywords: windows 10, uwp, 应用包, 应用更新, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 0fe0a721368b10f68ba30665883e9070692dbb04
ms.sourcegitcommit: 97166b4a273cea789aedcfbb3cba4ce074958ed8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726587"
---
# <a name="detect-package-identity-and-runtime-context"></a>检测包标识和运行时上下文

你的应用程序的某些版本未在 .MSIX 包中分发。 在运行时，应用程序可以通过使用 Windows 程序包管理器 API 或你自己的自定义安装程序来检测它是否已部署为 .MSIX 包。 你可能需要更改应用程序行为（如更新设置），或者你可能想要利用仅适用于 .MSIX 包的功能。

若要确定应用程序是否在支持完全 .MSIX 功能集的 Windows 版本上作为 .MSIX 包运行，可在 kernel32.dll 中使用[GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx)本机函数。 当桌面应用程序作为未打包的应用程序运行时，如果没有包标识，此函数将返回一个错误，该错误可帮助您推断应用程序的运行上下文。

如果该函数成功，则表示：

* 应用打包在 .MSIX 包中。
* 你的应用正在 Windows 10 （版本1709（版本16299）或更高版本上运行，并提供完整的 .MSIX 支持。

## <a name="use-getcurrentpackagefullname-in-native-code"></a>在本机代码中使用 GetCurrentPackageFullName

下面的代码示例演示如何使用[GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx)来确定应用的上下文。

```cpp
#define _UNICODE 1
#define UNICODE 1

#include <Windows.h>
#include <appmodel.h>
#include <malloc.h>
#include <stdio.h>

int __cdecl wmain()
{
    UINT32 length = 0;
    LONG rc = GetCurrentPackageFullName(&length, NULL);
    if (rc != ERROR_INSUFFICIENT_BUFFER)
    {
        if (rc == APPMODEL_ERROR_NO_PACKAGE)
            wprintf(L"Process has no package identity\n");
        else
            wprintf(L"Error %d in GetCurrentPackageFullName\n", rc);
        return 1;
    }

    PWSTR fullName = (PWSTR) malloc(length * sizeof(*fullName));
    if (fullName == NULL)
    {
        wprintf(L"Error allocating memory\n");
        return 2;
    }

    rc = GetCurrentPackageFullName(&length, fullName);
    if (rc != ERROR_SUCCESS)
    {
        wprintf(L"Error %d retrieving PackageFullName\n", rc);
        return 3;
    }
    wprintf(L"%s\n", fullName);

    free(fullName);

    return 0;
}
```

## <a name="use-getcurrentpackagefullname-function-in-managed-code"></a>在托管代码中使用 GetCurrentPackageFullName 函数

若要在托管 .NET Framework 应用中调用[GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) ，需要使用[平台调用（P/Invoke）](https://docs.microsoft.com/dotnet/standard/native-interop/pinvoke)或某种其他形式的互操作。

若要简化此过程，可以使用[DesktopBridgeHelpers](https://github.com/qmatteoq/DesktopBridgeHelpers/)库。 此库支持 .NET Framework 4 及更高版本，并在内部使用 P/Invoke 来提供帮助器类，以确定应用程序是否在支持完整 .MSIX 功能集的 Windows 版本上运行。 此库还可作为[NuGet 包](https://www.nuget.org/packages/DesktopBridge.Helpers/)提供。

在项目中安装包后，可以创建 `DesktopBridge.Helpers` 类的新实例，并调用 `IsRunningAsUwp` 方法。 如果你的应用正在 Windows 10 上的 .MSIX 包（版本1709（版本16299）或更高版本中运行，则此方法返回 true; 如果不是，则返回 false。 下面的示例演示如何调用此方法。

```csharp
private bool IsRunningAsUwp()
{
   UwpHelpers helpers = new UwpHelpers();
   return helpers.IsRunningAsUwp();
}

private void Form1_Load(object sender, EventArgs e)
{
   if (IsRunningAsUwp())
   {
       txtUwp.Text = "I'm running as MSIX";
   }
   else
   {
       txtUwp.Text = "I'm running as a native desktop app";
   }
}
```
