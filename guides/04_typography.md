英文链接地址：https://material.angular.io/guide/typography

# Angular Material typography

# Angular Material 排版

## 1.What is typography?

## 1.什么是排版？

Typography is a way of arranging type to make text legible, readable, and appealing when displayed. Angular Material's typography is based on the guidelines from the Material Design spec and is arranged into typography levels. Each level has a font-size, line-height and font-weight. The available levels are:

	display-4, display-3, display-2 and display-1 - Large, one-off headers, usually at the top of the page (e.g. a hero header).
	headline - Section heading corresponding to the <h1> tag.
	title - Section heading corresponding to the <h2> tag.
	subheading-2 - Section heading corresponding to the <h3> tag.
	subheading-1 - Section heading corresponding to the <h4> tag.
	body-1 - Base body text.
	body-2 - Bolder body text.
	caption - Smaller body and hint text.
	button - Buttons and anchors.


排版是一种是文字排列方式。它使得文本在展示时明白易懂、有吸引力。`Angular Material` 的排版是基于`Material Design`的设计标准指南的。它被分为几个排版层级，每个层级指定了字体大小、行高、字体粗细。可用的等级包括以下几个：

	display-4, display-3, display-2 and display-1 - 大的，一次性的页头，通常用于页面顶部
	headline - 区块的标题， 相当于 <h1> 标签
	title - 区块的标题， 相当于 <h2> 标签
	subheading-2 - 区块的标题， 相当于 <h3> 标签
	subheading-1 - 区块的标题， 相当于 <h4> 标签
	body-1 - 基本的主要文本
	body-2 - 加粗的主要文本
	caption - 稍小的主要文本和提示文本
	button - 按钮和锚链接

The typography levels are collected into a typography config which is used to generate the CSS.

排版层级聚集在排版配置中，这个配置用于生成 CSS。


## 2.Usage

## 2.用法

To get started, you first include the Roboto font with the 300, 400 and 500 weights. You can host it yourself or include it from Google Fonts:

首先，你需要引入包含300, 400 and 500 粗的 Roboto 字体。你可以自己存储或者从[谷歌字体](https://fonts.google.com/)中引入。

	<link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500" rel="stylesheet">

Now you can add the appropriate CSS classes to the elements that you want to style:

现在，你可以给元素添加合适的 CSS 类名：

	<h1 class="mat-display-1">Jackdaws love my big sphinx of quartz.</h1>
	<h2 class="mat-h2">The quick brown fox jumps over the lazy dog.</h2>


By default, Angular Material doesn't apply any global CSS. To apply the library's typographic styles more broadly, you can take advantage of the mat-typography CSS class. This class will style all descendant native elements.

默认情况下，`Angular Material`并不使用任何的全局样式。为了可以更广泛地使用这个类库的字体样式，你可以用`mat-typography`这个 CSS 类名。这个类名将给它的所有后代原生的元素添加样式。

	<!-- By default, Angular Material applies no global styles to native elements. -->
	<h1>This header is unstyled</h1>

	<!-- Applying the mat-tyography class adds styles for native elements. -->
	<section class="mat-typography">
	  <h1>This header will be styled</h1>
	</section>


## 3.Customization

## 3.自定义

Typography customization is an extension of Angular Material's SASS-based theming. Similar to creating a custom theme, you can create a custom typography configuration.

自定义排版是`Angular Material`基于 SASS 主题的一种拓展。类似于创建自定义主题，你可以创建自定义的排版配置。

	@import '~@angular/material/theming';

	// Define a custom typography config that overrides the font-family as well as the
	// `headlines` and `body-1` levels.
	// 定义一个自定义的排版配置项，重写字体及`headlines` and `body-1`
	$custom-typography: mat-typography-config(
	  $font-family: monospace,
	  $headline: mat-typography-level(32px, 48px, 700),
	  $body-1: mat-typography-level(16px, 24px, 500)
	);


As the above example demonstrates, a typography configuration is created by using the mat-typography-config function, which is given both the font-family and the set of typographic levels described earlier. Each typographic level is defined by the mat-typography-level function, which requires a font-size, line-height, and font-weight.

如上述例子所示，通过使用`mat-typography-config`函数创建的自定义的排版配置，它包含字体和之前提到的排版层级（headline、body-1）。每一个排版层级由`mat-typography-level`函数定义。这个函数需要 font-size, line-height, and font-weight 等3个参数。

Once the custom typography definition is created, it can be consumed to generate styles via different SASS mixins.

自定义排版一旦创建，他就能通过不同的 SASS mixin 来生成样式。

	// Override typography CSS classes (e.g., mat-h1, mat-display-1, mat-typography, etc.).
	@include mat-base-typography($custom-typography);

	// Override typography for a specific Angular Material components.
	@include mat-checkbox-typography($custom-typography);

	// Override typography for all Angular Material, including mat-base-typography and all components.
	@include angular-material-typography($custom-typography);

For more details about the typography functions and default config, see the source.

## 4.Material typography in your custom CSS

Angular Material includes typography utility mixins and functions that you can use to customize your own components:

	mat-font-size($config, $level) - Gets the font-size, based on the provided config and level.
	mat-font-family($config) - Gets the font-family, based on the provided config.
	mat-line-height($config, $level) - Gets the line-height, based on the provided config and level.
	mat-font-weight($config, $level) - Gets the font-weight, based on the provided config and level.
	mat-typography-level-to-styles($config, $level) - Mixin that takes in a configuration object and a typography level, and outputs a short-hand CSS font declaration.


	@import '~@angular/material/theming';

	// Create a config with the default typography levels.
	$config: mat-typography-config();

	// Custom header that uses only the Material `font-size` and `font-family`.
	.unicorn-header {
	  font-size: mat-font-size($config, headline);
	  font-family: mat-font-family($config);
	}

	// Custom title that uses all of the typography styles from the `title` level.
	.unicorn-title {
	  @include mat-typography-level-to-styles($config, title);
	}