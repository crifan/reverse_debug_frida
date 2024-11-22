# 逆向调试利器：Frida

* 最新版本：`v4.3.0`
* 更新时间：`20241122`

## 简介

介绍支持Android、iOS等多个平台的通用逆向工具：Frida。先是Frida概览，包括Frida的代码和架构，以及相关文档和案例和资料。再介绍如何在PC端和移动端即iOS和安卓端的安装和升级Frida。然后是如何使用Frida，先介绍Frida的典型使用逻辑，然后再去介绍frida命令行工具，其中包括通用的逻辑，比如调试目标的方式，以及写js脚本；然后是典型的使用方式，以及此处iOS逆向涉及到的ObjC的内容，包括ObjC的参数和变量类型；接着是数据类型，包括NativePointer。接着是高级的Stalker。接着介绍frida-trace，以及frida-tools工具集合，包括frida-ps、frida-ls、frida-ls-devices等；接着介绍其他相关的内容，包括frida-server等。接着整理一些Frida开发期间的经验和心得，包括hook函数方面的，包括frida和frida-trace，frida中的Interceptor、Stalker、常用iOS函数等。其他还有工具类的函数、js以及其中的console.log，和自己编译frida-server，和其他常见问题和报错。接着整理基于Frida的工具。以及Frida的一些常见用途，包括反调试、辅助反混淆、绕过参数加密、逆向各种app等。最后贴出参考资料。

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
