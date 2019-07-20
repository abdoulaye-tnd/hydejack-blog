---
layout: post
title: 如何在静态网站上优雅地安装站内搜索引擎
comments: true
---

* toc
{:toc}

## 前言

如果你碰巧有在静态网站上安装搜索引擎的需求，并且不打算使用付费服务，也不想折腾；那么，这篇文章**正是你所需要的**。

如果你不想看长篇大论的“废话”，那么你可以**直接转跳到 [安装过程](#安装过程) 这一章节**。

我写这篇文章的原因，是因为我为这个问题已经苦恼了很久，并且尝试过许多博客所用的方法。但是由于这个网站使用的 Jekyll 改版主题并不支持大多数方法，再加上该网站托管在 Netlify 的特殊性，这些方法往往都不能完美地解决该网站对于搜索的需求。

## Trial and error and error and error...

为了解决这个问题，我尝试过以下的方案，并且都因为各种原因失败了：

1. 使用各种各样的 JavaScript 脚本：因为无法正确运作而失败，即使在我排查了所有障碍之后。
2. 以百度为首的各种**免费**站内搜索服务：这些服务往往常年不维护，已经不能正常使用了。（例如无法使用 HTTPS ）
3. 以 Elastic Search 为首的各种**仅有付费方案**的专业站内搜索服务：用不起，而且我原以为有免费套餐可以用，其实这些服务商都早已取消免费套餐了。
4. Cloudflare 傻瓜式应用部署：虽然最终因为效果不好而不采用，但是正是这里启发了我。
5. 搜索引擎链接转跳搜索：悲催的是，没有一个搜索引擎收录我的网站……

这些方案前前后后断续地浪费了我**将近两个星期**的时间，我用尽了网络上所有文章所说的方法，**没有一个奏效**。

## 安装过程

最后，我采用了 [Site Search 360°](https://www.sitesearch360.com) 的**免费**站内搜索订阅产品，完美地在这个网站上安装了搜索引擎。

### 准备工作

1. 在 [Site Search 360° (https://www.sitesearch360.com)](https://www.sitesearch360.com) 上 [注册](https://control.sitesearch360.com/signup) 一个账号，添加你的网站。

2. 在 [控制台](https://control.sitesearch360.com) 上进行一些必要的配置，例如 Sitemap 抓取配置、标题抓取配置等。其中需要注意的是，默认是不采用 Sitemap 抓取的，并且默认的标题抓取规则是抓取 `h1` 并非 `title` 标签，这些**都是坑**。

![](https://gitee.com/h00kran/blog-assets/raw/master/img/title_xpath_screenshot.png){:.lead}

3. [检查抓取的数据](https://control.sitesearch360.com/#section=indexControl) 无误后，即完成了准备工作。

### 部署代码

一般来说，只要在“你希望搜索框出现的位置”放置 [Site Search 360° 设计页面](https://www.sitesearch360.com/search-designer) 生成的代码即可。但是，由于受该 Jekyll 主题的限制，我**无法**随心所欲地放置这些代码。因此，我是这样解决的：

1. 找到一个将会在 `body` 末端注入文件内容的文件（在我的网站中，这个文件叫 `my-body.html` ，而且只有这文件），插入搜索框和脚本的代码：

```html
<input ... />
<script>
    ...
</script>
<script>
    ...
</script>
<script>
    ...
</script>
```

2. 找到一个存放自定义外观样式代码的文件（在我的网站中，这个文件叫 `my-style.scss` ，而且**也是**只有这文件），通过定义 `position` 为 `absolute` 的方式来将搜索框“放到”它应该出现的地方：

```css
input#searchBox {
    position: absolute;
    right: 20pt;
    top: 20pt;
}
```

### 美化

如果不是为了美化搜索框的话，我可能会直接用 Cloudflare 应用来部署了。但是，Cloudflare 注入的东西都差强人意，而且自定义样式后会有各种各样的小问题。正因为如此，我才会选择这篇文章所说的方案。废话不多说，看看有什么地方是可以美化的：

1. 在 [Site Search 360° 设计页面](https://www.sitesearch360.com/search-designer) 上自定义。优点是操作简单，缺点是很多“高级设置”无法更改。

2. 通过上一步获得代码后，对代码进行修改。在这个网站上，我只修改了圆角属性（而且**比较坑的是**，圆角属性用 CSS 修改的话会被覆写）：

```html
<script>
    var ss360Config = {
        style: {
            ...
            searchBox: {
                ...
                border: {
                    ...
                    radius: "30px"
}, ... }, ... }, ...}
</script>
```

3. 此外，我们当然可以用 CSS 定义外观（这里又有个**可能存在的坑**，就是 CDN 的一些操作可能会导致样式失效；例如下方演示的去除外框，必须要这样写才生效）：

```css
input:focus {
    outline: none;
}

.form-control:focus {
    border-color: inherit;
    -webkit-box-shadow: none;
    box-shadow: none;
}
```

## 结语

我自认为这个方法是**最简单、最完美且适用性最好的**，希望我的文章可以帮助到你！

顺便说一件有趣的事：我发现那些写过关于“静态网站部署搜索”的文章的博客往往都不采用他们的文章中所说的方法，而使用第三方的付费搜索服务，或者对文章中所述的方法进行魔改后进行搭建。所以我才会写这篇文章，尽量把我遇到的情况、细节和坑都讲清楚（虽然我在建站方面不算精通，也只是个新手而已）。

如有谬误之处欢迎通过 [电子邮件 (hoganlee_dev@outlook.com)](mailto:hoganlee_dev@outlook.com?subject=[Feedback@foresite.top]%20请简要描述问题) 向我吐槽，感谢你的耐心阅读！
