---
description: 可以通过包支持框架运行脚本，为用户环境自定义桌面应用程序。
title: 使用包支持框架运行脚本
ms.date: 09/20/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f684eae955f9a1f03d2008aee5ed6e3f594c296c
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090655"
---
# <a name="run-scripts-with-the-package-support-framework"></a>使用包支持框架运行脚本


使用脚本，IT 专业人员可以在使用 .MSIX 打包后，向用户的环境动态自定义应用程序。 例如，你可以使用脚本来配置数据库、设置 VPN、装载共享驱动器或动态执行许可证检查。 脚本提供了很大的灵活性。 它们可能会更改注册表项或基于计算机或服务器配置执行文件修改。

你可以使用包支持框架 (PSF) 在打包的应用程序可执行文件运行之前运行一个 PowerShell 脚本，并在运行应用程序可执行文件后运行一个 PowerShell 脚本以进行清理。 在应用程序清单中定义的每个应用程序可执行文件都可以有自己的脚本。 您可以将脚本配置为仅在第一次应用程序启动时运行一次，而不显示 PowerShell 窗口，使用户不会错误地提前结束该脚本。 还有其他选项可用于配置脚本的运行方式，如下所示。

## <a name="prerequisites"></a>先决条件

若要使脚本能够运行，需要将 PowerShell 执行策略设置为 `RemoteSigned` 。 可以通过运行以下命令来执行此操作：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

需要为64位 PowerShell 可执行文件和32位 PowerShell 可执行文件设置执行策略。 请确保打开每个版本的 PowerShell，并运行上面所示的其中一个命令。

下面是每个可执行文件的位置。

* 64 位计算机：
  * 64位可执行文件：% SystemRoot% \system32\WindowsPowerShell\v1.0\powershell.exe
  * 32位可执行文件：% SystemRoot% \SysWOW64\WindowsPowerShell\v1.0\powershell.exe
* 32 位计算机：
  * 32位可执行文件：% SystemRoot% \system32\WindowsPowerShell\v1.0\powershell.exe

有关 PowerShell 执行策略的详细信息，请参阅 [此文](/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6)。

确保还在包中包含 StartingScriptWrapper.ps1 文件，并将其放在可执行文件所在的同一文件夹中。 可以从 [PSF NuGet 包](https://www.nuget.org/packages/Microsoft.PackageSupportFramework/)复制此文件。

## <a name="enable-scripts"></a>启用脚本

若要指定将为每个打包的应用程序可执行文件运行的脚本，需要修改 [ 文件上的config.js](package-support-framework.md#create-a-configuration-file)。 若要告诉 PSF 在执行打包应用程序之前运行脚本，请添加名为的配置项目 `startScript` 。 若要告诉 PSF 在打包的应用程序完成后运行脚本，请添加名为的配置项目 `endScript` 。

### <a name="script-configuration-items"></a>编写配置项目脚本

以下是可用于脚本的配置项目。 结束脚本将忽略 `waitForScriptToFinish` 和 `stopOnScriptError` 配置项目。

| 项名                | 值类型 | 必需？ | 默认  | 说明
|-------------------------|------------|-----------|----------|---------|
| `scriptPath`              | string     | 是       | 空值      | 脚本的路径，包括名称和扩展名。 路径相对于应用程序的工作目录（如果已指定），否则在包的根目录处启动。
| `scriptArguments`         | string     | 否        | empty    | 空格分隔参数列表。 对于 PowerShell 脚本调用，格式是相同的。 此字符串将追加到 `scriptPath` ，以便进行有效的 PowerShell.exe 调用。
| `runInVirtualEnvironment` | boolean    | 否        | 是     | 指定是否应在打包应用程序运行所在的虚拟环境中运行脚本。
| `runOnce`                 | boolean    | 否        | 是     | 指定脚本是否应每个用户每个版本运行一次。
| `showWindow`              | boolean    | 否        | false    | 指定是否显示 PowerShell 窗口。
| `stopOnScriptError`       | boolean    | 否        | false    | 指定在启动脚本失败时是否退出应用程序。
| `waitForScriptToFinish`   | boolean    | 否        | 是     | 指定打包的应用程序在启动前是否应等待开始脚本完成。
| `timeout`                 | DWORD      | 否        | INFINITE | 允许脚本执行的时间长度。 当时间结束时，脚本将停止。

> [!NOTE]
> `stopOnScriptError: true` `waitForScriptToFinish: false` 不支持为示例应用程序设置和。 如果你设置这两个配置项目，则 PSF 将返回 ERROR_BAD_CONFIGURATION 的错误。


## <a name="sample-configuration"></a>示例配置

下面是使用两个不同的应用程序可执行文件的示例配置。

```json
{
  "applications": [
    {
      "id": "Sample",
      "executable": "Sample.exe",
      "workingDirectory": "",
      "stopOnScriptError": false,
      "startScript":
      {
        "scriptPath": "RunMePlease.ps1",
        "scriptArguments": "\\\"First argument\\\" secondArgument",
        "runInVirtualEnvironment": true,
        "showWindow": true,
        "waitForScriptToFinish": false
      },
      "endScript":
      {
        "scriptPath": "RunMeAfter.ps1",
        "scriptArguments": "ThisIsMe.txt"
      }
    },
    {
      "id": "CPPSample",
      "executable": "CPPSample.exe",
      "workingDirectory": "",
      "startScript":
      {
        "scriptPath": "CPPStart.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runInVirtualEnvironment": true
      },
      "endScript":
      {
        "scriptPath": "CPPEnd.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runOnce": false
      }
    }
  ],
  "processes": [
    ...(taken out for brevity)
  ]
}
```
