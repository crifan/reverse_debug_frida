# frida-server

越狱手机`iPhone`安装`Frida`后，就可以通过`Frida`插件的`已安装文件`：

![frida_installed_files](../../assets/img/frida_installed_files.png)

看到安装了2个核心文件：

* `/usr/lib/frida/frida-agent.dylib`
* `/usr/sbin/frida-server`
  * 其中`frida-server`就是`Frida`的server端

## `frida-server`语法help

```bash
iPhone11-151:~ root# frida-server --help
Usage:
  frida [OPTION?]

Help Options:
  -h, --help                            Show help options

Application Options:
  --version                             Output version information and exit
  -l, --listen=ADDRESS                  Listen on ADDRESS
  --certificate=CERTIFICATE             Enable TLS using CERTIFICATE
  --origin=ORIGIN                       Only accept requests with ?Origin? header matching ORIGIN (by default any origin will be accepted)
  --token=TOKEN                         Require authentication using TOKEN
  --asset-root=ROOT                     Serve static files inside ROOT (by default no files are served)
  -d, --directory=DIRECTORY             Store binaries in DIRECTORY
  -D, --daemonize                       Detach and become a daemon
  --policy-softener=system|internal     Select policy softener
  -P, --disable-preload                 Disable preload optimization
  -C, --ignore-crashes                  Disable native crash reporter integration
  -v, --verbose                         Be verbose
```
