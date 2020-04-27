---
Description: 本文深入探讨桌面桥幕后的工作原理。
title: 在桌面桥幕后
ms.date: 01/30/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: c646f8c393c43a6aa01fc0cf594b269cef984433
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "77072738"
---
# <a name="understanding-how-packaged-desktop-apps-run-on-windows"></a>了解打包的桌面应用如何在 Windows 上运行

本文详细探讨在为桌面应用程序创建 Windows 应用包时，文件和注册表项发生的情况。

新式包的主要目标是尽量将应用程序状态与系统状态相隔离，同时保留与其他应用的兼容性。 Windows 10 实现此目的的方式是，将应用程序放入 MSIX 包，然后检测并重定向它在运行时对文件系统和注册表所做的一些更改。

为桌面应用程序创建的包是仅限桌面的、完全信任的应用程序，并且不虚拟化或沙盒化的。 这使它们可以使用与经典桌面应用程序相同的方式与其他应用交互。

## <a name="installation"></a>安装

应用包安装在 *C:\Program Files\WindowsApps\package_name* 下，并且可执行文件的标题为 *app_name.exe*。 每个包文件夹都包含一个清单（名为 AppxManifest.xml），其中包含已打包应用的特殊 XML 命名空间。 该清单文件内部是一个 ```<EntryPoint>``` 元素，该元素引用完全信任的应用。 启动该应用程序时，它不会在应用容器内部运行，而是像往常一样以用户身份运行。

部署后，软件包文件由操作系统标记为只读并严格锁定。 如果这些文件遭到篡改，Windows 将阻止应用启动。

## <a name="file-system"></a>文件系统

对于打包的桌面应用程序，OS 支持不同级别的文件系统操作，具体取决于文件夹的位置。

### <a name="appdata-operations-on-windows-10-version-1903-and-later"></a>Windows 10 版本 1903 和更高版本上的 AppData 操作

AppData 用户文件夹中所有新建的文件和文件夹（例如 *C:\Users\user_name\AppData*）将写入特定于用户和应用的专用位置，但在运行时将会合并，以显示在实际的 AppData 位置。 对于仅由应用程序本身使用的项目，这可以实现某种程度的状态隔离，同时，在卸载应用程序后，使系统能够清理这些文件。 允许修改 AppData 用户文件夹中的现有文件，以在应用程序与 OS 之间提供更高程度的兼容性和交互性。 这可以减轻文件系统的“腐朽”程度，因为 OS 能够意识到应用程序所做的每项文件或目录更改。 状态隔离还使打包的桌面应用程序能够选取同一应用程序的非打包版本的移除位置。 请注意，OS 不支持将虚拟文件系统 (VFS) 文件夹用作用户的 AppData 文件夹。

### <a name="appdata-operations-on-windows-10-version-1809-and-earlier"></a>Windows 10 版本 1809 和更低版本上的 AppData 操作

对 AppData 文件夹（例如 *C:\Users\user_name\AppData*）的所有写入（包括创建、删除和更新）将在写入时复制到特定于用户和应用的专用位置。 这会产生一种假象：打包的应用程序正在编辑真正的 AppData，但它实际上修改的是专用副本。 通过以这种方式重定向写入，系统可以跟踪应用所作的所有文件修改。 这样，在卸载应用程序后，系统就可以清理这些文件，从而减轻系统的“腐朽”程度，并为用户提供更好的应用程序删除体验。

### <a name="other-folders"></a>其他文件夹

除了重定向 AppData 以外，Windows 的常用文件夹（System32、Program Files (x86) 等）还与应用包中的相应目录动态合并。 每个包都在其根目录中包含一个名为“VFS”的文件夹。 VFS 目录中目录或文件的任何读取都在运行时与其各自的本机对应项合并。 例如，应用程序可能包含 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll* 作为其应用包的一部分，但文件看起来像是安装在 *C:\Windows\System32\vc10.dll* 中。  这保留了与可能预期文件处于非软件包位置的桌面应用程序的兼容性。

不允许对应用包中的文件/文件夹执行写入。 OS 将忽略对不在包中的文件和文件夹的写入，只要用户拥有权限，便允许此类操作。

### <a name="common-operations"></a>常见操作

以下简短参考表格显示了常见文件系统操作以及 OS 如何处理它们。

| 操作 | 结果 | 示例 |
| --- | --- | --- |
| 读取或枚举已知的 Windows 文件或文件夹 | *C:\Program Files\package_name\VFS\well_known_folder* 与本地系统对应项的动态合并。 | 读取 *C:\Windows\System32* 会返回 *C:\Windows\System32* 的内容以及 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86* 的内容。 |
| 在 AppData 下写入 | **Windows 10 版本 1903 和更高版本：** 在以下目录中创建的新文件和文件夹将重定向到特定于用户和包的专用位置：<ul><li>本地</li><li>Local\Microsoft</li><li>漫游</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Menu\Programs</li></ul>在响应文件打开命令时，OS 将先从特定于用户和包的位置打开文件。 如果不存在此位置，则 OS 将尝试从真实的 AppData 位置打开文件。 如果已从真实的 AppData 位置打开该文件，则不会对该文件进行虚拟化。 如果用户拥有权限，则允许删除 AppData 中的文件。<p/><p/>**Windows 10 版本 1809 和更低版本：** 在写入时复制到每用户、每应用位置。 | AppData 通常为 *C:\Users\user_name\AppData*。 |
| 在软件包内写入 | 不允许。 软件包为只读。 | 不允许在 *C:\Program Files\WindowsApps\package_name* 下写入。 |
| 在软件包外写入 | 如果用户具有权限，则允许。 | 如果软件包不包含 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll*，并且用户具有权限，则允许对 *C:\Windows\System32\foo.dll* 的写入。 |

### <a name="packaged-vfs-locations"></a>打包的 VFS 位置

下表显示了为应用在系统上的哪个位置覆盖作为程序包一部分交付的文件。 应用程序将认为这些文件位于所列的系统位置，但这些文件实际上位于 *C:\Program Files\WindowsApps\package_name\VFS* 中的重定向位置。 根据 [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx) 常量确定 FOLDERID 位置。

系统位置 | 重定向位置 (在 [PackageRoot]\VFS\ 下\) | 支持的体系结构
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | Common AppData | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>注册表

应用包中有一个 registry.dat 文件，该文件充当真实注册表中的 *HKLM\Software* 的逻辑等效项。 在运行时，此虚拟注册表将此配置单元的内容合并到原生系统配置单元，以提供两者的单一视图。 例如，如果 registry.dat 包含单个项“Foo”，则运行时 *HKLM\Software* 的读取也将显示为包含“Foo”（除了所有原生系统项）。

仅 *HKLM\Software* 下的项是软件包的一部分；*HKCU* 或注册表其他部分下的项不是。 不允许写入软件包中的项或值。 只要用户拥有权限，便允许对不在包中的项或值执行写入。

HKCU 下的所有写入都在写入时复制到每用户、每应用位置。 在传统上，卸载程序无法清除 *HKEY_CURRENT_USER*，因为注销用户的注册表数据已卸载并且不可用。

所有写入将在包升级期间保留，并且仅在完全删除应用程序时才删除这些写入内容。

### <a name="common-operations"></a>常见操作

以下简短参考表格显示了常见注册表操作以及 OS 如何处理它们。

操作 | 结果 | 示例
:--- | :--- | :---
读取或枚举 *HKLM\Software* | 软件包配置单元与本地系统对应项的动态合并。 | 如果 registry.dat 包含单个项“Foo”，则在运行时，*HKLM\Software* 的读取将显示 *HKLM\Software* 和 *HKLM\Software\Foo* 的内容。
在 HKCU 下写入 | 在写入时复制到每用户、每应用专用位置。 | 与文件的 AppData 相同。
在软件包内写入。 | 不允许。 软件包为只读。 | 如果软件包配置单元中存在对应的项/值，则不允许在 *HKLM\Software* 下写入。
在软件包外写入 | 由 OS 忽略。 如果用户具有权限，则允许。 | 只要软件包配置单元中不存在对应的项/值并且用户具有正确的访问权限，则允许在 *HKLM\Software* 下写入。

## <a name="uninstallation"></a>卸载

当用户卸载某个包时，将删除位于 *C:\Program Files\WindowsApps\package_name* 中的所有文件和文件夹，并删除打包期间捕获的对 AppData 或注册表的任何重定向写入。