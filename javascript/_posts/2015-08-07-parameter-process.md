---
title: 表单参数获取

---

## 需求

一个用户注册页面,要求用户在表单中填写姓名,年龄,电子邮箱,个人说明.点击提交之后ajax发送到后台进行保存.

接口'/user'参数如下

```
{
  "user.name": "name",
  "user.age": 22,
  "user.email": "qiu@126.com",
  "user.desc": "说明"
}
```

## 版本1.0

```
<form action="#" class="user-info">
  <div>
    <label>姓名:<input type="text" name="user.name"></label>
  </div>
  <div>
    <label>年龄:<input type="text" name="user.age"></label>
  </div>
  <div>
    <label>电子邮箱<input type="text" name="user.email"></label>
  </div>
  <div>说明:
    <textarea name="user.desc"></textarea>
  </div>
  <div>
    <button type="submit" class="btn-submit">确定注册</button>
  </div>
</form>

<script>
var userInfo = $('.user-info');
userInfo.on('submit', function (e) {
  e.preventDefault();

  $.ajax({
    url: '/user',
    data: userInfo.serialize()
  })
  .done(function (resp) {
    console.log('注册成功');
  });
})
</script>
```

实现了需求,但是代码揉在一起, 可维护性, 测试性差, 可以优化, 这里主要优化js.

## 版本2.0



## 扩展需求:

1. **参数校验**: 姓名不能为空且长度不超过10, 年龄取值区间[1, 99], 邮箱需符合基本格式, 验证不通过不能提交
    1. 需要提示用户验证失败原因
    2. 需要将验证不通过字段清空
2. **个人说明**: 如果个人说明为空,不传这个字段到后端, 或者要求传递一个默认说明.
3. **接口升级**: 参数修改如下

    ```
    {
      "name": "name",
      "age": 22,
      "email": "qiu@126.com",
      "desc": "说明"
    }
    ```
