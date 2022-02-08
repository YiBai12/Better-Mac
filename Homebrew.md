# 一、简介

Homebrew 是 macOS 和 Linux 缺失软件包的管理器，使用 Homebrew 安装包或软件。

# 二、安装

## 1. 官方源：

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 2. 国内源：

```shell
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

# 三、使用

## 1. 更新 Homebrew

```
brew udpate
```

## 2. 安装软件包

```
brew install [包名]
```

## 3. 查询可更新的包

```
brew outdated
```

## 4. 更新包 (formula)

```
//更新所有
brew upgrade

//更新指定包
brew upgrade [包名]
```

## 5. 清理旧版本

```
//清理所有包的旧版本
brew cleanup

//清理指定包的旧版本
brew cleanup [包名]

//查看可清理的旧版本包，不执行实际操作
brew cleanup -n
```

## 6. 锁定不想更新的包

```
//锁定某个包
brew pin $FORMULA
//取消锁定
brew unpin $FORMULA
```

## 7. 卸载安装包

```
brew uninstall [包名]
```

## 8. 查看包信息

```
brew info [包名]
```

## 9. 查看安装列表

```
brew list
```

## 10. 查询可用包

```
brew search [包名]
```

# 四、卸载

## 1. 官方源：

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```

## 2. 国内源：

```shell
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```
