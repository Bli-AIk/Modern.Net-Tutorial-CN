# 教程：使用 .NET 构建现代跨平台应用

[1. 引言](README.md)  
[2. Avalonia UI](2_Avalonia.md)  
[3. 依赖注入](3_DependencyInjection.md)  
[4. MVVM 设计模式](4_MVVM.md)  
[5. 对话框与工具](5_DialogsTools.md)  
[6. 单元测试](6_UnitTesting.md)  
[7. 响应式编程](7_Reactive.md)  
[8. 部署](8_Deployment.md)  
[9. 程序集裁剪](9_AssemblyTrimming.md)  
[10. 多环境支持](10_MultipleEnvironments.md)  

## 2. Avalonia UI

Avalonia 是一个现代化的 UI 框架，类似于 WPF，支持所有主流操作系统：Windows、Linux、macOS、iOS、Android，甚至 WebAssembly！它已经足够成熟，可以用于商业项目。

首先，在你的 IDE 中安装 Avalonia 插件。[安装指南](https://docs.avaloniaui.net/docs/next/get-started/set-up-an-editor)

### 资源

- [官方网站](https://avaloniaui.net/)  
- [GitHub 源代码](https://github.com/AvaloniaUI/Avalonia)  
- [官方文档](https://docs.avaloniaui.net/)  
- [FluentAvalonia](https://github.com/amwx/FluentAvalonia) - 提供更好的 UI 组件  
- [AwesomeAvalonia](https://github.com/AvaloniaCommunity/awesome-avalonia) - Avalonia 生态工具合集  

Avalonia 的官方文档写得非常详细，建议花时间仔细阅读各个部分。它与 WPF 非常相似，但也做出了一些显著改进。将 WPF 代码迁移到 Avalonia 相对容易，而反过来则较为困难。对于官方文档中未涵盖的类，可以参考 WPF 和 UWP 的相关文档，它们在很多方面是相似的。

如需进一步帮助，可参考以下资源：
- [GitHub Avalonia 讨论区](https://github.com/AvaloniaUI/Avalonia/discussions)  
- [GitHub Avalonia 问题跟踪](https://github.com/AvaloniaUI/Avalonia/issues)  
- [StackOverflow](https://stackoverflow.com/)  
- [Discord DotNetEvolution](https://discord.com/invite/HSuhTyG)  

### XAML 设计

目前暂时不用关注代码绑定部分，这将在 MVVM 章节介绍。你需要先学习 XAML 语言，并掌握如何创建样式和资源。可以先尝试设计一些漂亮的 UI，而不涉及任何逻辑。

一个限制是，Avalonia 并没有可视化编辑器，只有 XAML 预览功能，因此你需要手写所有 XAML 代码。但这实际上是一个优势。我最初在 WPF 中编写 UI 时，都是通过绝对坐标和大量的 `Margin` 来定位控件，而这并不是正确的做法。布局应更多依赖 `StackPanel`。

此外，还应使用 `Grid` 来定义行和列。`Margin` 仅用于控制控件之间的间距，而不应该用于确定控件在窗口中的位置。

```xaml
<Grid Margin="10,6,10,10" ColumnDefinitions="150,*" RowDefinitions="*,40">
```

你可以尝试将自动生成的 WPF 代码改写成 `StackPanel` 和 `Grid` 结构，这样会让代码更加清晰。

### FluentAvalonia

[FluentAvalonia](https://github.com/amwx/FluentAvalonia) 提供了更美观的样式和更多控件，你可能会希望使用它。Avalonia 本身没有内置 `MessageBox`，但 FluentAvalonia 提供了这个功能。

你可以下载 FluentAvalonia 的源代码并运行示例应用，以查看所有控件的实际效果。该示例应用还允许你浏览所有的样式资源和当前的颜色方案，因此是一个非常有用的工具。

### 图标处理

[Svg.Skia](https://github.com/wieslawsoltes/Svg.Skia) 可以用于处理 SVG 图标，但它无法动态调整图标颜色。

FluentAvalonia 提供了 `SymbolIcon`，其中包含了大多数常见的图标。你可以下载 FluentAvalonia 的源代码并运行示例应用来查看所有可用图标。

```xaml
<flu:SymbolIcon Symbol="Add" FontSize="20" />
```

如果 `SymbolIcon` 中没有你需要的图标，可以将 SVG 转换为 `Path`，但这可能较为复杂。此外，`Path` 需要手动指定 `Brush`，因此不会自动适配按钮的前景色。

我个人更倾向于创建一个包含自定义图标的字体。

[这里有一篇关于使用 FontForge 创建图标字体的简单指南](https://mohammedraji.github.io/posts/The-Definitive-guide-to-create-an-icon-font/)。

我的按钮代码如下：
```c#
<Button Classes="icon" Width="35" Content="I" />
```

`icon` 样式定义如下：
```xaml
<Style Selector="Button.icon">
    <Setter Property="FontFamily" Value="avares://Common.Avalonia.App/Styles/Icons.otf#" />
    <Setter Property="FontSize" Value="17" />
</Style>
```

### 示例代码

[这里有一些基于这些原则创建的示例 View](https://github.com/mysteryx93/HanumanInstituteApps/tree/master/Src/App.Converter432Hz/Converter432Hz/Views)。  
我创建了一些自定义[样式](https://github.com/mysteryx93/HanumanInstituteApps/blob/master/Src/Apps/Styles/CommonStyles.axaml)和[资源](https://github.com/mysteryx93/HanumanInstituteApps/blob/master/Src/Apps/Styles/CommonResources.axaml)，并在 [App.xaml](https://github.com/mysteryx93/HanumanInstituteApps/blob/master/Src/App.Converter432Hz/Converter432Hz/App.axaml) 中注册它们。

应用程序支持动态切换明暗主题。样式中添加了背景渐变，使按钮样式类似 FluentAvalonia 的“强调”风格（不透明度 35%），修改了 `ListBox` 以便双击时整个项都可交互，并调整了一些 UI 细节。

### 可视化调试

调整样式可能较为困难，因为你无法直接看到控件的内部结构。

首先，运行应用程序并按 `F12` 打开 Avalonia DevTools。将工具窗口放在屏幕右侧，应用程序界面放在左侧。点击“可视化树”选项卡，鼠标悬停在你想分析的控件上，然后按 `CTRL+SHIFT`，即可在可视化树中定位该元素，并查看其生效的属性。

你还可以浏览源代码：
- 如果使用内置样式，可以参考 [Avalonia.Themes.Fluent](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Themes.Fluent)
- 如果使用 FluentAvalonia，可以参考 [FluentAvalonia 样式库](https://github.com/amwx/FluentAvalonia/tree/master/src/FluentAvalonia/Styling)

### 代码绑定

最终，我的代码绑定方式如下。我希望保持这种风格。你可能会疑惑我是如何做到完全无代码绑定的，稍后会介绍这一点。

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

### 创建项目

Avalonia 提供了桌面专用模板和跨平台模板。[项目创建指南](https://docs.avaloniaui.net/docs/next/get-started/test-drive/create-a-project)

请确保你的 `csproj` 文件中包含以下内容：

```
<TargetFramework>net7.0</TargetFramework>
<Nullable>enable</Nullable>
<LangVersion>default</LangVersion>
```

[> 下一章：依赖注入](3_DependencyInjection.md)