# 逆向app

## 抖音

* 从app启动最开始就开始hook调试
  * 交互式调试
    ```bash
    frida -U -f com.ss.iphone.ugc.Aweme
    ```
  * 用js脚本
    * 调试`dyld`
      ```bash
      frida -U -f com.ss.iphone.ugc.Aweme -l frida/dyldImage.js
      ```
* 其他
  * 【未解决】Mac中Frida启动抖音app进程并调试和hook函数
  * 【未解决】用Frida的frida-trace去hook函数iOS版抖音
  * 【未解决】frida调试抖音app去hook函数：_dyld_get_image_name
  * 【未解决】frida去hook函数_dyld_get_image_name时打印参数为字符串
  * 【未解决】用Frida动态调试iOS版抖音app
  * 【未解决】Mac中用Frida调试iOS版抖音
  * 【已解决】用frida启动hook调试iOS抖音app
  * 【未解决】尝试Frida的stalker能否修复抖音AwemeCore中函数名常量字符串

## Apple账号 ~= AppleStore

* Apple账号 ~= AppleStore = `Preferences` + （`AuthKit`的daemon）`akd` + `AppleAccount` + `AppleAccountUI` + `AppleMediaServices` + `libMobileGestalt` + 其他
  * 【记录】iOS逆向Apple账号：用frida和frida-trace去hook打印更多账号相关函数调用
  * 【无法解决】iOS逆向Apple账号：用frida的ssl bypyass脚本尝试解决Charles抓包代理报错
  * 【未解决】iOS逆向Apple账号：用Frida去监控NSURL去调试Apple账号登录过程
  * 【未解决】iOS逆向Apple账号：用Frida去调试NSURL核心网络请求函数调用
  * 【未解决】iOS逆向Apple账号：分析研究frida抓包到的Apple账号登录过程和网络相关的内容

## 迅雷

* 迅雷
  * 【记录】用frida动态调试重新打包后的安卓迅雷apk
  * 【未解决】Mac中搭建Frida的动态调试安卓apk的开发环境
