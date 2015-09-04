---
title: 64位win7如何安装php
---

## php安装

1. 去[php官网][4]下载window对应版本的zip压缩包,[php-5.6.13-Win32-VC11-x64.zip][2]
2. 将压缩包解压到本地文件夹, 如`C:\tools\php-5.6.13-nts-Win32-VC11-x64`,将该目录添加到环境变量的path.
3. 此时在命令行运行`php -v`会提示**无法启动此程序, 因为计算机中丢失`MSVCR110.dll`**,或者

    ```
    /c/tools/php-5.6.13-nts-Win32-VC11-x64/php.exe: error while loading shared libraries: MSVCR110.dll: cannot open shared object file: No such file or directory
    ```
    因为`VC11`版本的php是对应visual studio编译产生, 需要依赖对应dll文件.需要下载[64位微软官方The Visual C++ Redistributable Packages install runtime components ][3]并安装.

4. 此时运行`php -v`显示如下版本信息即表示安装成功

```
PHP 5.6.13 (cli) (built: Sep  3 2015 15:13:31)
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2015 Zend Technologies
```

## 参考资料

- [win7(32/64)+apache2.4+php5.5+mysql5.6 环境搭建配置][1]


[4]: http://windows.php.net/download#php-5.6
[3]: http://www.microsoft.com/en-us/download/details.aspx?id=30679#tc_qz_original=1440365482
[2]: http://windows.php.net/downloads/releases/php-5.6.13-Win32-VC11-x64.zip
[1]: http://blog.csdn.net/z_cf1985/article/details/22454749
