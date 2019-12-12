---
Description: 本文深入研究桌面桥深层次的工作原理。
title: 在桌面桥幕后
ms.date: 04/25/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: c3f2d5adb3bdd790b3fef7731faebd90089f8a59
ms.sourcegitcommit: d749fa662214bddaa6854f1ee95761c547db8dae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2019
ms.locfileid: "75008097"
---
# <a name="behind-the-scenes-of-your-packaged-desktop-application"></a>打包桌面应用程序的幕后

本文详细介绍了在为桌面应用程序创建 Windows 应用包时，对文件和注册表项进行的操作。

新式包的主要目标是尽可能将应用程序状态与系统状态分开，同时保持与其他应用的兼容性。 Windows 10 通过将应用程序放在 .MSIX 包中来实现此功能，然后在运行时检测并重定向在文件系统和注册表中所做的一些更改。

你为桌面应用程序创建的包是仅限桌面的完全信任应用程序，并且不是虚拟化或沙盒。 这使它们可以使用与经典桌面应用程序相同的方式与其他应用交互。

## <a name="installation"></a>安装

应用包安装在 *C:\Program Files\WindowsApps\package_name* 下，并且可执行文件的标题为 *app_name.exe*。 每个软件包文件夹都包含一个清单（名为 AppxManifest.xml），其中包含已打包应用的特殊 XML 命名空间。 该清单文件内部是一个 ```<EntryPoint>``` 元素，该元素引用完全信任的应用。 当该应用程序启动时，它不会在应用程序容器内运行，而是以用户通常的方式运行。

部署后，软件包文件由操作系统标记为只读并严格锁定。 如果这些文件遭到篡改，Windows 将阻止应用启动。

## <a name="file-system"></a>文件系统

对于打包的桌面应用程序，操作系统支持不同级别的文件系统操作，具体取决于文件夹位置。

### <a name="appdata-operations-on-windows-10-version-1903-and-later"></a>Windows 10 版本1903及更高版本上的 AppData 操作

用户的 AppData 文件夹中的所有新创建的文件和文件夹（例如， *c:\users\ user_name \appdata*）都将写入专用的每个用户、每个应用的位置，但在运行时将显示在实际 AppData 位置。 这允许对仅由应用程序本身使用的项目进行某种程度的状态分离，这使系统能够在卸载应用程序时清理这些文件。 允许对用户的 AppData 文件夹下的现有文件进行修改，以防止应用程序和操作系统之间出现更高的兼容性和交互性。 这可以减少文件系统 "rot"，因为操作系统知道应用程序所做的每个文件或目录更改。 状态分隔还允许打包的桌面应用程序选择不包含相同应用程序的非打包版本。 请注意，操作系统不支持用户的 AppData 文件夹的虚拟文件系统（VFS）文件夹。

### <a name="appdata-operations-on-windows-10-version-1809-and-earlier"></a>Windows 10 版本1809及更低版本上的 AppData 操作

对用户的 AppData 文件夹（例如， *c:\users\ user_name \appdata*）的所有写入（包括 create、delete 和 update）都将在写入专用的每个用户、每个应用位置时复制。 这将创建打包后的应用程序在实际修改专用副本时编辑真正 AppData 的假象。 通过以这种方式重定向写入，系统可以跟踪应用所作的所有文件修改。 这允许系统在卸载应用程序时清理这些文件，从而减少系统 "rot" 并为用户提供更好的应用程序删除体验。

### <a name="other-folders"></a>其他文件夹

除了重定向 AppData 外，Windows 的已知文件夹（System32、Program Files （x86）等）还动态地与应用程序包中的相应目录合并在一起。 每个软件包都在其根目录中包含一个名为“VFS”的文件夹。 VFS 目录中目录或文件的任何读取都在运行时与其各自的本机对应项合并。 例如，应用程序可能包含*C:\Program files\windowsapps\ package_name \vfs\systemx86\vc10.dll*作为其应用包的一部分，但该文件似乎安装在*C:\Windows\System32\vc10.dll*中。  这保留了与可能预期文件处于非软件包位置的桌面应用程序的兼容性。

不允许写入应用包中的文件/文件夹。 不属于包的文件和文件夹的写入会被 OS 忽略，只要用户具有相应的权限，就可以使用这些文件和文件夹。

### <a name="common-operations"></a>常见操作

此简短引用表显示了常见的文件系统操作以及操作系统如何处理它们。

| 操作 | 结果 | 示例 |
| --- | --- | --- |
| 读取或枚举已知的 Windows 文件或文件夹 | *C:\Program Files\package_name\VFS\well_known_folder* 与本地系统对应项的动态合并。 | 读取 *C:\Windows\System32* 会返回 *C:\Windows\System32* 的内容以及 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86* 的内容。 |
| 在 AppData 下写入 | **Windows 10 版本1903及更高版本：** 在以下目录下创建的新文件和文件夹将重定向到每个用户、每个包的专用位置：<ul><li>本地</li><li>Local\Microsoft</li><li>Roaming</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Menu\Programs</li></ul>在响应文件打开命令时，OS 将首先从每个用户的每个包的位置打开该文件。 如果该位置不存在，则操作系统将尝试从真实的 AppData 位置打开文件。 如果该文件是从实际 AppData 位置打开的，则不会对该文件进行虚拟化。 如果用户具有权限，则允许在 AppData 下删除文件。<p/><p/>**Windows 10 版本1809及更早版本：** 复制到每个用户、每个应用的位置。 | AppData 通常为 *C:\Users\user_name\AppData*。 |
| 在软件包内写入 | 不允许。 软件包为只读。 | 不允许在 *C:\Program Files\WindowsApps\package_name* 下写入。 |
| 在软件包外写入 | 如果用户具有权限，则允许。 | 如果软件包不包含 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll*，并且用户具有权限，则允许对 *C:\Windows\System32\foo.dll* 的写入。 |

### <a name="packaged-vfs-locations"></a>打包的 VFS 位置

下表显示了为应用在系统上的哪个位置覆盖作为程序包一部分交付的文件。 应用程序会将这些文件感知到列出的系统位置，实际上它们位于*C:\Program files\windowsapps\ package_name \vfs*中的重定向位置。 根据 [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx) 常量确定 FOLDERID 位置。

系统位置 | 重定向的位置（在 [PackageRoot] \VFS\) | 支持的体系结构
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | WIN ENT LTSB 2016 English 64 Bits | x86, amd64
FOLDERID_ProgramData | Common AppData | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>注册表

应用包含有一个 registry.dat 文件，该文件充当真实注册表中的 *HKLM\Software* 的逻辑等效项。 在运行时，此虚拟注册表将此配置单元的内容合并到原生系统配置单元，以提供两者的单一视图。 例如，如果 registry.dat 包含单个项“Foo”，则运行时 *HKLM\Software* 的读取也将显示为包含“Foo”（除了所有原生系统项）。

仅 *HKLM\Software* 下的项是软件包的一部分；*HKCU* 或注册表其他部分下的项不是。 不允许写入软件包中的项或值。 只要用户具有权限，就允许对不属于包的键或值进行写入。

HKCU 下的所有写入都在写入时复制到每用户、每应用位置。 在传统上，卸载程序无法清除 *HKEY_CURRENT_USER*，因为注销用户的注册表数据已卸载并且不可用。

所有写入都将在包升级期间保留，并且仅在完全删除应用程序时删除。

### <a name="common-operations"></a>常见操作

此简短引用表显示了常见的注册表操作以及操作系统如何处理它们。

操作 | 结果 | 示例
:--- | :--- | :---
读取或枚举 *HKLM\Software* | 软件包配置单元与本地系统对应项的动态合并。 | 如果 registry.dat 包含单个项“Foo”，则在运行时，*HKLM\Software* 的读取将显示 *HKLM\Software* 和 *HKLM\Software\Foo* 的内容。
在 HKCU 下写入 | 在写入时复制到每用户、每应用专用位置。 | 与文件的 AppData 相同。
在软件包内写入。 | 不允许。 软件包为只读。 | 如果软件包配置单元中存在对应的项/值，则不允许在 *HKLM\Software* 下写入。
在软件包外写入 | 被 OS 忽略。 如果用户具有权限，则允许。 | 只要软件包配置单元中不存在对应的项/值并且用户具有正确的访问权限，则允许在 *HKLM\Software* 下写入。

## <a name="uninstallation"></a>卸载

当用户卸载包时，将删除*C:\Program files\windowsapps\ package_name*下的所有文件和文件夹，以及对 AppData 或在打包过程中捕获的注册表的任何重定向写入操作。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
