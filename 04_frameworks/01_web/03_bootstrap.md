[TOC]



# 工具

- visual studio code  + plugin: bootstrap-snippets
- npm install -g live-server

# Grid System



## working of grid system

- row必须放置在.container类下以便填充和对齐
- 使用rows来创建垂直的列
- 内容必须放在列中，只有列才能成为rows的直接子节点
- 预先声明的.row和.col-xs-4可以用来快速创建grid布局。
- 列通过填充(padding)获取沟槽

## media queries

# CSS Overview

## html5 doctype

```html
<!DOCTYPE html>
<html>
  ...
</html>
```

## Mobile First

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

## Responsive Images

```html
<img src="..." class="img-responsive" alt="responsive image">
```

## typography and links

- basic global display: body上有设置background-color: #fff
- typography: @font-family-base, @font-size-base, @line-height-base
- link styles

## normalize

 ## container

使用.container包裹页面内容可以使内容居中

# Typography

bootstrap应用了helvetica neue, helvetica, arial, sans-serif作为默认字体。使用排版可以创建headings标题，段落，列表及其它inline元素

## inline subheadings

```html
<h1>I'm heading1 h1<small>I'm the secondary heading1 h2</small></h1>
```

## lead body copy

为了给段落加上重点，可以添加lead类，这会使字号更大， 行高更高

```html
<h2>lead example</h2>
<p class="lead">
    This is an example paragraph demonstrating 
   the use of lead body copy. This is an example paragraph 
   demonstrating the use of lead body copy.This is an example 
   paragraph demonstrating the use of lead body copy.This is an 
   example paragraph demonstrating the use of lead body copy.
   This is an example paragraph demonstrating the use of lead body copy.
</p>
<p>
    This is an example paragraph demonstrating 
   the use of lead body copy. This is an example paragraph 
   demonstrating the use of lead body copy.This is an example 
   paragraph demonstrating the use of lead body copy.This is an 
   example paragraph demonstrating the use of lead body copy.
   This is an example paragraph demonstrating the use of lead body copy.
</p>

```

### emphasis

emphasis会用父元素的85%的字号，strong使用更大的字体作为着重。em使用斜体作为着重。

```