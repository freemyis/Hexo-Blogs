---
title: Tailwind CSS学习笔记
date: 2022-02-22 10:31:19
tags:
---
> ## Tailwind CSS浅析
### Tailwind CSS是什么
```
Tailwind Css是一个css框架，和bootstrap、elementui一样，可以加速前端开发的工具
```
### Tailwind CSS和其他css框架的区别
#### 涉及到css发展的阶段性：
```
1. 原生写法，直接写到html dom style上或者写在<style></style>
优点：想咋写咋写，自主性强
缺点：样式没有一个规范，px等数值设置杂乱无章
2. css组件化写法，将UI封装成一个公共组件，样式一致，可复用
优点：提高了组件复用性，需要什么直接拿来用，减少了css和代码，提高开发效率
缺点：通用性太强，无法适用一些特别的、个性化的定制性需求实现，修改需要覆盖组件原有css样式，写法很脏，极不友好
3. css原子化，手动封装常用单个css样式
优点：可以复用css样式(class)，提高开发效率
缺点：需要自己琢磨class命名，容易混乱和命名冲突等

区别：
bootstrap已经封装好了卡片等组件，Tailwind CSS并未做这件事，仍旧是原子化的，如果需要我们可以基于Tailwind CSS写一套属于自己的定制化框架，定制性极强。
Tailwind CSS优缺点：
优点
1. 基本可以不用再写css,全部用class实现，避免出现不友好的写法。
2. 避免了上面提到的命名苦恼的问题，所有class都是语义化定义好的。
3. 响应式设计，遵循移动优先的设计模式(md: lg:)
4. 默认提供一套专业的色值
5. 可以写鼠标hover等样式实现(:hover，:focus, :active)
6. 可扩展性强，样式重构方便，可以在tailwind.config.js里按照tailwind命名规范定制化自己的通用class实现
7. @base样式？
8. vscode有支持智能提示的插件
9. 支持Dark模式的伪类(dark:...)
```
[Tailwind CSS深色模式设置](https://www.tailwindcss.cn/docs/dark-mode)
```
缺点
1. class类型过长(解决方案：提取组件？ 使用apply创建抽象css类)
2. 熟悉过程需要成本
```
### 疑问
#### tailwind的使用场景，所有项目都要用tailwind嘛
```
Tailwind CSS 更适合于页面定制化高的场景,一些快速简单交付的项目没必要用，用通用框架开发更快，看定制化情况使用
```
#### tailwind封装了这么多class会不会打包很大(3M多)
```
Tailwind CSS 针对这个问题添加了purge,production的时候清除未使用的class样式
```

#### 后续
[windcss](https://windicss.org/guide/)
[重新构想原子化css](https://antfu.me/posts/reimagine-atomic-css-zh)