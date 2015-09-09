---
title: svn客户端命令
---

## svn checkout: 从服务器仓库获取项目到本地

`svn checkout`命令用于从SVN服务器仓库获取项目源代码到本地.

语法:

```
svn checkout/co URL PATH
```

其中:

- URL是项目服务器地址
- PATH是远程项目保存到本地的地址, 如果不设置, 会在当前目录下创建项目目录并下载.


## svn status: 查看本地项目文件状态

`svn status`用于查看本地文件状态: 修改, 新增, 删除, 未加入版本控制等.

语法:

```
svn status PATH
```

`svn help status`查看详细帮助


## svn add: 添加文件到版本控制

要提交一个文件到服务器仓库需要两个步骤:

1. svn add将文件添加到版本控制
2. svn commit将文件提交到服务器


