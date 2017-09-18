---
title: Java开发环境搭建
---

# 安装jdk

1. oracle官网下载jdk安装文件安装
2. `/usr/libexec/java_home -V`查看本机所有jdk
3. `vim ~/.bash_profile`:

    ```
    export JAVA_HOME=$JAVA_8_HOME
    alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
    PATH=$PATH:$JAVA_HOME/bin
    CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    ```
4. `source ~/.bash_profile`


# 安装maven

1. Apache官网下载maven解压到本地
2. `vim ~/.bash_profile`添加:

    ```
    # maven
    export M2_HOME=/Users/songmengjiao/Desktop/tool/apache-maven-3.5.0
    export M2=$M2_HOME/bin
    export PATH=$M2:$PATH
    ```
3. `source ~/.bash_profile`
4. `mvn -v`显示版本信息
5. (可选)修改maven本地仓库路径:修改`M2_HOME/conf/settings.xml`下`<localRepository></localRepository>`到制定目录.


GkZMBrX!w8F=

http://www.codingpedia.org/ama/how-to-prepare-the-macbook-pro-for-java-development-and-more/
