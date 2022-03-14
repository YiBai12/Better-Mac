# 一、安装

```
# 使用 Homebrew 安装 aria2
brew install aria2
```

brew 安装的内容统一可以在 /usr/local/Cellar 下可以查找到对应的文件夹。

安装完成后先打开文件夹位置： /usr/local/Cellar/aria/对应版本号/bin/

将文件夹内的 aria2c 重命名为 aria2c.bak，并将下载资源中的 aria2c 复制进去。

这一步的原由是 aria2c 官方版最多支持 16 线程，这个修改版最多支持 128 线程。

# 二、配置

## 2.1 创建配置文件 aria2.conf 和空对话文件 aria2.session

```
mkdir ~/.aria2 && cd ~/.aria2
touch aria2.conf
touch aria2.session
```

## 2.2 编辑 aria2.conf

本人设置文件:

-   默认开启 RPC 模式

-   已设置 RPC 授权令牌

-   已经添加 BT tracker，更多详见 [XIU2/TrackersListCollection](https://trackerslist.com/#/zh)

设置 aria2.conf 文件如下：

```
$ cat aria2.conf

## '#'开头为注释内容, 选项都有相应的注释说明, 根据需要修改 ##
## 被注释的选项填写的是默认值, 建议在需要修改时再取消注释  ##

## 沙漠之子 自用 2020-2-6 ##

## 文件保存相关 ##

# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=${HOME}/Downloads
# 启用磁盘缓存, 0为禁用缓存, 需1.16以上版本, 默认:16M
disk-cache=32M
# 文件预分配方式, 能有效降低磁盘碎片, 默认:prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc和trunc则需要文件系统和内核支持
# NTFS建议使用falloc, EXT3/4建议trunc, MAC 下需要注释此项
#file-allocation=none
# 断点续传
continue=true

## 下载连接相关 ##

# 最大同时下载任务数, 运行时可修改, 默认:5
max-concurrent-downloads=5
# 同一服务器连接数, 添加时可指定, 默认:1
max-connection-per-server=128
# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定size=10M, 文件为20MiB 则使用两个来源下载; 文件为15MiB 则使用一个来源下载
min-split-size=2KiB
# 单个任务最大线程数, 添加时可指定, 默认:5
split=128
# 分片选择算法,有助于视频的边下边播同时兼顾减少建立连接的次数
stream-piece-selector=geom
# 整体下载速度限制, 运行时可修改, 默认:0
#max-overall-download-limit=0
# 单个任务下载速度限制, 默认:0
#max-download-limit=0
# 整体上传速度限制, 运行时可修改, 默认:0
#max-overall-upload-limit=0
# 单个任务上传速度限制, 默认:0
#max-upload-limit=0
# 禁用IPv6, 默认:false
#disable-ipv6=true
# 连接超时时间, 默认:60
timeout=60
# 最大重试次数, 设置为0表示不限制重试次数, 默认:5
max-tries=5
# 设置重试等待的秒数, 默认:0
retry-wait=10

## 进度保存相关 ##

# 日志文件
log-level=notice
log=${HOME}/.aria2/aria2.log
# 从会话文件中读取下载任务
# 需提前创建一个空文件否则会报错
input-file=${HOME}/.aria2/aria2.session
# 在Aria2退出时保存`错误/未完成`的下载任务到会话文件
save-session=${HOME}/.aria2/aria2.session
# 定时保存会话, 0为退出时才保存, 需1.16.1以上版本, 默认:0
save-session-interval=60

# 强制保存会话, 即使任务已经完成, 默认:false
# 较新的版本开启后会在任务完成后依然保留.aria2文件
#force-save=true

## RPC相关设置 ##

# 启用RPC, 默认:false
enable-rpc=true
# 允许所有来源, 默认:false
rpc-allow-origin-all=true
# 允许非外部访问, 默认:false
rpc-listen-all=true
# RPC监听端口, 端口被占用时可以修改, 默认:6800
rpc-listen-port=6800
# 设置的RPC授权令牌
# 此处使用`openssl rand -base64 32`命令生成<TOKEN>
# rpc-secret=<TOKEN>
# 是否启用 RPC 服务的 SSL/TLS 加密,
# 启用加密后 RPC 服务需要使用 https 或者 wss 协议连接
#rpc-secure=true
# 在 RPC 服务中启用 SSL/TLS 加密时的证书文件,
# 使用 PEM 格式时，您必须通过 --rpc-private-key 指定私钥
#rpc-certificate=/path/to/certificate.pem
# 在 RPC 服务中启用 SSL/TLS 加密时的私钥文件
#rpc-private-key=/path/to/certificate.key

## HTTP 设置 ##

# 自定义 User Agent
user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.85 Safari/537.36

## BT/PT下载相关 ##

# 当下载的是一个种子(以.torrent结尾)时, 自动开始BT任务, 默认:true
follow-torrent=true
# BT监听端口, 当端口被屏蔽时使用, 默认:6881-6999
listen-port=6881-6999
# 单个种子最大连接数, 默认:55
bt-max-peers=55

### DHT 功能, 仅对 BT 生效, PT 无效###
# 打开 DHT (IPv4) 功能
enable-dht=true
# 打开 DHT (IPv6) 功能
enable-dht6=true
# DHT网络监听端口, 默认:6881-6999
dht-listen-port=6881-6999
# 本地节点查找
bt-enable-lpd=true
# 种子交换
enable-peer-exchange=true
# DHT (IPv4) 路由表文件路径
dht-file-path=${HOME}/.aria2/dht.dat
# DHT (IPv6) 路由表文件路径
dht-file-path6=${HOME}/.aria2/dht6.dat

# 客户端伪装, PT需要
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77

# 同一服务器连接数
# 每个种子限速, 对少种的PT很有用, 默认:50K
#bt-request-peer-speed-limit=50K
# 当种子的分享率达到这个数时, 自动停止做种, 0为一直做种, 默认:1.0
seed-ratio=1.0
# BT校验相关, 默认:true
#bt-hash-check-seed=true
# 继续之前的BT任务时, 无需再次校验, 默认:false
bt-seed-unverified=true
# 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
bt-save-metadata=true

# BT 服务器地址
# 逗号分隔的 BT 服务器地址. 如果服务器地址在 --bt-exclude-tracker 选项中, 其将不会生效.
bt-tracker=
# BT 排除服务器地址
bt-exclude-tracker=

# 启用后台进程
daemon=false

# 部分事件hook, 调用第三方命令:/path/to/command
# BT下载完成(如有做种将包含做种，如需调用请务必确定设定完成做种条件)
on-bt-download-complete=${HOME}/.aria2/download-complete-hook.sh
# 下载完成
on-download-complete=${HOME}/.aria2/download-complete-hook.sh
# 下载错误
on-download-error=

# 代理 仅支持 HTTP 协议
#all-proxy=http://127.0.0.1:1087

# BT下载完成(如有做种将包含做种，如需调用请务必确定设定完成做种条件)
on-bt-download-complete=${HOME}/.aria2/download-complete-hook.sh
# 下载完成
on-download-complete=${HOME}/.aria2/download-complete-hook.sh
```

# 三、启动

## 3.1 终端静默启动

```
$ aria2c --conf-path="/Users/xxxxxx/.aria2/aria2.conf" -D
```

## 3.2 设置为 macOS 的开机启动

### 3.2.1 创建用户启动文件

```
touch ~/Library/LaunchAgents/aria2.plist
```

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>KeepAlive</key>
        <true/>
        <key>Label</key>
        <string>com.aria2c</string>
        <key>ProgramArguments</key>
        <array>
            <string>/usr/local/Cellar/aria2/1.36.0/bin/aria2c</string>
            <string>--conf-path=/Users/yixinhu/.aria2/aria2.conf</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
        <key>WorkingDirectory</key>
        <string>/Users/yixinhu/Downloads</string>
    </dict>
</plist>
```

    注意: 修改 WorkingDirectory 目录

### 3.2.2 检查 plist 语法是否正确

```
plutil ~/Library/LaunchAgents/aria2.plist
```

### 3.2.3 修改文件权限

```
chmod 644 ~/Library/LaunchAgents/aria2.plist
```

### 3.2.4 添加并启用自启动项:

```
# 添加自启动项: aria2
launchctl load ~/Library/LaunchAgents/aria2.plist

# 删除自启动项: aria2
launchctl unload ~/Library/LaunchAgents/aria2.plist

# 启动服务: aria2
launchctl start aria2

# 停止服务: aria2
launchctl stop aria2
```

# 四、添加自动更新 BT tracker 功能

## 4.1 创建 trackers-list-aria2.sh 脚本:

```
touch ~/.aria2/trackers-list-aria2.sh
```

写入如下:

```
#!/bin/bash
#trackers-list-aria2.sh
# aria2 设置文件路径
CONF=${HOME}/.aria2/aria2.conf

#设置选择的 trackerlist （可选 all_aria2.txt, best_aria2.txt, http_aria2.txt）
trackerfile=all_aria2.txt
#downloadfile=https://raw.githubusercontent.com/ngosang/trackerslist/master/${trackerfile}
downloadfile=https://trackerslist.com/${trackerfile}

list=$(curl -fsSL ${downloadfile})
if ! grep -q "bt-tracker" "${CONF}" ; then
    echo -e "\033[34m==> 添加 bt-tracker 服务器信息......\033[0m"
    echo -e "\nbt-tracker=${list}" >> "${CONF}"
else
    echo -e "\033[34m==> 更新 bt-tracker 服务器信息.....\033[0m"
    sed -i '' "s@bt-tracker.*@bt-tracker=${list}@g" "${CONF}"
fi

## 重启 aria2 服务
echo -e "\033[34m==> 停止 aria2 服务......\033[0m"
launchctl stop aria2
echo -e "\033[34m==> 启动 aria2 服务......\033[0m"
launchctl start aria2
```

## 4.2 设置任务计划程序 实现自动更新:

编译当前用户任务计划:

```
crontab -e
```

在打开的 vi 中 键入如下, 并使用:wq 命令保存退出, 可用 crontab -l 查看当前用户任务计划

```
0 18 * * * ~/.aria2/trackers-list-aria2.sh
```

或者直接:

```
(crontab -l 2&> /dev/null; echo "0 18 * * * ~/.aria2/trackers-list-aria2.sh") | crontab
```

取消计划任务

```
crontab -e
```

然后手动删除, 或者

```
crontab -l 2&> /dev/null| sed "/trackers-list-aria2.sh/d" | crontab
```

# 五、添加下载通知

## 5.1 创建 download-complete-hook.sh 脚本:

```
touch ~/.aria2/download-complete-hook.sh
```

写入如下:

```
$ cat download-complete-hook.sh

#!/bin/sh
# 给aria2 RPC添加一个下载完成通知 for macOS
# 最终效果：当下载完成会在屏幕右上角弹出一个提示框显示具体下载完成的文件名，
# 同时中文语音播报：“有个文件下载完成，请查收！”
# 变量 3 表示下载完成文件的路径
# 具体提示框设置可参考`https://code-maven.com/display-notification-from-the-mac-command-line`。
# 不支持设置自定义图标

fname=`basename $3`
osascript <<EOF
display notification "$fname 已经下载完成！" with title "【下载完成】"
say "有个文件下载完成，请查收！"
EOF
```

## 5.2 设置运行权限:

```
chmod +x ~/.aria2/download-complete-hook.sh
```

# 六、配置 Web 界面

-   无需安装，直接使用浏览器打开: http://ariang.mayswind.net/latest/
-   安装浏览器插件
