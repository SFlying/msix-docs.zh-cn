---
Description: 本文深入研究桌面桥深层次的工作原理。
title: 在桌面桥幕后
ms.date: 04/25/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: 8e7a435ca3394152cedaafa01033e5f510f6d8f3
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828793"
---
# <a name="behind-the-scenes-of-your-packaged-desktop-application"></a>打包的桌面应用程序在后台

本文提供有关会发生什么情况的文件和注册表项时为桌面应用程序创建 Windows 应用包的更深入的了解。

现代的主要目标是包的单独的系统状态尽可能保持与其他应用程序兼容性的同时从应用程序状态。 Windows 10 通过放置在通用 Windows 平台 (UWP) 的程序包，应用程序然后检测和重定向到文件系统和注册表中的运行时做一些更改完成此操作。

为桌面应用程序创建的包是-仅适用于桌面、 完全信任的应用程序并不是虚拟化或沙盒。 这使它们可以使用与经典桌面应用程序相同的方式与其他应用交互。

## <a name="installation"></a>安装

应用包安装在 *C:\Program Files\WindowsApps\package_name* 下，并且可执行文件的标题为 *app_name.exe*。 每个软件包文件夹都包含一个清单（名为 AppxManifest.xml），其中包含已打包应用的特殊 XML 命名空间。 该清单文件内部是一个 ```<EntryPoint>``` 元素，该元素引用完全信任的应用。 启动该应用程序时，不会运行在应用程序容器，但改为运行以用户身份正常运行。

部署后，软件包文件由操作系统标记为只读并严格锁定。 如果这些文件遭到篡改，Windows 将阻止应用启动。

## <a name="file-system"></a>文件系统

操作系统支持不同级别的打包桌面应用程序，具体取决于文件夹位置的文件系统操作。

### <a name="appdata-operations-on-windows-10-version-1903-and-later"></a>Windows 10，版本 1903年及更高版本上的应用程序数据操作

用户的 AppData 文件夹中的所有新创建的文件和文件夹 (例如， *C:\Users\user_name\AppData*) 写入到专用每个用户，在运行时才会出现在真实的 AppData 位置但合并每个应用的位置。 这允许一定程度进行状态的隔离仅由应用程序本身，使用的项目，这使得系统卸载该应用程序时清理这些文件。 对用户的 AppData 文件夹下的现有文件的修改就可以防止更高程度的兼容性和应用程序和操作系统之间的交互功能。 这会减少文件系统"rot"，因为操作系统识别的应用程序发出的每个文件或目录更改。 状态分离还允许打包桌面应用程序选取同一个应用程序的非打包版本离开的位置。 请注意 OS 不支持的用户的 AppData 文件夹虚拟文件系统 (VFS) 文件夹。

### <a name="appdata-operations-on-windows-10-version-1809-and-earlier"></a>对 Windows 10，版本 1809年及更早版本的应用程序数据操作

所有写入到用户的 AppData 文件夹 (例如， *C:\Users\user_name\AppData*)，其中包括创建、 删除和更新、 复制写入到专用每个用户，每个应用的位置。 这将创建时它实际上在修改的私有副本，编辑已包装应用程序实际应用程序数据的视觉效果。 通过以这种方式重定向写入，系统可以跟踪应用所作的所有文件修改。 这允许系统卸载该应用程序时清理这些文件，因此减少系统"rot"并提供更好的应用程序删除体验用户。

### <a name="other-folders"></a>其他文件夹

除了重定向应用程序数据，与在应用程序包中的相应目录动态合并 Windows' 的已知文件夹 （System32、 Program Files (x86) 等）。 每个软件包都在其根目录中包含一个名为“VFS”的文件夹。 VFS 目录中目录或文件的任何读取都在运行时与其各自的本机对应项合并。 例如，应用程序可能包含*C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll* ，像显示的一部分的应用程序包，但该文件以安装在*C:\Windows\System32\vc10.dll*。  这保留了与可能预期文件处于非软件包位置的桌面应用程序的兼容性。

不允许写入应用包中的文件/文件夹。 写入文件和文件夹不是包的一部分，只要用户具有的权限允许和由操作系统将被忽略。

### <a name="common-operations"></a>常见操作

此短引用表显示了常见的文件系统操作，以及操作系统如何处理它们。

| 操作 | 结果 | 示例 |
| --- | --- | --- |
| 读取或枚举已知的 Windows 文件或文件夹 | *C:\Program Files\package_name\VFS\well_known_folder* 与本地系统对应项的动态合并。 | 读取 *C:\Windows\System32* 会返回 *C:\Windows\System32* 的内容以及 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86* 的内容。 |
| 在 AppData 下写入 | **Windows 10，版本 1903 及更高版本：** 在以下目录下创建新文件和文件夹重定向到每个用户，每个包的专用位置：<ul><li>本地</li><li>Local\Microsoft</li><li>Roaming</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Menu\Programs</li></ul>在响应文件的打开命令，OS 将从每个用户打开文件，单个包位置第一次。 如果此位置不存在，操作系统将尝试从实际的 AppData 位置打开该文件。 如果从实际的 AppData 位置打开该文件，则会发生该文件没有虚拟化。 如果用户具有的权限，允许在 AppData 下的文件删除。<p/><p/>**Windows 10，版本 1809 及更早版本：** 在写入时复制到每用户、每应用位置。 | AppData 通常为 *C:\Users\user_name\AppData*。 |
| 在软件包内写入 | 不允许。 软件包为只读。 | 不允许在 *C:\Program Files\WindowsApps\package_name* 下写入。 |
| 在软件包外写入 | 如果用户具有权限，则允许。 | 如果软件包不包含 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll*，并且用户具有权限，则允许对 *C:\Windows\System32\foo.dll* 的写入。 |

### <a name="packaged-vfs-locations"></a>打包的 VFS 位置

下表显示了为应用在系统上的哪个位置覆盖作为程序包一部分交付的文件。 你的应用程序能够体会到这些文件时实际上它们是在内部重定向的位置中的列的系统位置，为*C:\Program Files\WindowsApps\package_name\VFS*。 根据 [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx) 常量确定 FOLDERID 位置。

系统位置 | 重定向位置 (位于 [packageroot 下] \VFS\) | 支持的体系结构
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

应用包含有一个 registry.dat 文件，该文件充当真实注册表中的 *HKLM\Software* 的逻辑等效项。 在运行时，此虚拟注册表将此配置单元的内容合并到原生系统配置单元，以提供两者的单一视图。 例如，如果 registry.dat 包含单个项“Foo”，则运行时 *HKLM\Software* 的读取也将显示为包含“Foo”（除了所有原生系统项）。

仅 *HKLM\Software* 下的项是软件包的一部分；*HKCU* 或注册表其他部分下的项不是。 不允许写入软件包中的项或值。 将写入到键或值不允许的包的一部分，只要用户具有的权限。

HKCU 下的所有写入都在写入时复制到每用户、每应用位置。 在传统上，卸载程序无法清除 *HKEY_CURRENT_USER*，因为注销用户的注册表数据已卸载并且不可用。

所有写入都是在包升级过程中保持，仅在完全删除该应用程序时删除。

### <a name="common-operations"></a>常见操作

此短引用表显示了常见注册表操作，以及操作系统如何处理它们。

操作 | 结果 | 示例
:--- | :--- | :---
读取或枚举 *HKLM\Software* | 软件包配置单元与本地系统对应项的动态合并。 | 如果 registry.dat 包含单个项“Foo”，则在运行时，*HKLM\Software* 的读取将显示 *HKLM\Software* 和 *HKLM\Software\Foo* 的内容。
在 HKCU 下写入 | 在写入时复制到每用户、每应用专用位置。 | 与文件的 AppData 相同。
在软件包内写入。 | 不允许。 软件包为只读。 | 如果软件包配置单元中存在对应的项/值，则不允许在 *HKLM\Software* 下写入。
在软件包外写入 | 忽略由操作系统。 如果用户具有权限，则允许。 | 只要软件包配置单元中不存在对应的项/值并且用户具有正确的访问权限，则允许在 *HKLM\Software* 下写入。

## <a name="uninstallation"></a>卸载

当用户卸载包时，所有文件和文件夹都位于下*C:\Program Files\WindowsApps\package_name*被删除，以及任何重定向到应用程序数据或已捕获期间在注册表写入打包过程。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。