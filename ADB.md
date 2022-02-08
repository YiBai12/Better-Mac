ADB，全称为 Android Debug Bridge，它是 Android 开发/测试人员不可替代的强大工具。

# 一、ADB 命令的安装：

```shell
brew cask install android-platform-tools
```

# 二、电脑和手机的连接：

## 1. usb 连接

-   打开 Android 手机的 USB 调试模式，并连接到 MAC 电脑;
-   使用命令`adb devices`查看已连接的手机，
    -   如果找不到，则：
        -   使用命令`system_profiler SPUSBDataType`查看手机的 VID，并将 VID 写入到~/.android/adb_usb.ini 文件中，该文件可能需要新建;
        -   使用命令`adb kill-server`停止服务，并使用命令`adb start-server`重启服务;
        -   再次执行`adb devices`查看已连接的手机。

## 2. 无线连接

-   保证手机和电脑处在同一个无线网络内在 USB 连接的基础上，执行命令`adb tcpip 5555`断开 USB 连接，
-   执行命令`adb connect 192.168.x.x:5555`
-   此时执行命令`adb devices`即可查看到连接的手机设备信息

# 三、ADB 常用命令（不断增加）

## 1. adb 服务相关

-   启动服务

```
adb start-server
```

-   终止服务

```
adb kill-server
```

-   远程连接云手机

```
adb connect 云手机 ip+端口
```

## 2. 连接设备相关

-   查看连接设备

```
adb devices
```

-   重启设备

```
adb reboot [bootloader|recovery] #可选参数进入 bootloader(刷机模式)或 recovery(恢复模式)
```

## 3. apk 相关

-   安装 apk

```
adb install app 的本地绝对路径
adb install -r app 的本地绝对路径 #删除已安装,并安装
```

-   卸载 apk

```
adb uninstall 包名
adb uninstall -k 包名#可选参数-k 的作用为卸载软件但是保留配置和缓存文件
```

-   查看 app 相关所有信息

```
adb shell dumpsys package 包名
```

-   查看 app 的路径

```
adb shell pm path 包名
```

-   删除与包相关的所有数据：清除数据和缓存

```
adb shell pm clear 包名
```

-   查看已安装的 app

```
adb shell pm list packages
adb shell pm list packages | grep 指定名称 #找到指定包名
```

## 4. 上传或者下载

-   上传

```
adb push 本地路径 手机路径
```

-   下载

```
adb pull 手机路径 本地路径
```

## 5. 进入 shell 命令

```
adb shell
```
