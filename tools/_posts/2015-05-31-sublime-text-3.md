---
title: sublime text3基本使用及常用插件介绍

---

# sublime text3基本使用及常用插件介绍

sublime text3下载地址：[http://www.sublimetext.com/3](http://www.sublimetext.com/3)本文所有操作都是基于Sublime Text3

这里是一个[非常全面的教程](http://zh.lucida.me/blog/sublime-text-complete-guide/)

## 快捷键列表

-   Ctrl + g    跳转到相应的行
-   Ctrl + m    在括号起始位置和终止位置之间切换
-   Ctrl + Shift + m    选中括号内内容
-   Ctrl + Shift + k    删除光标所在行
-   Ctrl + x    当光标选中区间时剪切选中区间，否则剪切光标所在行
-   Ctrl + Shift + up   向上选择行，并支持同时编辑多行
-   Ctrl + Shift + down 向下选择行，并支持同时编辑多行
-   Ctrl + l    选择光标所在行

## FAQs

### 不支持gbk编码

安装插件**ConvertToUTF8**,可能需要根据提示安装辅助插件

### HTML标签中间的大括号如何自动补全

菜单-> preferences -> Key Bindings - User在json配置文件中添加如下配置

    { "keys": ["{"], "command": "insert_snippet", "args": {"contents": "{$0}"}}

### 中文输入法不跟随输入位置

答：官方修复之前使用：IMESupport插件

### 如何为特定文件类型制定语法高亮，如为.handlebar文件设置html高亮

答：菜单中选择：`View > Syntax > Open all current extension as... > html`这样就可以为自定义后缀名文件使用所需的语法高亮



## Package Control

[Package Control][]是Sublime Text的包管理器，可以通过它安装[2000多个package][]。安装后的package将自动更新。基本上大部分工具通过自动和手动都可以完成安装。

### 通过控制台安装Package Control

1. 按快捷键ctrl + `调出控制台
2. 在控制台中运行如下代码

        import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

3. 以上代码将创建Package安装目录。并且下载[Package Control.sublime-package][]文件到目录下。
4. 重启Sublime Text，完成安装

### 手动安装Package Control
自动安装的原理其实就是在特定目录为Package Control创建文件夹和配置文件，手动创建所需目录，文件同样可以达到安装的目的：

1. 菜单中选择：`Preferences > Browse Packages...`
2. 在打开的资源管理器中向上一个目录，然后进入到`Installed Packages/`目录
3. 下载[Package Control.sublime-package][]并复制到`Installed Packages/`目录下
4. 重启Sublime Text，完成安装

[Package Control]: https://sublime.wbond.net/installation
[2000多个package]: https://sublime.wbond.net/browse
[Package Control.sublime-package]: https://sublime.wbond.net/Package%20Control.sublime-package



## Emmet

[Emmet][]通过简洁的语法描述html内容，提高工作效率

### 使用Package Control安装Emmet

1. 快捷键`ctrl + shift + p`然后在控制窗口中输入`Package Control: Install Package`
2. 选择`Emmet`安装，重启Sublime Text完成安装

### 快捷键

#### Tab

在HTML, XML, HAML, CSS, SASS/SCSS, LESS and strings in programming languages (like JavaScript, Python, Ruby etc.)中按Emmet语法写好语句后`Tab`键即可生成所需的代码。

由于某些语言中Sublime Text默认的Tab行为是作者更期望的，可以在`Emmet.sublime-setting`文件中设置`disable_tab_abbreviations_for_scopes`来取消Tab在这些文件类型中的触发。具体方法见官网[tab-key-handler][]

### ctrl + e

默认在大部分自定义后缀名的文件中使用Tab是不能触发Emmet的，但是使用`ctrl + e`可以在任意文档中生效，这在编写html模板时非常有用

### Emmet基本语法

[emmet-zen-coding-tutorial][]是个很不错的教程，下面是一些简单的语法规则

#### 元素

通过元素名生成HTML标签，如div生成&lt;div>&lt;/div>，当需要生成自定义标签时，使用`ctrl + e`即可，如foo生成&lt;foo>&lt;/foo>

### 子元素嵌套&gt;

类似CSS子选择器`div>ul>li`生成

    <div>
        <ul>
            <li></li>
        </ul>
    </div>

### 兄弟元素+

类似CSS兄弟选择器，+生成兄弟关系的元素`div+p+div`生成

    <div></div>
    <p></p>
    <div></div>

### 向上操作符^

Emmet操作符的作用对象是基于当前上下文的，&gt;操作符会让上下文向下转移到深层元素，^操作符可以向上移动上下文，如`div+div>p>span+em`生成

    <div></div>
    <div>
        <p><span></span><em></em></p>
    </div>

通过^操作符修改上下文控制元素`div+div>p>span+em^bq`生成

    <div></div>
    <div>
        <p><span></span><em></em></p>
        <blockquote></blockquote>
    </div>

### 多元素操作符*

使用*后跟数字，生成对于数量的元素`ul>li*5`生成

    <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>

### 分组操作符()

使用括号分组完成复杂的逻辑`div>(div>ul>li*2>a)*2+footer>p`生成

    <div>
        <div>
            <ul>
                <li><a href=""></a></li>
                <li><a href=""></a></li>
            </ul>
        </div>
        <div>
            <ul>
                <li><a href=""></a></li>
                <li><a href=""></a></li>
            </ul>
        </div>
        <footer>
            <p></p>
        </footer>
    </div>

### id和class生成

使用类似css中id和class的语法，可以为元素添加所需属性如`div#header+div.cls1.cls2`生成

    <div id="header"></div>
    <div class="cls1 cls2"></div>

### 自定义属性

使用类似css中[attr]的语法添加自定义属性`td[title="Hello" colspan=3]`生成

    <td title="hello" colspan="3"></td>

### 元素编号$

使用*生成多个元素的时候，可以使用$进行编号`ul>li.item$*5`生成

    <ul>
        <li class="item1"></li>
        <li class="item2"></li>
        <li class="item3"></li>
        <li class="item4"></li>
        <li class="item5"></li>
    </ul>

### {}添加文本

`ul>(li{item $})*3`生成

    <ul>
        <li>item 1</li>
        <li>item 2</li>
        <li>item 3</li>
    </ul>

[Emmet]: https://github.com/sergeche/emmet-sublime
[tab-key-handler]: https://github.com/sergeche/emmet-sublime#tab-key-handler
[emmet-zen-coding-tutorial]: http://www.landfancy.com/archives/emmet-zen-coding-tutorial/

## Sublime Text Markdown Preview

[sublimetext-markdown-preview][]是Sublime Text的一个Markdown预览插件，有了它就可以轻松使用Sublime Text编辑Markdown文档了。

### 使用Package Control安装

1. 如果没有安装过Package Control，先安装
2. 按快捷键`Ctrl + shift + p`打开命令窗口
3. 在命令窗口中执行`Package Control: Install Package`
4. 选择`Markdown Preview`并进行安装

### 手动安装

1. 在菜单中选择`Preferences > Browse Packages...`
2. 在弹出的资源管理器中向上一个目录，然后进入到`Installed Packages/`目录
3. 下载[sublimetext-markdown-preview压缩包][]到`Installed Packages/`目录下并重命名为`Markdown Preview.sublime-package`
4. 重启Sublime Text

### 预览Markdown文件

1. 打开Markdown文件
2. 快捷键`Ctrl + shift + p`，在控制窗口中输入`Markdown Preview`
3. 此时有多个选项可选，一般选择`Markdown Preview: Preview in Browser`
4. 此时要求选择解析器，任选一个即可
5. Sublime Text在默认浏览器中打开Markdown解析后的html文件，有时候只是在新选项卡中打开了html文件，可以右键：`copy file path`然后到浏览器中访问即可

[sublimetext-markdown-preview]: https://github.com/revolunet/sublimetext-markdown-preview
[sublimetext-markdown-preview压缩包]: https://github.com/revolunet/sublimetext-markdown-preview/archive/master.zip


## sublime-autoprefixer

[sublime-autoprefixer][]根据[Can I Use][]数据库信息为CSS样式添加适当的厂商前缀，也可以自定义需要添加前缀的浏览器版本。

sublime-autoprefixer只对CSS起作用，不处理Sass或者LESS之类的预处理格式。

### 使用Package Control安装sublime-autoprefixer

1. 快捷键`ctrl + shift + p`然后控制台输入`Package Control: Install Package`
2. 在弹出窗口中输入`Autoprefixer`，安装，重启Sublime Text

### 使用autoprefixer

1. 支持整个css文件添加前缀，也可选中部分cas代码添加前缀
2. 快捷键`ctrl + shift + p`，输入`Autoprefix CSS`回车

[sublime-autoprefixer]: https://github.com/sindresorhus/sublime-autoprefixer
[Can I Use]: http://caniuse.com/

## 使用[BracketHighlighter][]高亮括号配对

高亮括号配对在查找时很方便

### 使用Package Control安装BracketHighlighter

1. 如果没有Package Control，先安装
2. 快捷键`ctrl + shift + p`，在控制窗口中输入`Package Control: Install Package`
3. 在控制窗口中输入`BracketHighlighter`并选择安装
4. 安装完成

[BracketHighlighter]: https://github.com/facelessuser/BracketHighlighter/tree/ST3
