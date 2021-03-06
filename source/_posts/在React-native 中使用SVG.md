---
title: 在 React-native 中使用SVG
date: 2019-04-01 11:08:31
tags: React-native
---

在 React-Native 中使用 SVG 的一种简单而方便的方法。

<!-- more -->

在 React-native 中并不支持 SVG, 并且官方已经对此问题提出了[回复](https://github.com/react-native-community/discussions-and-proposals/issues/104),简单的说就是社区已经有了解决方案，所以官方并不会内置对 SVG 的处理，
如果想在 React-native 中使用 SVG，其实已经非常简单，分为以下几个部分。

## 准备工作

1.  安装 react-native-svg

    > npm i react-native-svg
    > 或
    > yarn add react-native-svg

2)  link 进项目

    > react-native link react-native-svg

3)  准备好自己需要的 svg 文件
4)  打开这个在[线转换工具](https://www.smooth-code.com/open-source/svgr/playground/)

我以下面这个 svg 文件举例

```xhtml
<?xml version="1.0" standalone="no"?><!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg
  class="icon"
  width="200px"
  height="200.00px"
  viewBox="0 0 1024 1024"
  version="1.1"
  xmlns="http://www.w3.org/2000/svg"
>
  <path
    fill="#ffffff"
    d="M612.685 511.995L872.61 252.139c13.48-13.454 20.906-31.351 20.906-50.394 0-19.02-7.41-36.907-20.922-50.423-13.452-13.412-31.32-20.8-50.31-20.8-18.991 0-36.858 7.388-50.359 20.85L511.95 411.278 251.944 151.321c-13.453-13.412-31.32-20.8-50.31-20.8-18.993 0-36.86 7.389-50.312 20.802l-0.146 0.147c-27.605 27.75-27.586 72.87 0.099 100.636l259.94 259.883-260 259.93c-27.63 27.715-27.644 72.837-0.032 100.583l0.172 0.171c13.26 13.2 31.585 20.77 50.278 20.77 18.968 0 36.824-7.375 50.28-20.77l260.038-259.97 259.974 259.916c13.455 13.452 31.34 20.86 50.36 20.86 19.014 0 36.898-7.406 50.323-20.826 13.483-13.452 20.908-31.349 20.909-50.39 0-19.042-7.424-36.94-20.873-50.36l-259.96-259.908z"
  />
</svg>
```

打开[线转换工具](https://www.smooth-code.com/open-source/svgr/playground/)之后，左侧选项栏里面，勾选 `React Native`

之后将自己 svg 文件内容贴在左侧，右侧会自动生成可以在 ReactNative 中运行的代码。

我将上面的代码转换之后得到了

```jsx
import React from "react";
import Svg, { Path } from "react-native-svg";

const SvgComponent = props => (
  <Svg
    className="prefix__icon"
    width={200}
    height={200}
    viewBox="0 0 1024 1024"
    {...props}
  >
    <Path
      fill="#fff"
      d="M612.685 511.995L872.61 252.139c13.48-13.454 20.906-31.351 20.906-50.394 0-19.02-7.41-36.907-20.922-50.423-13.452-13.412-31.32-20.8-50.31-20.8-18.991 0-36.858 7.388-50.359 20.85L511.95 411.278 251.944 151.321c-13.453-13.412-31.32-20.8-50.31-20.8-18.993 0-36.86 7.389-50.312 20.802l-.146.147c-27.605 27.75-27.586 72.87.099 100.636l259.94 259.883-260 259.93c-27.63 27.715-27.644 72.837-.032 100.583l.172.171c13.26 13.2 31.585 20.77 50.278 20.77 18.968 0 36.824-7.375 50.28-20.77l260.038-259.97 259.974 259.916c13.455 13.452 31.34 20.86 50.36 20.86 19.014 0 36.898-7.406 50.323-20.826 13.483-13.452 20.908-31.349 20.909-50.39 0-19.042-7.424-36.94-20.873-50.36l-259.96-259.908z"
    />
  </Svg>
);

export default SvgComponent;
```

得到 js 代码之后就可以在项目中新建一个 js 文件将转换后的代码写在文件内就可以当做普通的模块调用了,可以删除或者设定一个默认的宽高，删掉 className，通过设定 fill=currentColor,来让 svg 继承 color 的值。

对于有非常多的 SVG 文件来说，比如一次性有好几十个，那么一个一个的转换，实在太麻烦了，这个时候就可以通过脚本来实现

通过 `@svgr/cli` 提供的命令行工具来批量转入，这里需要几个关键的命令行参数

```
npx @svgr/cli --native -d 输出目录 输入目录

```

例如

> npx @svgr/cli --native -d ./src/svg ./src/icon

这样就可以调用命令行程序进行批量转换

关于更多的配置可以参考[文档](https://www.smooth-code.com/open-source/svgr/docs/getting-started/)
