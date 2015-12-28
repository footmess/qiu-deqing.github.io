---
title: 读取本地文件
---

# FileReader

`FileReader`提供一系列用于读取`File`或者`Blob`对象的方法. 这些方法都是异步的.

```
var reader = new FileReader();
```

上面的代码创建了一个FileReader实例用于读取文件, FileReader提供了一些列用于读取文件的方法

## readAsText({File|Bolb}, encoding)

`readAsText()`方法用于读取文本文件, 接受两个参数, 第一个是需要读取的File或者Blob对象, 第二个是可选的文件编码, 默认值为**UTF-8**, 文件读取完成之后会触发`load`事件, FileRader实例的result包含文件内容.

```
var reader = new FileReader()
reader.onload = function (e) {
    var text = reader.result
}
reader.readAsText(file, encoding)
```

## readAsDataURL({File|Blob})

读取File或者Blob对象并且生成对应的**data URL**. 文件读取完成之后触发`load`事件, 数据包含在FileReader对象实例的result属性中, 通常是文件数据的base64编码字符串.

```
var reader = new FileReader()
reader.onload = function (e) {
    var dataURL = reader.result
}
reader.readAsDataURL(file)
```

## readAsBinaryString({File|Blob})

可以用此方法读取任意类型文件并返回文件的二进制编码, 通过与`XMLHttpRequest.sendAsBinary()`结合可以实现上传任意文件到服务器.

```
var reader = new FileReader()
reader.onload = function (e) {
    var rawData = reader.result
}
reader.readAsBinaryString(file)
```

## readAsArrayBuffer({File|Blob})

读取File或者Blob生成`ArrayBuffer`. `ArrayBuffer`是固定长度的二进制数据缓存. 在执行文件操作的时候很有用,例如将JPEG转换为PNG

```
var reader = new FileReader()
reader.onload = function (e) {
    var arrayBuffer = reader.reault
}
reader.readAsArrayBuffer(file)
```

## about()

停止读操作

```
reader.abort()
```

# 例子

以下是简单例子

## 使用FileReader API读取文本文件

以下代码监听input的change事件, 读取并输出文本内容

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>

<input type="file">

<script>
var fileInput = document.querySelector('input')
var textFileReg = /text.*/

fileInput.addEventListener('change', function (e) {
    var file = fileInput.files[0]
    if (file && textFileReg.test(file.type)) {
        var reader = new FileReader()
        reader.onload = function () {
            console.log(reader.result)
        }
        reader.readAsText(file, 'UTF-8')
    } else {
        console.log('请选择文本文件')
    }

}, false)

</script>
</body>
</html>
```

## 读取本地图片并显示

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>

<input type="file">

<script>
var fileInput = document.querySelector('input')
var imgFileReg = /image.*/

fileInput.addEventListener('change', function (e) {
    var file = fileInput.files[0]
    if (file && imgFileReg.test(file.type)) {
        var reader = new FileReader()
        reader.onload = function () {
            var img = document.createElement('img')
            img.src = reader.result
            document.body.appendChild(img)
        }
        reader.readAsDataURL(file)
    } else {
        console.log('请选择图片文件')
    }

}, false)

</script>
</body>
</html>

```

# 参考资料

- [http://blog.teamtreehouse.com/reading-files-using-the-html5-filereader-api][1]
- [http://www.w3.org/TR/file-upload/][2]


[2]: http://www.w3.org/TR/file-upload/
[1]: http://blog.teamtreehouse.com/reading-files-using-the-html5-filereader-api
