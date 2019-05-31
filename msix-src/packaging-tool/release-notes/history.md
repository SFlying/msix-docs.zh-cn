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
ms.openlocfilehash: 52a1306f9bf484b828cfa4471030dd0065f90981
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400684"
---
# <a name="msix-packaging-tool-release-notes"></a>MSIX 打包工具发行说明 

#### <a name="ver-120195220"></a>Ver 1.2019.522.0

新功能：

- 支持桌面安装程序需要重新启动-[了解详细信息](../support-restart.md)
    - 重新启动的自动登录选项 
- 应用设置中的新选项
    - 指定的默认证书进行签名的包 
    - 对于需要重新启动的安装程序指定的退出代码
    
已知问题：

- 目前不支持负的重新启动退出代码
- 如果指定默认证书，则每个转换工作流将需要选择使用证书
- 在远程或虚拟机重启，可能会出现额外的登录提示 
- 还原默认设置按钮不会删除证书密码或安装程序退出代码

### <a name="ver-120194020---public-release"></a>**Ver 1.2019.402.0-公开发布的版本**

 - 可以将远程计算机的上[的详细信息](../remote-conversion-setup.md)
 - 验证 COM ProgId 的类型值，COM 类项并删除 COM 注册无效
 - 更新 MSIX 打包工具中重新分发的 Windows SDK 工具 
 - 自动将参数拆分为单独的字段对清单表示 com 可执行文件
 - 可靠的处理 PowerShell 安装程序参数
 - 执行禁用 Windows 更新服务的计算机准备阶段中所必需的步骤
- 添加了对时间戳已签名的包中所有其中签名是当前可用的工作流
    - 在工具设置页中，可以指定您的默认时间戳 URL 和时间戳服务器的类型 
- 将包保存后将保留在 VFS 包编辑器中创建的空文件夹
- 用户可以指定有效的预期的退出代码，CLI 转换
- 在转换过程中生成的空文件夹将在打包保持不变
- 更新应用程序标识生成的逻辑，并为包名称和应用程序添加额外的验证 
- 改进了包编辑器中的管理体验
- 在包的编辑器中保存时自动版本控制建议
- 添加了使用"。" 进度的版本字段
- 已修复的验证的安装位置
- 固定清单转换问题的文件类型关联的处理程序和 com 服务器条目
- 添加获取的命令行转换状态的功能
- 改进了的 COM 警告的日志记录与人工可读的错误
