---
title: 操作系统常见操作
---


## linux为文件创建软链接(快捷方式)

```
ln -s srcFile targetFile
```

在targetFile处为srcFile创建软连接, 常见应用

## 在项目目录下创建host文件的快捷方式, 方便直接绑定host

```
ln -s /etc/hosts /Users/qiudeqing/Desktop/projects/hosts
```

执行以上命令之后projects目录下就有一个hosts文件, 对它的修改会自动映射到`/etc/hosts`方便操作

## 在项目目录下创建nginx配置文件快捷方式,方便配置

```
ln -s /usr/local/etc/nginx/nginx.conf  /Users/qiudeqing/Desktop/projects/nginx.conf
```

## mac如何让Finder显示隐藏文件和文件夹

终端执行:
```
defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder
```

## mac如何让Fider隐藏特殊文件和文件夹

终端执行:
```
defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder
```
