---
Description: 解决阻止桌面应用程序在 .MSIX 容器中运行的问题
title: 解决阻止桌面应用程序在 .MSIX 容器中运行的问题
ms.date: 05/13/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0af2ab1de7c43ed6060048d68e148afefbc1e10f
ms.sourcegitcommit: 8d6bc53d5f5ae80d9ce191fe81660407e9f11e0e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83430429"
---
# <a name="create-a-package-support-framework-fixup"></a>创建包支持框架修复 

如果没有针对你的问题进行运行时修复，则可以通过编写替换函数并包含任何有用的配置数据来创建新的运行时修复。 让我们看一下每个部分。

### <a name="replacement-functions"></a>替换函数

首先，在应用程序在 .MSIX 容器中运行时确定哪些函数调用失败。 然后可以创建替代的函数，让运行时管理器改为调用这些函数。 这样，便可以使用符合新式运行时环境规则的行为来替代函数的实现。

声明 ``FIXUP_DEFINE_EXPORTS`` 宏，然后在每个的顶部添加包含语句 `fixup_framework.h` 。要在其中添加运行时修补程序的函数的 CPP 文件。

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>确保 `FIXUP_DEFINE_EXPORTS` 宏出现在 include 语句之前。

创建一个函数，该函数具有要修改其行为的函数的签名。 下面是用来替换函数的示例函数 `MessageBoxW` 。

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

调用将 `DECLARE_FIXUP` `MessageBoxW` 函数映射到新的替换函数。 当应用程序尝试调用函数时 `MessageBoxW` ，它将改为调用替换函数。

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>避免对运行时修复中函数的递归调用

`reentrancy_guard`可以向函数添加类型，以防止递归函数调用。

例如，你可能会生成函数的替换函数 `CreateFile` 。 您的实现可能调用 `CopyFile` 函数，但函数的实现 `CopyFile` 可能调用 `CreateFile` 函数。 这可能会导致无限递归调用 `CreateFile` 函数。

有关详细信息， `reentrancy_guard` 请参阅[authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>配置数据

如果要将配置数据添加到运行时修补程序，请考虑将其添加到中 ``config.json`` 。 这样，你就可以使用 `FixupQueryCurrentDllConfig` 轻松分析该数据。 此示例分析该配置文件中的布尔值和字符串值。

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

### <a name="fixup-metadata"></a>修正元数据

每个修正和 PSF 启动器应用程序都有一个 XML 元数据文件，其中包含以下信息：

* 版本： PSF 的版本。微不足道.根据[Sem 版本 2](https://semver.org/)修补格式。
* 最低 Windows 平台：修正或 PSF 启动器所需的最小 windows 版本。
* 说明：修正的简短说明。
* WhenToUse：试探法上的试探法。

有关示例，请参阅[FileRedirectionFixupMetadata](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/fixups/FileRedirectionFixup/FileRedirectionFixupMetadata.xml)元数据文件中的重定向链接。 [此处](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/MetadataSchema.xsd)提供了元数据架构。