---
Description: 可以通过包支持框架运行脚本，为用户环境自定义桌面应用程序。
title: 用包支持框架运行脚本
ms.date: 09/20/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 886255fc6e376dc2bf8f166857f6fe5d4ee9ffd9
ms.sourcegitcommit: cc7fe74ea7c7b8c06190330023b3dff43034960e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310995"
---
# <a name="run-scripts-with-the-package-support-framework"></a>用包支持框架运行脚本

如果你在为应用程序创建 .MSIX 包后发现应用程序的兼容性问题，则可以通过运行脚本来动态自定义用户环境的应用程序，从而解决其中的一些问题。 例如，这些脚本可能会更改注册表项或基于计算机或服务器配置执行文件修改。

在应用程序可执行文件运行之前，可以使用包支持框架（PSF）运行一个 PowerShell 脚本，并运行一个 PowerShell 脚本。 在应用程序清单中定义的每个应用程序可执行文件都可以有自己的脚本。

## <a name="prerequisites"></a>先决条件

若要使脚本能够运行，需要将 PowerShell 执行策略设置为`Unrestricted`或。 `RemoteSigned` 可以通过运行以下命令之一来执行此操作：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

需要为64位 PowerShell 可执行文件和32位 PowerShell 可执行文件设置执行策略。 请确保打开每个版本的 PowerShell，并运行上面所示的其中一个命令。

下面是每个可执行文件的位置。

* 64 位计算机：
  * 64位可执行文件：%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
  * 32位可执行文件：%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
* 32位计算机：
  * 32位可执行文件：%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe

有关 PowerShell 执行策略的详细信息，请参阅[此文](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6)。

## <a name="enable-scripts"></a>启用脚本

若要指定将为每个打包的应用程序可执行文件运行的脚本，需要修改[配置文件](package-support-framework.md#create-a-configuration-file)。 若要告诉 PSF 在执行打包应用程序之前运行脚本，请添加名`startScript`为的配置项目。 若要告诉 PSF 在打包的应用程序完成后运行脚本，请添加名`endScript`为的配置项目。

### <a name="script-configuration-items"></a>编写配置项目脚本

以下是可用于脚本的配置项目。 结束脚本将忽略`waitForScriptToFinish`和`stopOnScriptError`配置项目。

| 项名称                | 值类型 | 是否为必需？ | 默认  | 描述
|-------------------------|------------|-----------|----------|---------|
| `scriptPath`              | string     | 是       | 不可用      | 脚本的路径，包括名称和扩展名。 路径从应用程序的根目录开始。
| `scriptArguments`         | string     | 否        | 空    | 空格分隔参数列表。 对于 PowerShell 脚本调用，格式是相同的。 此字符串将追加到`scriptPath` ，以便进行有效的 PowerShell 调用。
| `runInVirtualEnvironment` | boolean    | 否        | 真     | 指定是否应在打包应用程序运行所在的虚拟环境中运行脚本。
| `runOnce`                 | boolean    | 否        | 真     | 指定脚本是否应每个用户每个版本运行一次。
| `showWindow`              | boolean    | 否        | 假    | 指定是否显示 PowerShell 窗口。
| `stopOnScriptError`       | boolean    | 否        | 假    | 指定在启动脚本失败时是否退出应用程序。
| `waitForScriptToFinish`   | boolean    | 否        | 真     | 指定打包的应用程序在启动前是否应等待开始脚本完成。
| `timeout`                 | DWORD      | 否        | 无数 | 允许脚本执行的时间长度。 当时间结束时，脚本将停止。

> [!NOTE]
> 不`stopOnScriptError: true`支持`waitForScriptToFinish: false`为示例应用程序设置和。 如果你设置这两个配置项目，则 PSF 将返回错误 ERROR_BAD_CONFIGURATION。


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
