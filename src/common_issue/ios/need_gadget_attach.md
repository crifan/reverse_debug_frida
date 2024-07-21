# Failed to attach: need Gadget to attach on jailed iOS

* 问题：Mac中用frida启动iOS版抖音
  ```bash
  frida -U -f com.ss.iphone.ugc.Aweme
  ```
  * 报错
    ```bash
    Failed to attach: need Gadget to attach on jailed iOS; its default location is: /Users/crifan/.cache/frida/gadget-ios.dylib
    ```
* 原因：缺少对应的Frida的`gadget`库文件
* 解决办法
  * 概述：下载对应版本的`gadget`库文件，放到对应位置（此处提示的`/Users/crifan/.cache/frida/gadget-ios.dylib`）即可
  * 详见：[安装Frida](../../install_upgrade/install_frida.md)中的`安装Frida的gadget`
