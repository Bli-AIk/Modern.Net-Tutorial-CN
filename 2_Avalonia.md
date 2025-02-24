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

## 2. Avalonia UI

Avalonia 是一个类似现代 WPF 的 UI，适用于所有操作系统：Windows、Linux、MacOS、iOS、Android 甚至 Web Assembly！它已经足够成熟，可用于商业用途。

首先，在您的 IDE 中[安装插件](https://docs.avaloniaui.net/docs/next/get-started/set-up-an-editor)。

### 资源

*   [主网站](https://avaloniaui.net/)
*   [GitHub 源代码](https://github.com/AvaloniaUI/Avalonia)
*   [文档](https://docs.avaloniaui.net/)
* [FluentAvalonia](https://github.com/amwx/FluentAvalonia) - 提供更优秀的 UI  
* [AwesomeAvalonia](https://github.com/AvaloniaCommunity/awesome-avalonia) - 第三方工具列表  

Avalonia官方文档编写得非常出色。你应该花时间阅读各个部分。它与 WPF 非常相似，但有一些差异很大的改进。将 WPF 转换为 Avalonia 要比反过来容易得多。对于未记录的类，您通常可以参考 WPF 和 UWP 文档，它们会非常相似。

如需更多帮助：

*   [GitHub Avalonia 讨论](https://github.com/AvaloniaUI/Avalonia/discussions)
*   [GitHub Avalonia Issues](https://github.com/AvaloniaUI/Avalonia/issues)
*   [StackOverflow](https://stackoverflow.com/)
*   [Discord DotNetEvolution](https://discord.com/invite/HSuhTyG)

### XAML

不要关注代码隐藏部分；那将在 MVVM 部分中讨论。你需要学习的是 XAML 语言以及创建样式和资源。练习创建一些不执行任何操作的优秀 UI。

一个限制是你没有可视化编辑器，只能获得可视化预览。这迫使你手动编写所有 XAML。正如我所发现的，这是一件好事。我的 WPF 代码都是使用绝对坐标和大边距来定位控件的。这不是布局应该采用的方式。 `StackPanel` 是布局的最佳伙伴，所以请使用它来代替。

同时使用 `Grid` 与行和列。边距应仅用于控制控件之间的间距，而不是用于在窗口中定位控件。

<Grid Margin="10,6,10,10" ColumnDefinitions="150,*" RowDefinitions="*,40">


使用 StackPanel 和网格重写自动生成的 WPF 代码，您将获得更简洁的代码。

### FluentAvalonia

FluentAvalonia 提供了更好的样式和更多控件，因此您可能也想使用它。Avalonia 中没有内置的 MessageBox，而 FluentAvalonia 提供了这一功能。

下载他们的源代码并运行示例应用程序，以查看所有控件的实际效果。示例应用程序还允许您浏览所有样式资源和活动颜色，因此是一个非常实用的工具。

### 图标

Svg.Skia 是 SVG 图标的一个选项；但它无法动态调整图标颜色。

FluentAvalonia 为 `SymbolIcon` 提供了内置图标，满足您的大部分需求。下载其源代码并运行示例应用程序以查看它们。

<flu:SymbolIcon Symbol="Add" FontSize="20" />


如果你需要的图标不在 SymbolIcon 中，有一种方法可以将 SVG 转换为 Path，但这可能会变得复杂，而且 Path 要求你设置固定的 Brush，因此它不会适应按钮的 Forecolor。

我发现创建一个包含自定义图标的字体更容易。

[以下是使用 FontForge 创建图标字体的简单指南。](https://mohammedraji.github.io/posts/The-Definitive-guide-to-create-an-icon-font/)

我的按钮看起来像这样

<Button Classes="icon" Width="35" Content="I" />


使用 `icon` 定义为

<Style Selector="Button.icon">
<Setter Property="FontFamily" Value="avares://Common.Avalonia.App/Styles/Icons.otf#" />
<Setter Property="FontSize" Value="17" />
</Style>


### 示例代码

以下是一些使用这些原则创建的示例视图。我已在 App.xaml 中注册了自定义样式和资源。

该应用随后与可即时设置的浅色和深色主题完美配合。这些样式添加了背景渐变，将按钮样式更改为类似 FluentAvalonia 的“强调”风格，并带有 35%的不透明度，调整了 ListBox 使得双击覆盖整个项目区域，并根据我的需求调整了一些细节。

### 可视化调试

设置和更改样式可能很困难，因为您不知道控件内部是如何构建的。

首先，运行你的应用程序并按下 F12 以打开 Avalonia DevTools。将工具窗口放在屏幕右侧，你的应用程序放在左侧。点击 Visual Tree 选项卡，将鼠标悬停在你想要分析的控件上，然后按下 CTRL+SHIFT。它将在可视化树中浏览到该元素，你可以看到哪些属性正在生效。

此外，您可以浏览源样式，如果使用内置样式，则为 Avalonia.Themes.Fluent，或者 FluentAvalonia。

### Code-Behind

最后，这是我的代码隐藏文件的样子。我想保持这种方式。你可能会好奇我是如何实现一个完全没有代码隐藏文件的全功能应用的。我稍后会讲到这一点。

public partial class MainView : CommonWindow<MainViewModel>
{
protected override void Initialize() => AvaloniaXamlLoader.Load(this);
}


在 XAML 中，'d' 前缀允许设置设计时属性。这允许为设计器设置 DataContext。我们将在下一节中创建 ViewModelLocator 类。

d:DataContext="{x:Static local:ViewModelLocator.Main}"


### 创建您的项目

有两个项目模板可供选择：仅限桌面或跨平台。说明在此处和此处。

确保你在 `csproj` 中针对正确的框架，启用了 Nullable，并使用了最新的语言特性。

<TargetFramework>net7.0</TargetFramework>
<Nullable>enable</Nullable>
<LangVersion>default</LangVersion>


对于代码风格，如果你想使用最推荐的编码实践，可以安全地从.NET Roslyn 编译器项目中获取 `.editorconfig` 文件。将其放置在解决方案文件夹中，以便所有项目均可使用。

同时确保创建一个包含您常用命名空间的文件 `Usings.cs` 。

global using System;
global using System.Collections;
global using System.Collections.Generic;
global using System.Threading.Tasks;
global using RxCommandUnit = ReactiveUI.ReactiveCommand<System.Reactive.Unit, System.Reactive.Unit>;
global using static System.FormattableString;


### App.xaml.cs

将此放入 `App.xaml.cs`

截至撰写本文时，存在一个错误，即设计器无法识别自定义库的 XAML 命名空间。您必须在此类库中的任何类上使用 `GC.KeepAlive` 来解决此问题。

此外，我发现使用设计器时，初始化 MainView 和应用程序是没有意义的；你可以跳过应用程序初始化，预览会显示得更快。

public override async void OnFrameworkInitializationCompleted()
{
#if DEBUG
// Required by Avalonia XAML editor to recognize custom XAML namespaces. Until they fix the problem.
GC.KeepAlive(typeof(SvgImage));
GC.KeepAlive(typeof(EventTriggerBehavior));

if (Design.IsDesignMode)
{
base.OnFrameworkInitializationCompleted();
return;
}
#endif


[\> 下一步：依赖注入](3_DependencyInjection.md)

