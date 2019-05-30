# <a name="signing-windows-10-app-package"></a>Windows 10 应用程序包进行签名 

应用程序包签名是创建可以部署的 Windows 10 应用程序包的过程中的必需的步骤。 Windows 10 要求所有应用程序使用有效的代码签名证书进行签名。 

若要成功安装 Windows 10 应用程序，包就无需签名但还受信任的设备上。 这意味着该证书必须链接到一个在设备上受信任的根。 为默认值，Windows 10 信任来自大部分证书颁发机构提供代码签名证书的证书。 

|主题| 描述 |
|:---|:---|
|[签名的先决条件](https://docs.microsoft.com/en-us/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#prerequisites)| 本部分讨论对 Windows 10 应用包进行签名所需的先决条件。 | 
|[使用 SignTool](https://docs.microsoft.com/en-us/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#using-signtool)| 本部分讨论如何使用 Windows 10 SDK 中的 SignTool 应用程序包进行签名。|

## <a name="timestamping"></a>时间戳 

除了使用证书对应用程序包进行签名，决定了代码签名证书的有效性的另一个重要特征就是**加盖时间戳**。 时间戳将保留允许可接受的应用程序部署平台，即使该证书已过期的应用程序包的签名。 在包检查时，时间戳允许包的签名验证根据其进行签名的时间。 这使得包可以被接受甚至后证书不再有效。 将根据当前的时间计算是不带时间戳的包，如果证书不再有效，Windows 将接受包。 

以下是围绕应用签名的输入/输出时间戳的不同方案：

| |应用程序签名没有时间戳 | 使用时间戳签名的应用程序 |
|---|---------------------------------- | ------------------------------- |
| 证书有效 |将安装应用 | 将安装应用 |
| 证书是 invalid(expired) | 应用程序将无法安装 | 通过时间戳颁发机构进行签名通过验证证书的真实性将安装应用 |

 > [!NOTE]
 > 如果在设备上成功安装该应用程序，它将继续甚至在证书过期而不考虑它正在加盖时间戳后运行。 

## <a name="device-mode"></a>设备模式

Windows 10，用户可以选择要在其中运行其设备设置应用中的模式。 模式是 Microsoft Store 应用程序、 旁加载应用程序和开发人员模式。 

**Microsoft Store 应用**是最安全，因为它仅允许安装来自 Microsoft Store 的应用。 Microsoft Store 中的应用程序经过认证过程，以确保应用是安全的使用。 

**旁加载应用程序**并**开发人员模式**更宽松的证书所签名的其他，只要这些证书是受信任的应用和链，进入设备上受信任的根。 如果您是开发人员和生成或调试 Windows 10 应用，只能选择开发人员模式。 可以找到有关开发人员模式和它提供了更多信息[此处](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development)。 
