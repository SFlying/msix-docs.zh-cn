---
title: 检测包标识和运行时上下文
description: 描述应用如何在 Win 1709 或更高版本上确定自身是否以 MSIX 包的形式提供。
author: Huios
ms.date: 01/23/2020
ms.topic: article
keywords: windows 10, uwp, 应用包, 应用更新, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 0fe0a721368b10f68ba30665883e9070692dbb04
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "76726587"
---
# <a name="detect-package-identity-and-runtime-context"></a>检测包标识和运行时上下文

你的应用程序可能有某些版本不是在 MSIX 包中分发的。 在运行时，应用可以通过使用 Windows 包管理器 API 或你自己的自定义安装程序来检测自己是不是作为 MSIX 包部署。 可以更改更新设置等应用行为，也可以利用仅适用于 MSIX 包的功能。

若要确定应用程序是在支持完整 MSIX 功能集的 Windows 版本上作为 MSIX 包运行，可在 kernel32.dll 中使用 [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) 本机函数。 当桌面应用程序作为没有包标识的非包装应用程序运行时，此函数会返回一个错误，它能帮助你推断应用运行的上下文。

如果函数成功，这意味着：

* 你的应用包装在 MSIX 包中。
* 你的应用可以在 Windows 10 版本 1709（内部版本 16299）或更高版本的应用上运行，可获得完整的 MSIX 支持。

## <a name="use-getcurrentpackagefullname-in-native-code"></a>在本机代码中使用 GetCurrentPackageFullName

以下代码示例演示了如何使用 [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx) 来确定应用的上下文。

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

若要在托管 .NET Framework 应用中调用 [GetCurrentPackageFullName](https://msdn.microsoft.com/library/windows/desktop/hh446599(v=vs.85).aspx)需要使用 [Platform Invoke (P/Invoke)](https://docs.microsoft.com/dotnet/standard/native-interop/pinvoke) 或其他形式的互操作。

若要简化此过程，可以使用 [DesktopBridgeHelpers](https://github.com/qmatteoq/DesktopBridgeHelpers/) 库。 此库支持 .NET Framework 4 及更高版本，并在内部使用 P/Invoke 来提供帮助器类，以确定应用是否在支持完整 MSIX 功能集的 Windows 版本上运行。 此库还可以 [NuGet 包](https://www.nuget.org/packages/DesktopBridge.Helpers/)的形式提供。

在项目中安装包后，可以创建 `DesktopBridge.Helpers` 类的新实例，并调用 `IsRunningAsUwp` 方法。 如果你的应用作为 MSIX 包在 Windows 10 版本 1709（内部版本 16299）或更高版本上运行，则此方法返回 true；如果不满足以上任意条件，则返回 false。 下面的示例演示如何调用此方法。

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
