---
title: MSIX 打包工具发行说明
description: MSIX 打包工具发行说明的完整历史记录
author: c-don
ms.author: cdon
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10、 uwp、 msix、 msix 打包工具、 预览体验计划
ms.localizationpriority: medium
ms.custom: Vibranium
ms.openlocfilehash: be65c2838d189760707e859bfd97751424af2d35
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900789"
---
# <a name="msix-packaging-tool-release-notes"></a>MSIX 打包工具发行说明 

#### <a name="ver-120194020"></a>Ver 1.2019.402.0

 - 验证 COM ProgId 的类型值，COM 类项并删除 COM 注册无效
 - 更新 MSIX 打包工具中重新分发的 Windows SDK 工具 

#### <a name="ver-120193220"></a>Ver 1.2019.322.0

 - 自动将参数拆分为单独的字段对清单表示 com 可执行文件

#### <a name="ver-120193150"></a>Ver 1.2019.315.0

 - 可靠的处理 PowerShell 安装程序参数
 - 执行禁用 Windows 更新服务的计算机准备阶段中所必需的步骤

#### <a name="ver-120193080"></a>Ver 1.2019.308.0

- 添加了对时间戳已签名的包中所有其中签名是当前可用的工作流
    - 在工具设置页中，可以指定您的默认时间戳 URL 和时间戳服务器的类型 
- 将包保存后将保留在 VFS 包编辑器中创建的空文件夹

#### <a name="ver-120193040"></a>Ver 1.2019.304.0

新功能：

- 用户可以指定有效的预期的退出代码，CLI 转换
- 在转换过程中生成的空文件夹将在打包保持不变
- 更新[AppID 生成逻辑](#appid-generation-logic)，并添加为包名称和应用程序的其他验证 

##### <a name="appid-generation-logic"></a>AppID 生成逻辑
当前派生应用程序 ID 的过程如下所示： 
1. 查找 exe/msi 名称，并在扩展
2. 将转换为大写
3. 删除所有非字母数字字符
4. 转换为相应的英语单词数字
5. 如果生成的字符串包含不超过 3 个字符，请改为使用字符串"应用"
6. 如果生成的字符串包含超过 64 个字符，则将其截断。
7. 存在冲突，截断字符串，如果它包含多个 62 个字符，并追加 00 开始的两位数字值。

#### <a name="ver-120192260"></a>Ver 1.2019.226.0
新功能：

- 可以将远程计算机的上[的详细信息](../remote-conversion-setup.md)
- 改进了包编辑器中的管理体验
- 在包的编辑器中保存时自动版本控制建议

其他更新

- 添加了使用"。" 进度的版本字段
- 已修复的验证的安装位置
- 固定清单转换问题的文件类型关联的处理程序和 com 服务器条目
- 添加获取的命令行转换状态的功能
- 改进了的 COM 警告的日志记录与人工可读的错误

