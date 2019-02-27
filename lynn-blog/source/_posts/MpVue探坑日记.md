---
title: MpVue探坑日记
date: 2018-08-15 22:05:22
category:
  - Coding
tags:
  - Dev
  - MpVue
  - 微信小程序
---

# MpVue 踩坑日记

近日将原有的原生小程序迁移到 Vue 技术栈中，遂采用[mpvue](http://mpvue.com/mpvue/)进行代码迁移和重构。建议将官方文档看一遍之后能避开大部分坑。

# img 标签

img 标签的 src 属性无法指向相对路径，需要用绝对路径代替。详见：[GitHub](http://t.cn/RDuBSgR)

# tabbar

`tabbar`中的图标设置，需要将图片文件放在`/static`中才可以正常引用。详见：[GitHub](https://github.com/Meituan-Dianping/mpvue/issues/94)

# wx:指令转 v-指令

## v-for

微信小程序的`wx:for`不需要指定 index，转到`v-for`之后需要指定 index。

## dataset

微信小程序的事件方法不能传参数，所以只能绑定在页面上，但是切换到 vue 就支持传参，因此可以去掉大部分的`data:XXX`绑定的语句。

## bindtap 等事件

事件的转换参考官方文档，大部分的移动端事件可以支持。

# 标签

目前 view 标签迁移到 div 标签没有发现问题。

# setData()

小程序中 setData 有两种应用场景，一种是赋值，一种是把自己的值重新赋给自己用于更新页面。在 Vue 中由于采用了双向绑定，因此直接采用`this.foo=bar`即可

# :key

开发过程中遇到以下场景：通过 v-for 生成一系列自定义的组件，组件内部的 onload 方法里有一个语句会改变组件内部的值，现象是这个组件无限调用 onload 方法，导致调试器崩溃。  
经过排查，原来的 v-for 语句后边绑定的`:key="Math.random(index)"`会出现问题，改为`:key="index"`问题解决。
使用 Math.random()绑定 key 值的原因在于，之前在练习 Vue 的时候，做了一个可添加删除的 TodoList，单纯地使用 inde 作为 key 值，在删除的时候出现了一个 bug，选中了 checkbox 的那一个条目删除之后，第二个条目的 index 值变成了前一个条目的 index 值，此时 vue 会认为这个 idnex 项可以复用，checkbox 的状态就被保留了，从此只使用两种方法进行 key 值的绑定：

```javascript
:key="Date.now()+index"
:key="Math.random(index)"
// 窃以为Vue的一大败笔就是默认复用，反而造成了某些项目的资源浪费
// 核心方法UpdateChild()在以后的博客中会有涉及
```

具体为什么 Math.random()绑定 key 值造成无限刷新，还没有结果。

# 页面后退之后不销毁，数据缓存

详见：[#140](https://github.com/Meituan-Dianping/mpvue/issues/140)  
解决方案：页面中添加生命周期

```javascript
onUnload(){
  Object.assign(this.$data, this.$options.data())
}
// 不推荐全局注册，因为有些页面其实是需要缓存的，只有在某些特定页面比如包含input标签
```

# Vue 文件内的 scoped style 标签

发现一个问题：  
现象：在 vue 组件 ​​ 内部，带有 scoped 属性的 style 标签里，写 scss 嵌套样式，会出现第二层以上的样式失效。 ​
成因：vue 的 scoped 有如 ​​ 下规则：

> 1.  给 HTML 的 DOM 节点加一个不重复 data 属性(形如：data-v-2311c06a)来表示他的唯一性
>
> 2.  在每句 css 选择器的末尾（编译后的生成的 css 语句）加一个当前组件的 data 属性选择器（如[data-v-2311c06a]）来私有化样式
>
> 3.  如果组件内部包含有其他组件，只会给其他组件的最外层标签加上当前组件的 data 属性

以上会导致嵌套的选择器附加的 data 属性选择器会错误
网上找到的类似的情况有：
<https://blog.csdn.net/juse__we/article/details/80419617>  
<https://www.jianshu.com/p/f9a8b7784655>  
<https://github.com/vuejs/vue/issues/7067>  
解决办法：养成好习惯，大段的嵌套样式在外部文件中，组件内 scoped 样式只写简单的非嵌套的样式

# MpVue 中使用 SCSS

使用官方的脚手架之后需要自行安装 node-sass 和
sass-loader

<br />
<br />
<br />

<small>
更新时间：2018 年 08 月 15 日 22:26:32
</small>
