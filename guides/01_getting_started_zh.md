
英文原文地址：https://material.angular.io/guide/getting-started

# Getting started

关于如何开始创建一个新的 Angular 项目，请查阅 [the Angular CLI](https://cli.angular.io/)。

对于已经创建的 Angular 项目，遵循以下步骤来开始使用`Angular Material`。


## 步骤一：安装 Angular Material 和 Angular CDK

    npm install --save @angular/material @angular/cdk


包含最新变化的快照版本也是可用的。但是注意这个快照版不稳定而且可能停止使用。以下命令可以安装快照版本的 Angular Material 和 Angular CDK：

    npm install --save angular/material2-builds angular/cdk-builds


## 步骤二：动画

有些`Material`组件依赖于 Angular 的`animations`模块来实现更多高级的过渡变化。如果你想在你的应用中使用这些动画，你需要先安装`@angular/animations`模块，并且把`BrowserAnimationsModule`包含在你的应用中。

以下命令安装`@angular/animations`模块：

    npm install --save @angular/animations

以下示例代码，把`BrowserAnimationsModule`包含在你的应用中。

    import {BrowserAnimationsModule} from '@angular/platform-browser/animations'; 

    @NgModule({
      ...
      imports: [BrowserAnimationsModule],
      ...
    })
    export class PizzaPartyAppModule { }


如果你不想给你的项目增加其他的依赖，你可以使用`NoopAnimationsModule`模块。

    import {NoopAnimationsModule} from '@angular/platform-browser/animations';

    @NgModule({
      ...
      imports: [NoopAnimationsModule],
      ...
    })
    export class PizzaPartyAppModule { }


## 步骤三: 往组件中导入模块

给你需要使用的每个组件导入模块(NgModule)：

    import {MdButtonModule, MdCheckboxModule} from '@angular/material';

    @NgModule({
      ...
      imports: [MdButtonModule, MdCheckboxModule],
      ...
    })
    export class PizzaPartyAppModule { }

或者，你可以创建一个单独的模块(NgModule)，用来导入你想在你的应用中使用到的所有`Angular Material`组件, 然后，你可以在任何需要使用这些组件的地方导入这个模块。

    import {MdButtonModule, MdCheckboxModule} from '@angular/material';

    @NgModule({
      imports: [MdButtonModule, MdCheckboxModule],
      exports: [MdButtonModule, MdCheckboxModule],
    })
    export class MyOwnCustomMaterialModule { }


无论你使用哪种方式，都需要确保在导入 Angular 的`BrowserModule`模块后，再导入`Angular Material`的模块。因为对于 Ng模块(NgModules)来说，导入的顺序很重要。


## 步骤四：把一套主题包含进来

在你的应用中使用所有的核心和主题样式，必须把一套主题包含进来。

你可以在应用中包含一个全局的`Angular Material`的预制主题。

如果你使用`Angular CLI`, 你可以在`styles.css`文件中添加一套主题：

    @import "~@angular/material/prebuilt-themes/indigo-pink.css";


如果你没有使用`Angular CLI`， 你可以在你的`index.html`文件中通过`<link>`标签来引入一套预制的主题。

更多关于主题的内容和如何创建自定义主题的指南，请查阅[theming guide](https://material.angular.io/guide/theming)。



## 步骤五：手势支持

有些组件（比如 md-slide-toggle, md-slider, mdTooltip）的手势依赖于`HammerJS`。为入了使用到这些组件的完整的特性，需要先加载`HammerJS`。

你可以通过`npm`、CDN（例如[谷歌的 CDN](https://developers.google.com/speed/libraries/#hammerjs) ) 或先下载到本地项目的某个目录下再直接引用等方式添加`HammerJS`。


使用如下命令，可以通过`npm`来安装`HammerJS`：

    npm install --save hammerjs

安装之后，导入到你的应用根模块（默认是app.module.ts）中。

    import 'hammerjs';

## 步骤六（可选）：添加 `Material Icons`

如果你想要使用`md-icon`组件的 [Material Design Icons](https://material.io/icons/) 官方图标, 在`index.html`文件中添加图标字体文件：

    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

更多关于使用`Material Icons`的信息，请查阅[Material Icons Guide](https://google.github.io/material-design-icons/)。

注意：`md-icon`支持任意的字体或 SVG 图标，使用`Material Icons`是众多方式中的一种。


## 附录：配置 SystemJS

如果你的项目使用`SystemJS`来加载模块，你需要把`@angular/material`添加到`SystemJS`的配置中：


  System.config({
    // existing configuration options
    map: {
      // ...
      '@angular/material': 'npm:@angular/material/bundles/material.umd.js',
      // ...
    }
  });


## Angular Material 示例项目：

material.angular.io - Angular Material 项目的文档本身就是用 Angular Material 来构建的