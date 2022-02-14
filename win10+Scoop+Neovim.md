# Win10下，用Scoop安装Neovim，尝试能否解决vim+ultisnips无法正常使用的问题



## scoop

windows下的安装源搜索工具，有点类似centos下的yum和Ubuntu下的apt。用这个拉下来安装的软件没有广告。

## 安装条件

　win7 + 

　　powershell 3+

　　查看当前版本

```
$PSVersionTable.PSVersion
```

更改脚本执行的策略

```
set-executionpolicy remotesigned -s cu
```

## 安装

不太喜欢装在C盘里面，故更改它的安装路径到D盘

### 　设置scoop环境变量

```
$env:SCOOP='D:\Scoop'

[Environment]::SetEnvironmentVariable('SCOOP',$env:SCOOP,'User')
```

### 设置scoop global 环境变量

```
$env:SCOOP_GLOBAL='D:\ScoopGlobalApps'

[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL',$env:SCOOP_GLOBAL,'User')
```

这里设置环境变量第三个参数User表示用户级别，Machine表示系统级别。Machine没权限的话，可以手动去环境变量设置。

### 安装命令

```
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')

## 或者

iwr-useb get.scoop.sh|iex
```

我的电脑能用上边这个，若不能用，配置hosts

![image-20210920202039846](https://raw.githubusercontent.com/lunnche/picgo-image/main/202109202020897.png)

然后执行另一条可用的网址的命令

```
iex (new-object net.webclient).downloadstring('https://raw.githubusercontent.com/lukesampson/scoop/master/bin/install.ps1')  
```

安装成功。

### 卸载

```
scoop uninstall scoop
```

这个卸载，会删除你配置的scoop下面的所有软件，非常==危险==。

## 基本使用

### bucket

　　　　本身的main bucket源很少，故增加新的一个较多的bucket

```
scoop bucket add extras
```

```
Checking repo... ok
The extras bucket was added successfully.
```

### 安装软件

```
## 安装软件
scoop install -g [app的名称]
## 我这里-g 需要admin权限，取消-g可安装
scoop install curl
```

***

***

***

换上了Neovim还是一样的问题，应不是vim的问题，怀疑和ultisnips与python强依赖有关？
