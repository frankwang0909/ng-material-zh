
# 给自定义组件添加主题

为了能够用`Angular Material`的工具来为你的组件提供样式，组件的样式文件必须用 Sass 来定义。

## 1.使用 @mixin 自动地应用主题

### 1.1 为什么使用 @mixin

使用 @mixin 函数的好处是：当你要改变主题时，每个使用它的文件都能自动地更新。通过传入不同的主题参数来调用这个函数，使得项目或组件中，可以存在多个主题。

### 1.2 如何使用 @mixin

在主题文件中增加  @mixin 函数，然后调用这个函数来使用一个主题。这样，我们可以更好地为自定义组件设置主题。

你只需要在`custom-component-theme.scss`中创建一个  @mixin 函数

	// Import all the tools needed to customize the theme and extract parts of it
	// 导入自定义这个主题所需的工具
	@import '~@angular/material/theming';

	// Define a mixin that accepts a theme and outputs the color styles for the component.
	// 定义一个 mixin ，接受一个主题参数，返回组件的颜色样式。
	@mixin candy-carousel-theme($theme) {
	  // Extract whichever individual palettes you need from the theme.
	  // 从这个主题中抽取出你需要的任何色调
	  $primary: map-get($theme, primary);
	  $accent: map-get($theme, accent);

	  // Use mat-color to extract individual colors from a palette as necessary.
	  // 使用 mat-color 从色调中抽取任意的颜色。
	  .candy-carousel {
	    background-color: mat-color($primary);
	    border-color: mat-color($accent, A400);
	  }
	}

现在，你只需要调用这个 @mixin 函数就可以使用这个主题了：

	// 导入一个预制的主题
	@import '~@angular/material/prebuilt-themes/deeppurple-amber.css';

	// 导入你的自定义的主题文件，以便可以调用  custom-input-theme 函数
	@import 'app/candy-carousel/candy-carousel-theme.scss';

	// 使用预制主题的变量 $theme， 你可以调用这个主题函数
	@include candy-carousel-theme($theme);


更多关于主题函数的细节信息，请查看[源代码的注释](https://github.com/angular/material2/blob/master/src/lib/core/theming/_theming.scss)。

### 1.3 使用 @mixin 的最佳实践

使用 @mixin时， 主题文件应该只包含受传入的主题参数影响的定义。
 
所有不受主题影响的样式应该放在 candy-carousel.scss 文件中。这个文件应该包含一切不受主题影响的样式，比如尺寸，渐变等。

受主题影响的样式应该放在单独的主题文件 _candy-carousel-theme.scss 中。这文件名必须是下划线 _ 开头的。这个文件应该包含  @mixin 函数，它负责将主题运用到组件中。


## 2.使用色调中的颜色

你可以来使用`@angular/material/theming`提供的主题函数和`Material`色调变量。你可以使用`mat-color`函数来从色调中抽取指定的颜色。例如：

	// 导入主题函数
	@import '~@angular/material/theming';

	// 导入自定义主题
	@import 'src/unicorn-app-theme.scss';

	// 使用 mat-color 从色调中抽取单个的颜色
	.candy-carousel {
	  background-color: mat-color($candy-app-primary);
	  border-color: mat-color($candy-app-accent, A400);
	}

