# Waiting for USB device to appear

* 问题：Mac中运行frida去调试app
  ```bash
  frida -U -f com.apple.store.Jolly
  ```
  * 但是报错
    ```bash
    Waiting for USB device to appear...
    ```
* 原因：（连接iPhone到Mac的）USB数据线没插好
* 解决办法：重新拔插USB数据线，确保USB连接正常
  * 注：
    * 如何确认iPhone是否已插好
      * 方式1：通过爱思助手可以确认
        * 已插好：能看到iPhone详情
        * 没插好：看不到iPhone设备
      * 方式2：用frida的工具[frida-ls-devices](../../use_frida/frida_tools/frida_ls_devices.md)
