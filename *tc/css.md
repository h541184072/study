---
css基础
---

1.盒模型

```text
1. W3C 标准盒模型：
属性width,height只包含内容content，不包含border和padding。
2. IE 盒模型：
属性width,height包含border和padding，指的是content+padding+border。
```

2.移动端适配方案 (#)

-   [《关于移动端适配，你必须要知道的》](https://juejin.im/post/5cddf289f265da038f77696c)

3.写出 3 种水平垂直居中方案

```text
flex
grid
absolute transform
```

4.link 与 @import 的区别

```text
  link功能较多，可以定义 RSS，定义 Rel 等作用，而@import只能用于加载 css
  当解析到link时，页面会同步加载所引的 css，而@import所引用的 css 会等到页面加载完才被加载
  @import需要 IE5 以上才能使用
  link可以使用 js 动态引入，@import不行
```

7.link 预加载 预请求

```text
preload 是通知浏览器，页面必定需要的资源，但浏览器一定会加载这些资源
prefetch 是通知浏览器，页面可能需要的资源，但浏览器不一定会加载这些资源
```

8.script defer 与 async

```text
defer: 渲染完再执行, 加载完成后需要等待页面也加载完成才会执行代码
async: 下载完就执行, 加载完成后会自动执行脚本
```

9.SSR 和 PreRender

-   [《CSR、SSR、Prerender 原理全解密》](https://www.shangmayuan.com/a/43b9749957e0484eb5242333.html)

```text
PreRender 就是利用 Chrome 官方出品的 Puppeteer 工具，对页面进行爬取。    prerender-spa-plugin

能够看出，SSR 和 PreRender 的最大区别就在于，PreRender 是静态的，SSR 是动态的，SSR 会在服务端实时构建出对应的 DOM。
```

10.transition 的局限

```text
transition的优点在于简单易用，但是它有几个很大的局限。

（1）transition需要事件触发，所以没法在网页加载时自动发生。

（2）transition是一次性的，不能重复发生，除非一再触发。

（3）transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。

（4）一条transition规则，只能定义一个属性的变化，不能涉及多个属性。

CSS Animation就是为了解决这些问题而提出的。
    keyframes
```
