# Frida Codeshare

由于Frida功能太强大，使得用的人很多，出现了很多用法和案例。官网专门做了个代码分享平台，列出了不同人的不用用法供大家参考，叫做：

* [Frida CodeShare](https://codeshare.frida.re)
    * 概述：The Frida CodeShare project is comprised of developers from around the world working together with one goal - push Frida to its limits in new and innovative ways.
    * 列表页面
      * [Frida CodeShare BROWSE CODE](https://codeshare.frida.re/browse)

下面列出一些，相对常用的案例：

* iOS工具类
  * [Project: iOS Utils](https://codeshare.frida.re/@lichao890427/ios-utils/)
* 绕过证书校验
  * [Project: iOS SSL Bypass](https://codeshare.frida.re/@lichao890427/ios-ssl-bypass/)

## 如何利用Frida codeshare的代码

除了拷贝代码放到自己`js`文件中之外，还可以直接引用：

直接调用分享的代码：

```bash
frida --codeshare <frida_codeshare_url_without_url_prefix> -f <YOUR_BINARY>
```
* 举例
  * ios-utils
    ```bash
    frida --codeshare lichao890427/ios-utils -f <YOUR_BINARY>
    ```
      * 对应URL是：https://codeshare.frida.re/@lichao890427/ios-utils/
  * dump-ios
  ```bash
  frida --codeshare lichao890427/dump-ios -f YOUR_BINARY
  ```
