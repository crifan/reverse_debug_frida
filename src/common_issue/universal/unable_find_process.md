# Failed to spawn: unable to find process with name

* 报错：`Failed to spawn: unable to find process with name 'Preferences'`
  * ![frida_failed_spawn_process_name](../../assets/img/frida_failed_spawn_process_name.png)
* 原因：frida命令用的是
  ```bash
  frida -U -l ./hookAccountLogin.js -n Preferences
  ```
  * 其中`-n`是加二进制名称，此处`Preferences`是app，所以属于参数使用错误，调试目标语法搞错了
* 解决办法：
  * 搞懂frida的调试目标方式，改为别的方式即可
    * 方式1：用`-N app_package_id`
      ```bash
      frida -U -l ./hookAccountLogin.js -N com.apple.Preferences
      ```
    * 方式2：换`-p PID`
      ```bash
      frida -U -l ./hookAccountLogin.js -p 18031
      ```
      * 其中是用iPhone中ssh中通过ps查看到
        ```bash
        ~ ps -A | grep Preferences
        ...
        18031 ??         0:02.43 /Applications/Preferences.app/Preferences
        ```
        * 得知`Preferences`的`PID`是`18031`
    * 方式3：用`-F`
      ```bash
      frida -U -l ./hookAccountLogin.js -F
      ```
      * 注：确保`Preferences`=系统的设置app，处于最前台在运行才能用`-F`
