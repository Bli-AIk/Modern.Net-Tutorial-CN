[原文](https://github.com/mysteryx93/Modern.Net-Tutorial/blob/main/2_Avalonia.md)

# 教程：使用 .NET 构建现代跨平台应用

[1. 引言](README.md)  
[2. Avalonia UI](2_Avalonia.md)  
[3. 依赖注入](3_DependencyInjection.md)  
[4. MVVM 设计](4_MVVM.md)  
[5. 对话框与工具](5_DialogsTools.md)  
[6. 单元测试](6_UnitTesting.md)  
[7. 响应式编程](7_Reactive.md)  
[8. 部署](8_Deployment.md)  
[9. 程序集裁剪](9_AssemblyTrimming.md)  
[10. 多环境支持](10_MultipleEnvironments.md)  

## 2. Avalonia UI

Avalonia 是一个现代化的 UI 框架，类似于 WPF，可运行于所有操作系统，包括 Windows、Linux、macOS、iOS、Android，甚至 WebAssembly！它已经足够成熟，可以用于商业开发。

首先，在你的 IDE 中安装插件。[安装指南](https://docs.avaloniaui.net/docs/next/get-started/set-up-an-editor)

### 资源

- [官方网站](https://avaloniaui.net/)  
- [GitHub 源代码](https://github.com/AvaloniaUI/Avalonia)  
- [官方文档](https://docs.avaloniaui.net/)  
- [FluentAvalonia](https://github.com/amwx/FluentAvalonia) - 提供更优秀的 UI  
- [AwesomeAvalonia](https://github.com/AvaloniaCommunity/awesome-avalonia) - 第三方工具列表  

官方文档写得非常好，建议花时间阅读各个部分。Avalonia 与 WPF 十分相似，但在某些方面有所改进。将 WPF 代码转换为 Avalonia 远比反向转换要容易。对于未记录的类，你通常可以参考 WPF 和 UWP 的文档，因为它们的结构大多相似。

更多帮助：
- [GitHub Avalonia 讨论区](https://github.com/AvaloniaUI/Avalonia/discussions)  
- [GitHub Avalonia 问题跟踪](https://github.com/AvaloniaUI/Avalonia/issues)  
- [StackOverflow](https://stackoverflow.com/)  
- [Discord DotNetEvolution](https://discord.com/invite/HSuhTyG)  

### XAML

暂时不要关注代码隐藏（code-behind）部分，这将在 MVVM 章节介绍。现在的重点是学习 XAML 语言，以及如何创建样式和资源。可以先练习设计一些 UI，而不需要让它们真正运行。

Avalonia 目前没有可视化编辑器，只有预览功能，这意味着你需要手写所有 XAML 代码。但这其实是一个优点。我最初的 WPF 代码是通过绝对坐标和大量 `Margin` 来布局控件的，这并不是最佳的方式。在 Avalonia 中，`StackPanel` 是你的好朋友，应该优先使用它来布局。

此外，可以使用 `Grid` 进行行列布局，`Margin` 仅用于调整控件之间的间距，而不是用来定位控件。

```xaml
<Grid Margin="10,6,10,10" ColumnDefinitions="150,*" RowDefinitions="*,40">
```

建议将 WPF 代码中自动生成的绝对定位布局转换为 `StackPanel` 和 `Grid`，这样可以使代码更加整洁。

### FluentAvalonia

[FluentAvalonia](https://github.com/amwx/FluentAvalonia) 提供了更优秀的样式和控件，因此值得一试。Avalonia 本身不包含 `MessageBox`，而 FluentAvalonia 提供了这一功能。

下载其源代码并运行示例应用，可以看到所有控件的实际效果。该示例应用还允许你浏览所有样式资源和当前的配色方案，因此是一个非常实用的工具。

### 图标

[Svg.Skia](https://github.com/wieslawsoltes/Svg.Skia) 可用于显示 SVG 图标，但它无法动态调整图标颜色。

FluentAvalonia 提供了 `SymbolIcon`，其中包含了大部分常见的图标。你可以运行其示例应用来查看所有内置图标。

```xaml
<flu:SymbolIcon Symbol="Add" FontSize="20" />
```

如果 `SymbolIcon` 中没有你需要的图标，你可以将 SVG 转换为 `Path`，但这可能比较复杂，而且 `Path` 需要手动指定 `Brush`，无法自动适应按钮的前景色。

更简单的方法是创建一个自定义的字体图标集。

[这里有一篇关于使用 FontForge 创建图标字体的教程](https://mohammedraji.github.io/posts/The-Definitive-guide-to-create-an-icon-font/)

例如，我的按钮定义如下：
```c#
<Button Classes="icon" Width="35" Content="I" />
```
然后在 `icon` 样式中指定字体：
```xaml
<Style Selector="Button.icon">
    <Setter Property="FontFamily" Value="avares://Common.Avalonia.App/Styles/Icons.otf#" />
    <Setter Property="FontSize" Value="17" />
</Style>
```

### 示例代码

[这里有一些基于上述原则创建的示例视图](https://github.com/mysteryx93/HanumanInstituteApps/tree/master/Src/App.Converter432Hz/Converter432Hz/Views)。  
我还创建了自定义的 [样式](https://github.com/mysteryx93/HanumanInstituteApps/blob/master/Src/Apps/Styles/CommonStyles.axaml) 和 [资源](https://github.com/mysteryx93/HanumanInstituteApps/blob/master/Src/Apps/Styles/CommonResources.axaml)，它们在 [App.xaml](https://github.com/mysteryx93/HanumanInstituteApps/blob/master/Src/App.Converter432Hz/Converter432Hz/App.axaml) 中注册。

该应用支持明暗主题切换，并能实时应用。样式包括：
- 背景渐变色
- 按钮样式类似 FluentAvalonia 的 "Accent" 风格，透明度 35%
- `ListBox` 支持双击整个项目区域
- 其他适用于我的调整

### 视觉调试

调整样式时，可能很难了解控件的内部结构。

运行应用并按 `F12`，打开 Avalonia DevTools。将工具窗口放在屏幕右侧，应用放在左侧。点击 “Visual Tree” 选项卡，悬停在想要分析的控件上，然后按 `CTRL+SHIFT`，工具将定位到该元素，并显示生效的属性。

此外，你可以浏览 Avalonia 内置样式 [Avalonia.Themes.Fluent](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Themes.Fluent) 或 [FluentAvalonia](https://github.com/amwx/FluentAvalonia/tree/master/src/FluentAvalonia/Styling)。

### 代码隐藏

最终，我的代码隐藏文件非常精简，如下所示：
```c#
public partial class MainView : CommonWindow<MainViewModel>
{
    protected override void Initialize() => AvaloniaXamlLoader.Load(this);
}
```
在 XAML 中，`d` 前缀用于设置设计时属性，例如：
```xaml
d:DataContext="{x:Static local:ViewModelLocator.Main}"
```
我们将在后续章节创建 `ViewModelLocator` 类。

### 创建你的项目

Avalonia 提供了两种项目模板：桌面端和跨平台。创建项目的说明在 [这里](https://docs.avaloniaui.net/docs/next/get-started/test-drive/create-a-project) 和 [这里](https://docs.avaloniaui.net/docs/next/guides/building-cross-platform-applications/solution-setup)。

确保你的 `.csproj` 目标框架正确，并启用了空值检查：
```xml
<TargetFramework>net7.0</TargetFramework>
<Nullable>enable</Nullable>
<LangVersion>default</LangVersion>
```

### App.xaml.cs

在 `App.xaml.cs` 中添加以下代码：
```c#
public override async void OnFrameworkInitializationCompleted()
{
#if DEBUG
    GC.KeepAlive(typeof(SvgImage));
    GC.KeepAlive(typeof(EventTriggerBehavior));

    if (Design.IsDesignMode)
    {
        base.OnFrameworkInitializationCompleted();
        return;
    }
#endif
```

[> 下一章：依赖注入](3_DependencyInjection.md)