# 逆向调试利器：Frida

* 最新版本：`v0.7`
* 更新时间：`20230616`

## 简介

介绍支持Android、iOS等多个平台的通用逆向工具：Frida。先是Frida概览，包括Frida的代码和架构，以及相关文档和案例和资料，包括codeshare。再介绍如何在PC端和移动端安装和升级Frida。然后是如何使用Frida，包括通用的逻辑，比如调试目标的方式，以及写js脚本；然后是Frida的细分模块，包括iOS的ObjC、frida的命令行、frida-trace、frida-tools工具集合、frida-server、frida的Stalker、js。接着整理一些Frida开发期间的经验和心得，包括console.log打印日志、常见的一些问题，包括各种报错、导致iPhone重启、导致app崩溃等等。然后去整理一些用到了Frida的工具；接着整理Frida的一些具体的典型的用途，包括反调试、辅助反混淆、绕过参数加密、逆向各种app。最后贴出参考资料。

## 源码+浏览+下载

本书的各种源码、在线浏览地址、多种格式文件下载如下：

### HonKit源码

* [crifan/reverse_debug_frida: 逆向调试利器：Frida](https://github.com/crifan/reverse_debug_frida)

#### 如何使用此HonKit源码去生成发布为电子书

详见：[crifan/honkit_template: demo how to use crifan honkit template and demo](https://github.com/crifan/honkit_template)

### 在线浏览

* [逆向调试利器：Frida book.crifan.org](https://book.crifan.org/books/reverse_debug_frida/website/)
* [逆向调试利器：Frida crifan.github.io](https://crifan.github.io/reverse_debug_frida/website/)

### 离线下载阅读

* [逆向调试利器：Frida PDF](https://book.crifan.org/books/reverse_debug_frida/pdf/reverse_debug_frida.pdf)
* [逆向调试利器：Frida ePub](https://book.crifan.org/books/reverse_debug_frida/epub/reverse_debug_frida.epub)
* [逆向调试利器：Frida Mobi](https://book.crifan.org/books/reverse_debug_frida/mobi/reverse_debug_frida.mobi)

## 版权和用途说明

此电子书教程的全部内容，如无特别说明，均为本人原创。其中部分内容参考自网络，均已备注了出处。如发现有侵权，请通过邮箱联系我 `admin 艾特 crifan.com`，我会尽快删除。谢谢合作。

各种技术类教程，仅作为学习和研究使用。请勿用于任何非法用途。如有非法用途，均与本人无关。

## 鸣谢

感谢我的老婆**陈雪**的包容理解和悉心照料，才使得我`crifan`有更多精力去专注技术专研和整理归纳出这些电子书和技术教程，特此鸣谢。

## 其他

### 作者的其他电子书

本人`crifan`还写了其他`150+`本电子书教程，感兴趣可移步至：

[crifan/crifan_ebook_readme: Crifan的电子书的使用说明](https://github.com/crifan/crifan_ebook_readme)

### 关于作者

关于作者更多介绍，详见：

[关于CrifanLi李茂 – 在路上](https://www.crifan.org/about/)
