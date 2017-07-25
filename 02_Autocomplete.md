
# Autocomplete 自动填充

## Overview 概述

The autocomplete is a normal text input enhanced by a panel of suggested options. You can read more about autocompletes in the Material Design spec.

自动填充(autocomplete) 是带有建议选项的常见文本输入框。 你可以在 [Material Design 标准](https://material.io/guidelines/components/text-fields.html#text-fields-auto-complete-text-field)中阅读更多关于文本输入框自动填充的内容。

### Simple autocomplete 简单的自动填充

Start by adding a regular mdInput to the page. Let's assume you're using the formControl directive from the @angular/forms module to track the value of the input.

首先，在页面中添加一个常规的`mdInput`元素 。假设你已经在使用`@angular/forms`模块的`formControl`指令来追踪输入框的内容。(Frank备注：需要提前导入 formControl 指令。 import { FormControl } from '@angular/forms';)

my-comp.html

<md-input-container>
   <input type="text" mdInput [formControl]="myControl">
</md-input-container>

Next, create the autocomplete panel and the options displayed inside it. Each option should be defined by an md-option tag. Set each option's value property to whatever you'd like the value of the text input to be upon that option's selection.

然后，创建一个自动填充面板用于展示(建议的)选项。每一个选项应该由`md-option`标签定义。基于这个文本框可能的选项，设置每一个选项的值。

my-comp.html

<md-autocomplete>
   <md-option *ngFor="let option of options" [value]="option">
      {{ option }}
   </md-option>
</md-autocomplete>

Now we'll need to link the text input to its panel. We can do this by exporting the autocomplete panel instance into a local template variable (here we called it "auto"), and binding that variable to the input's mdAutocomplete property.

现在，我们需要把文本输入框与它对应的面板关联起来。我们可以通过导出自动填充面板的实例到本地的模板变量中(我们这里给它取名为 "auto" )，然后把这个变量与输入框的`mdAutocomplete`属性绑定。

my-comp.html

<md-input-container>
   <input type="text" mdInput [formControl]="myControl" [mdAutocomplete]="auto">  // 属性绑定
</md-input-container>

<md-autocomplete #auto="mdAutocomplete">
   <md-option *ngFor="let option of options" [value]="option">
      {{ option }}
   </md-option>
</md-autocomplete>

### Adding a custom filter 添加自定义的过滤器

At this point, the autocomplete panel should be toggleable on focus and options should be selectable. But if we want our options to filter when we type, we need to add a custom filter.

此时，自动填充面板应该可以切换和选择选项了。

You can filter the options in any way you like based on the text input*. Here we will perform a simple string test on the option value to see if it matches the input value, starting from the option's first letter. We already have access to the built-in valueChanges observable on the FormControl, so we can simply map the text input's values to the suggested options by passing them through this filter. The resulting observable (filteredOptions) can be added to the template in place of the options property using the async pipe.

Below we are also priming our value change stream with null so that the options are filtered by that value on init (before there are any value changes).

*For optimal accessibility, you may want to consider adding text guidance on the page to explain filter criteria. This is especially helpful for screenreader users if you're using a non-standard filter that doesn't limit matches to the beginning of the string.

my-comp.ts

    class MyComp {
       myControl = new FormControl();
       options = [
        'One',
        'Two',
        'Three'
       ];
       filteredOptions: Observable<string[]>;

       ngOnInit() {
          this.filteredOptions = this.myControl.valueChanges
             .startWith(null)
             .map(val => val ? this.filter(val) : this.options.slice());
       }

       filter(val: string): string[] {
          return this.options.filter(option => new RegExp(`^${val}`, 'gi').test(option)); 
       }
    }

my-comp.html

<md-input-container>
   <input type="text" mdInput [formControl]="myControl" [mdAutocomplete]="auto">
</md-input-container>

<md-autocomplete #auto="mdAutocomplete">
   <md-option *ngFor="let option of filteredOptions | async" [value]="option">
      {{ option }}
   </md-option>
</md-autocomplete>

Setting separate control and display values

If you want the option's control value (what is saved in the form) to be different than the option's display value (what is displayed in the actual text field), you'll need to set the displayWith property on your autocomplete element. A common use case for this might be if you want to save your data as an object, but display just one of the option's string properties.

To make this work, create a function on your component class that maps the control value to the desired display value. Then bind it to the autocomplete's displayWith property.

<md-input-container>
   <input type="text" mdInput [formControl]="myControl" [mdAutocomplete]="auto">
</md-input-container>

<md-autocomplete #auto="mdAutocomplete" [displayWith]="displayFn">
   <md-option *ngFor="let option of filteredOptions | async" [value]="option">
      {{ option.name }}
   </md-option>
</md-autocomplete>

my-comp.ts

    class MyComp {
       myControl = new FormControl();
       options = [
         new User('Mary'),
         new User('Shelley'),
         new User('Igor')
       ];
       filteredOptions: Observable<User[]>;

       ngOnInit() { 
          this.filteredOptions = this.myControl.valueChanges
             .startWith(null)
             .map(user => user && typeof user === 'object' ? user.name : user)
             .map(name => name ? this.filter(name) : this.options.slice());
       }

       filter(name: string): User[] {
          return this.options.filter(option => new RegExp(`^${name}`, 'gi').test(option.name)); 
       }

       displayFn(user: User): string {
          return user ? user.name : user;
       }
    }


Keyboard interaction:

DOWN_ARROW: Next option becomes active.
UP_ARROW: Previous option becomes active.
ENTER: Select currently active item.