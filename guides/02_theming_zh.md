
英文链接地址：https://material.angular.io/guide/theming


# 给你的 Angular Material 应用添加主题

## 主题是什么？

主题是应用于`Angular Material`组件的一系列颜色。这个库的主题方式是基于`Material Design`设计规范的指南的。

在`Angular Material`中，主题是由多个组合的色调创建的。具体来说，一套主题包含：

	一个主色调：所有视图和组件使用最广泛的色彩。
	一个表示强调的色调：用于悬浮动作按钮和交互元素。
	一个表示警告的色调：用于传递错误的状态信息。
	前景色：用于文本和图标。
	背景色：用于元素的背景。

在`Angular Material`中，所有的主题样式在构建时静态地创建，所以你的应用在初创阶段不需要花费时间在创建主题样式上。


## 使用预制的主题

`Angular Material`打包了几套预制的主题样式文件。这些主题文件同时也包含了所有核心的样式文件（适用于所有组件的样式），所以你只需要在你的应用中包含一个单独的`Angular Material`css文件。

你可以从`@angular/material/prebuilt-themes`模块中，把一个主题文件直接包含进你的应用中。

可用的预制主题有以下几种：

	deeppurple-amber.css
	indigo-pink.css
	pink-bluegrey.css
	purple-green.css


如果你使用`Angular CLI`，你只需在`styles.css`文件中引入一行代码：

	@import '~@angular/material/prebuilt-themes/deeppurple-amber.css';


或者，你可以通过`<link>`标签来直接引用主题样式文件：

	<link href="node_modules/@angular/material/prebuilt-themes/indigo-pink.css" rel="stylesheet">


真实的文件路径取决于你的服务器设置。

你也可以把主题样式文件与你的项目中其他样式文件合并。


最后，如果你的项目内容不是放在`md-sidenav-container`标签内，你需要给你的外层包裹的标签（比如<body>）增加一个名为`mat-app-background`的`class`。这样，方可确保正确的主题背景在你的页面中生效。


## 自定义主题

当你想要更加个性化，你可以创建自己的主题文件。

自定义主题文件做以下两件事：
	
	1.导入名为`mat-core()`的 sass mixin。 它包含了各种组件使用的所有通用样式。它只需要在你的项目中引入一次。如果这个 mixin 多次引入， 你的项目最终会重复多次这些通用样式。

	2.定义主题数据结构，作为色调的组成部分。这个对象可以由`mat-light-theme`或者`mat-dark-theme`方法创建，然后这个方法的返回值传给一个名为`angular-material-theme`的 mixin 。它会为这个主题输出所有相关的样式。


一个典型的主题文件如下代码所示：

	@import '~@angular/material/theming';
	// Plus imports for other components in your app.

	// Include the common styles for Angular Material. We include this here so that you only
	// have to load a single css file for Angular Material in your app.
	// Be sure that you only ever include this mixin once!
	@include mat-core();

	// Define the palettes for your theme using the Material Design palettes available in palette.scss
	// (imported above). For each palette, you can optionally specify a default, lighter, and darker
	// hue.
	$candy-app-primary: mat-palette($mat-indigo);
	$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);

	// The warn palette is optional (defaults to red).
	$candy-app-warn:    mat-palette($mat-red);

	// Create the theme object (a Sass map containing all of the palettes).
	$candy-app-theme: mat-light-theme($candy-app-primary, $candy-app-accent, $candy-app-warn);

	// Include theme styles for core and each component used in your app.
	// Alternatively, you can import and @include the theme mixins for each component
	// that you are using.
	@include angular-material-theme($candy-app-theme);


你只需要这一个 Sass 文件, 不需要使用 Sass 来编写项目其他的样式。

如果你使用`Angular CLI`，内置支持把 sass 文件编译成 css。你只需要在配置文件`angular-cli.json`的`styles`中添加一个入口，指向主题文件（假设你的主题文件名为 unicorn-app-theme.scss）。

如果你没使用`Angular CLI`，你可以使用任何现成的 Sass 工具（比如 gulp-sass 或 grunt-sass ）来编译文件。最简单的方法是使用`node-sass CLI` ， 你只需要运行如下命令：

	node-sass src/unicorn-app-theme.scss dist/unicorn-app-theme.css

然后把输出的样式文件（unicorn-app-theme.css）添加到`index.html`文件中。


主题文件不应该导入其他的 SCSS 文件。不然会在输出的 CSS 文件中写入重复的样式代码。如果你想要在其他的 SCSS 文件中使用主题定义对象（例如 $candy-app-theme ），那么在这个主题样式（SCSS文件）中需要把主题对象的定义分开，与`mat-core`和`angular-material-theme`等 mixin 独立开来。

这个主题文件可以与项目中的其他 css 文件压缩合并。

注意：如果你在 Angular 组件的`styleUrls`中引入生成的主题样式文件，这些样式文件将会作用于这个组件封装的视图中。


## 多主题

通过多次引入`angular-material-theme` mixin，你可以为你的项目创建多个主题。每一次引入都由一个额外的 CSS 类来控制。

记住，只需要引入一次`@mat-core`这个 mixin。不需要为每个主题都引入。

定义多个主题的示例代码：

	@import '~@angular/material/theming';
	// Plus imports for other components in your app.

	// Include the common styles for Angular Material. We include this here so that you only
	// have to load a single css file for Angular Material in your app.
	// **Be sure that you only ever include this mixin once!**
	@include mat-core();

	// Define the default theme (same as the example above).
	$candy-app-primary: mat-palette($mat-indigo);
	$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);
	$candy-app-theme:   mat-light-theme($candy-app-primary, $candy-app-accent);

	// Include the default theme styles.
	@include angular-material-theme($candy-app-theme);


	// Define an alternate dark theme.
	$dark-primary: mat-palette($mat-blue-grey);
	$dark-accent:  mat-palette($mat-amber, A200, A100, A400);
	$dark-warn:    mat-palette($mat-deep-orange);
	$dark-theme:   mat-dark-theme($dark-primary, $dark-accent, $dark-warn);

	// Include the alternative theme styles inside of a block with a CSS class. You can make this
	// CSS class whatever you want. In this example, any component inside of an element with
	// `.unicorn-dark-theme` will be affected by this alternate dark theme instead of the default theme.
	.unicorn-dark-theme {
	  @include angular-material-theme($dark-theme);
	}


上述例子中，父元素有`unicorn-dark-theme`这个 class 的任何组件都将使用暗色的主题，而其他组件会回退到默认的`$candy-app-theme`主题。

用这种方式，你可以引入任意数量的主题。同样，你也可以在其他单独的文件中 @include `angular-material-theme`。然后基于终端用户的交互懒加载这些样式（具体如何懒加载 CSS 资源取决于你的项目）。

然后，非常重要的是：`mat-core`这个 mixin 只需要引入一次。


## 多主题及其覆盖的组件

由于某些组件（比如菜单、下拉选择框、对话框）在全局覆盖的容器中，为了让主题的样式的 class 选择器能够影响到这些组件，还需要一个额外的步骤。

为此，你可以在全局覆盖的容器中指定`themeClass`。如下代码所示：

	import {OverlayContainer} from '@angular/material';

	@NgModule({
	  // ...
	})
	export class UnicornCandyAppModule {
	  constructor(overlayContainer: OverlayContainer) {
	    overlayContainer.themeClass = 'unicorn-dark-theme';
	  }
	}


为了改变当前的主题 class, 可以随时修改`OverlayContainer`的`themeClass`。


## 只为部分组件设置主题

`angular-material-theme` mixin 会输出这个库所有组件的样式。如果只想使用一部分组件（或者你想为指定的组件更换主题），你可以引入组件指定的主题 mixin。同时，也要需要引入`mat-core-theme`这个 mixin。它包含了为通用行为（比如波纹状扩散）指定的主题样式

	@import '~@angular/material/theming';
	// Plus imports for other components in your app.

	// 导入 Angular Material 的通用样式。在此导入通用样式，所以，你的项目中只需要加载一个单独的 Angular Material 的 css 文件。
	// **确保只导入一次这个 mixin!**
	@include mat-core();

	//定义主题
	$candy-app-primary: mat-palette($mat-indigo);
	$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);
	$candy-app-theme:   mat-light-theme($candy-app-primary, $candy-app-accent);

	// 只导入指定组件的主题样式。
	@include mat-core-theme($candy-app-theme);
	@include mat-button-theme($candy-app-theme);
	@include mat-checkbox-theme($candy-app-theme);


## 给你的组件设置主题

更多关于如何给你的组件设置主题的细节信息，请查阅[theming-your-components](https://material.angular.io/guide/theming-your-components)


## 未来的工作

	1.一旦 CSS 变量（自定义属性）在我们支持的所有浏览器上可以使用，我们将探索如何利用他们，来让设置主题变得更加简单。
	2.随着开发的继续，会增加跟多的预制主题。