# frida-ls

* `frida-ls`
  * 是[frida-tools](../../../use_frida/sub_module/frida_tools/README.md)中一个工具
  * 作用：显示系统中的文件列表
    * 类似于`ls`
    * 支持：当前（Mac等）**PC端**的文件和（iPhone等）**移动端**中的文件

## 用法

* 显示当前PC端的文件：不加参数
  ```bash
  frida-ls
  ```
* 显示**USB**连接的（iPhone等）移动端中的所有文件：`-U` == `USB`
  ```bash
  frida-ls -U
  ```

## 举例


### `frida-ls`

```bash
➜  ~ frida-ls
drwxr-xr-x 20 root wheel  640 Thu Feb  9 09:39:53 2023 .
drwxr-xr-x 20 root wheel  640 Thu Feb  9 09:39:53 2023 ..
lrwxr-xr-x  1 root admin   36 Thu Feb  9 09:39:53 2023 .VolumeIcon.icns -> System/Volumes/Data/.VolumeIcon.icns
----------  1 root admin    0 Thu Feb  9 09:39:53 2023 .file
drwxr-xr-x  2 root wheel   64 Thu Feb  9 09:39:53 2023 .vol
drwxrwxr-x 45 root admin 1440 Wed Jun 14 14:45:54 2023 Applications
drwxr-xr-x 66 root wheel 2112 Fri May 12 19:14:14 2023 Library
drwxr-xr-x 10 root wheel  320 Thu Feb  9 09:39:53 2023 System
drwxr-xr-x  5 root admin  160 Sat Mar 18 14:26:26 2023 Users
drwxr-xr-x  3 root wheel   96 Mon Jun  5 02:34:08 2023 Volumes
drwxr-xr-x 39 root wheel 1248 Thu Feb  9 09:39:53 2023 bin
drwxrwxr-x  2 root admin   64 Mon Oct 24 06:20:41 2022 cores
dr-xr-xr-x  4 root wheel 5127 Sat Mar 18 14:26:04 2023 dev
lrwxr-xr-x  1 root wheel   11 Thu Feb  9 09:39:53 2023 etc -> private/etc
lrwxr-xr-x  1 root wheel   25 Sat Mar 18 14:27:10 2023 home -> /System/Volumes/Data/home
drwxr-xr-x  7 root wheel  224 Mon May 29 03:57:47 2023 opt
drwxr-xr-x  6 root wheel  192 Sat Mar 18 14:27:03 2023 private
drwxr-xr-x 64 root wheel 2048 Thu Feb  9 09:39:53 2023 sbin
lrwxr-xr-x  1 root wheel   11 Thu Feb  9 09:39:53 2023 tmp -> private/tmp
drwxr-xr-x 11 root wheel  352 Thu Feb  9 09:39:53 2023 usr
lrwxr-xr-x  1 root wheel   11 Thu Feb  9 09:39:53 2023 var -> private/var
```

### `frida-ls -U`

* 注：Mac中用USB连接的是，iOS 15.0的`iPhone8`，已用`palera1n`实现`rootful`越狱

```bash
➜  ~ frida-ls -U
drwxr-xr-x  26 root   wheel  832 Fri May  5 07:03:51 2023 .
drwxr-xr-x  26 root   wheel  832 Fri May  5 07:03:51 2023 ..
-rw-r--r--   1 mobile staff 6148 Mon Feb  6 03:41:36 2023 .DS_Store
drwx------   2 root   wheel   64 Thu Sep 16 05:39:21 2021 .ba
----------   1 root   admin    0 Thu Sep 16 05:39:21 2021 .file
drwx------   2 root   wheel   64 Fri May  5 06:58:46 2023 .fseventsd
drwx------   2 root   wheel   64 Thu Sep 16 05:39:21 2021 .mb
-rw-r--r--   1 mobile staff    0 Sun Nov 20 01:58:22 2022 .palecursus_strapped
-rw-r--r--   1 root   staff    0 Fri May  5 07:03:50 2023 .procursus_strapped
drwxrwxr-x 121 root   admin 3872 Wed Jun 14 13:34:36 2023 Applications
drwxr-xr-x   7 root   wheel  306 Thu Sep 16 05:39:46 2021 Developer
drwxrwxr-x  26 root   admin  832 Thu May 18 15:05:28 2023 Library
drwxr-xr-x   5 root   wheel  160 Thu Sep 16 05:39:21 2021 System
lrwxr-xr-x   1 root   wheel   12 Fri May  5 07:03:50 2023 User -> //var/mobile
drwxr-xr-x  39 root   wheel 1248 Fri May  5 07:40:50 2023 bin
drwxr-xr-x   2 root   wheel   64 Fri May  5 07:03:50 2023 boot
drwxrwxr-x   4 root   admin  440 Wed Jun 14 06:35:22 2023 cores
dr-xr-xr-x   4 root   wheel 1492 Wed Jun 14 06:35:21 2023 dev
lrwxr-xr-x   1 root   wheel   11 Thu Sep 16 05:39:21 2021 etc -> private/etc
drwxr-xr-x   2 root   wheel   64 Fri May  5 07:03:50 2023 lib
drwxr-xr-x   2 root   wheel   64 Fri May  5 07:03:50 2023 mnt
drwxr-xr-x   7 root   wheel  224 Thu Sep 16 05:39:21 2021 private
drwxr-xr-x  24 root   wheel  768 Fri May  5 07:39:45 2023 sbin
lrwxr-xr-x   1 root   wheel   15 Thu Sep 16 05:39:21 2021 tmp -> private/var/tmp
drwxr-xr-x  10 root   wheel  320 Fri May  5 07:03:49 2023 usr
lrwxr-xr-x   1 root   admin   11 Thu Sep 16 05:39:21 2021 var -> private/var
```

## 语法help

```bash
➜  ~ frida-ls --help
usage: frida-ls [options] [FILE]...

positional arguments:
  files                 files to list information about

options:
  -h, --help            show this help message and exit
  -D ID, --device ID    connect to device with the given ID
  -U, --usb             connect to USB device
  -R, --remote          connect to remote frida-server
  -H HOST, --host HOST  connect to remote frida-server on HOST
  --certificate CERTIFICATE
                        speak TLS with HOST, expecting CERTIFICATE
  --origin ORIGIN       connect to remote server with “Origin” header set to ORIGIN
  --token TOKEN         authenticate with HOST using TOKEN
  --keepalive-interval INTERVAL
                        set keepalive interval in seconds, or 0 to disable (defaults to -1 to auto-
                        select based on transport)
  --p2p                 establish a peer-to-peer connection with target
  --stun-server ADDRESS
                        set STUN server ADDRESS to use with --p2p
  --relay address,username,password,turn-{udp,tcp,tls}
                        add relay to use with --p2p
  -O FILE, --options-file FILE
                        text file containing additional command line options
  --version             show program's version number and exit
```
