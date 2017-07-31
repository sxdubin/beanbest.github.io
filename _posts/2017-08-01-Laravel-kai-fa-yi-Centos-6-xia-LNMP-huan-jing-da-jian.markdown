---
layout: post
title: Laravel开发一（Centos6下LNMP环境搭建） 
date: 2017-07-31 12:32:24.000000000 +08:00
categories: [Laravel, Linux]
---

> 工欲善其事，必先利其器，想要在Laravel下开发必先有一套可以运行Laravel的环境.

互联网上现在有许多关于安装LNMP环境的教程，为什么我要重复造轮子来写这篇文章呢?

原因有四:

* `Laravel 5.5` 即将更新 默认PHP环境最低需要 `PHP 7.0` ，升级到PHP 7.0 方便以后更新laravel版本。

* 这边CMDB子业务需要和主系统要做分离，刚好提供了契机。

* 需要针对专门的业务属性，做专门的配置优化。

* 留存文档，供后面的同学或有相同需求的同学参考使用。

#### 安装版本一览

* nginx 1.12.*
* php 7.2
* mysql 5.7 
* composer  3.* 
* redis 2.1

#### Usage

```bash
$ git clone https://github.com/onevcat/vno-jekyll.git your_site
$ cd your_site
$ bundler install
$ bundler exec jekyll serve
```

Your site with `Vno Jekyll` enabled should be accessible in http://127.0.0.1:4000.

For more information about Jekyll, please visit [Jekyll's site](http://jekyllrb.com).

#### Configuration

All configuration could be done in `_config.yml`. Remember you need to restart to serve the page when after changing the config file. Everything in the config file should be self-explanatory.

#### Background image and avatar

You could replace the background and avatar image in `assets/images` folder to change them.

#### Sites using Vno

[My blog](http://onevcat.com) is using `Vno Jekyll` as well, you could see how it works in real. There are some other sites using the same theme. You can find them below:

| Site Name    | URL                                                |
| ------------ | ---------------------------------------------------|
| OneV's Den   | [http://onevcat.com](http://onevcat.com)           |
| July Tang    | [http://blog.julytang.xyz](http://onevcat.com)     |

> If you happen to be using this theme, welcome to [send me a pull request](https://github.com/onevcat/vno-jekyll/pulls) to add your site link here. :)

#### License

Great thanks to [Dale Anthony](https://github.com/daleanthony) and his [Uno](https://github.com/daleanthony/uno). Vno Jekyll is based on Uno, and contains a lot of modification on page layout, animation, font and some more things I can not remember. Vno Jekyll is followed with Uno and be licensed as [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/). See the link for more information.


