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
