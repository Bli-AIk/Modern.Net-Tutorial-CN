本仓库为源仓库的汉化fork。使用了 ChatGPT 协助翻译，仅供参考。

翻译内容可能包含误差或不准确之处，请结合原文自行斟酌使用。

若涉及版权问题，请联系原作者或版权所有方。

[原文](https://github.com/mysteryx93/Modern.Net-Tutorial/blob/main/README.md)

# 教程：使用 .NET 构建现代跨平台应用

[1. 引言](README.md)  
[2. Avalonia UI](2_Avalonia.md)  
[3. 依赖注入](3_DependencyInjection.md)  
[4. MVVM 设计](4_MVVM.md)  
[5. 对话框与工具](5_DialogsTools.md)  
[6. 单元测试](6_UnitTesting.md)  
[7. 响应式编程](7_Reactive.md)  
[8. 应用部署](8_Deployment.md)  
[9. 程序集裁剪](9_AssemblyTrimming.md)  
[10. 多环境支持](10_MultipleEnvironments.md)  

## 1. 引言

**作者：Etienne Charland**  

我最早在 .NET 1.0 Beta 1 时代就开始使用 .NET 进行编程。从 Visual Basic 过渡到 .NET 是一件大事，当时大家对 .NET 充满了期待！如今，.NET 已经走过了漫长的发展历程。

随着时间推移，设计模式不断演进，开发工具也日益完善。以前，我会使用 N-Tier 设计开发 WinForms 应用，而如今，这种设计方式已经被认为是混乱无章的“意大利面条代码”。

我之所以编写这篇教程，是因为尽管 .NET 在 Web 开发领域取得了巨大成功，但在桌面开发领域却几乎销声匿迹，而要找到合适的工具也需要经历漫长的学习曲线。我花费了大量时间提升自己的技能，希望这篇教程能帮助更多开发者磨砺自己的技艺！

### .NET 的优势

.NET 是一个极为出色的编程平台。有些人喜欢能够快速出成果的简单脚本语言，但我个人非常讨厌脚本语言。我希望构建的是结构清晰、坚实稳固，并且能够轻松扩展的应用。**在软件开发中，捷径终将崎岖，远途反成坦途。**.NET 可能在某些简单任务上稍显繁琐，但在处理复杂任务时，它的优势就会显现。我推荐使用 C#，因为网上的代码示例大多采用 C#，如果你想尝试更高级的语言，有些人对 F# 也颇为推崇。

.NET/C# 主要有以下优势：
- **高性能***  
- **跨平台**（支持 Windows、Linux、MacOS、iOS、Android）  
- **可以嵌入框架，无需额外依赖**  
- **拥有强大的社区支持**  
- **语言语法直观简洁**  
- **桌面应用和 Web 应用可以使用相同的语言**  

*关于性能*：你可能会认为 C++ 具有更高的性能，这一点毋庸置疑。因此，如果某些底层任务对性能要求极高，那么可以用 C++ 进行开发。然而，影响整个应用程序性能的关键因素在于代码的整体架构。如果代码结构混乱，即便是 C++ 也会变成性能瓶颈。如今，应用程序的性能更多取决于高效的多线程处理，而 C++ 的多线程编程异常复杂，而 .NET 提供了 **await/async** 关键字，使多线程编程变得异常简单。.NET 允许开发者构建高层次的应用架构，按需调用独立组件，并在必要时集成原生 C++ 库。与 Python 或 Java 相比，.NET 的性能要优越得多。

### .NET 在桌面开发中的困境

微软在 2002 年推出了 WinForms，随后在 2006 年发布了 WPF，使得开发者能够采用 MVVM 设计模式，实现 UI 与业务逻辑的完全分离。到这里一切看似都很顺利，但问题在于 MONO（.NET 的跨平台实现）从未对 WPF 给予足够的重视，导致 WPF 不能在 Linux 或 MacOS 上运行，迫使 MONO 只能依赖过时的 WinForms 作为唯一的跨平台桌面 UI 方案。

2015 年，微软推出了 UWP（通用 Windows 平台）用于开发 Windows 商店应用。如果你在全美范围内搜索 UWP 相关的招聘岗位，你可能只能找到寥寥无几的职位，这足以说明 UWP 的失败。UWP 充满了各种限制，并且只能运行在 Windows 上。虽然有一个基于 UWP 的 UNO 框架，使其能够跨平台运行，但依然要承受 UWP 本身的诸多限制。随后，微软又陆续推出了 WinUI 1、WinUI 2 和 WinUI 3，但这些方案存在大量问题。微软目前正在开发 MAUI，使 .NET 应用能够运行在 MacOS 上，但即便如此，它仍然不支持 Linux。我不知道你是否了解这些平台 —— 反正我并没有全部学习。

我的意思是，.NET 桌面开发的工具链极度混乱，因此大多数开发者都选择了其他替代方案。

但等等！其实还有一个选项：[**Avalonia UI**](https://avaloniaui.net/)。这是对 WPF 的完全重写，吸收了 UWP 和 WinUI 的各种现代化改进，并且能够运行在所有主要平台上：Windows、Linux、MacOS、Android、iOS，甚至 WebAssembly。如今，它已经足够成熟，可以用于商业项目。

**尽管这里介绍的工具非常强大，但由于学习曲线陡峭，使用这些工具的开发者并不多，网上的相关资料也相对有限。我希望这篇教程能帮助更多人了解和使用这些技术，从而壮大社区！**

### 我的开发环境

在 Windows 11 发布之前，我已经厌倦了 Windows 对用户电脑的种种控制，因此彻底放弃了它。我目前使用 Linux，如果你是开发者，基于 Arch 的 Linux 发行版是一个不错的选择，因为它始终保持最新的软件版本。我个人选择了 [**Garuda Linux**](https://garudalinux.org/)，并且非常喜欢它（但千万不要给你全家老小都装上基于 Arch 的 Linux！）。我在虚拟机里保留了 Windows，但基本上很少使用。这迫使我对自己的所有应用进行了现代化改造。

至于开发工具，Visual Studio 只能运行在 Windows 上，而 Visual Studio Code 功能有限。因此，我使用的是 [**JetBrains Rider**](https://www.jetbrains.com/rider/)，它支持 Windows、Linux 和 MacOS。JetBrains 为学生和开源开发者提供免费授权（前提是你需要有活跃的项目历史）[点击这里查看详情](https://www.jetbrains.com/rider/buy/#discounts)。即便你在 Windows 上开发，我仍然建议你尝试 JetBrains Rider，你会发现它的生产力远胜 Visual Studio，一旦尝试就再也回不去了。而且，JetBrains Rider 对 Avalonia UI 有着出色的支持。

总结一下 —— **如果你想更好地使用 .NET，那就抛弃 Windows、WPF 和 Visual Studio。**

构建现代应用涉及许多技术和工具，我会在教程中介绍一系列重要的内容。每个主题你都可以找到专门的书籍深入学习，而我会结合自己的经验，为你总结核心要点，并提供相关学习资源。

我将在本教程中多次使用 [**432hz 批量转换器**](https://sourceforge.net/projects/converter432hz/) 作为示例代码。它支持 Windows、Linux 和 MacOS，你可以下载试用。

如果你有任何问题或想法，可以在 [**讨论区**](https://github.com/mysteryx93/Modern.Net-Tutorial/discussions) 进行交流。

[> 下一章：Avalonia UI](2_Avalonia.md)