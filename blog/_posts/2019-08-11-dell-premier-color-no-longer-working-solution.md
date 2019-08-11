---
title: 解决 Windows 10 更新后 Dell PremierColor 不生效的问题
layout: post
comments: true
---

* toc
{:toc}

## 问题描述

更新 Windows 10 后 Dell PremierColor 可以正常运行，但对显示器的**色彩调整不生效**，即使更新显卡驱动或重新安装软件后依然无法解决。

## 解决方法

1. 进入 `控制面板` 并点击 `颜色管理` 。

2. 点击 `所有配置文件` 并在 `ICC 配置文件` 下找到所有与“Dell PremierColor”相关的配置文件，这些配置文件是连续排列的，**有的可能没有名称**。

![](https://gitee.com/h00kran/blog-assets/raw/master/img/colors_management_screenshot.png)

3. 耐心地将找到的配置文件逐个删除。具体方法是点击配置文件，按下 `Alt + R` 组合键 ，再按下 `Enter` 键。（注意，**请不要误删除其他色彩配置文件**，此操作是无法撤销的）

4. 确保显卡驱动已经更新到了最新版本，并且确保显卡驱动是从 [戴尔官网](https://www.dell.com/support/home/) 下载安装的，而不是从英特尔或英伟达等制造商的网站下载的。如果你不确定，那么可以从戴尔官网上下载最新的显卡驱动，然后执行“清洁安装”。

5. 从 Microsoft Store（或点击 [此链接](https://www.microsoft.com/store/productId/9N9PJGJG009K) ）上搜索并安装 Dell PremierColor ，打开软件并进行简单的配置后即可正常使用。