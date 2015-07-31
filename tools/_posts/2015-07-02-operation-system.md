---
title: 操作系统常见操作
---

## mac下文件大于4G无法拷贝到U盘

U盘文件格式不对, 应该格式化为exFAT, 步骤

1. 点击屏幕右上角的搜索图标
2. 搜索并打开磁盘工具
3. 左侧选择U盘, 右侧选择抹掉,格式选择exFAT

完成之后就可以复制了

## mac工具软件

- http://support.alfredapp.com/
- http://manytricks.com/moom/

## 文本对比工具

- http://www.scootersoftware.com/v4help/index.html?using_text_merge.html

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
