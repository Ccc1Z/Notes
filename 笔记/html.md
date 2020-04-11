## 表单标签
* 表单的概念:用于采集用户输入数据的,用于和服务器进行交互

* 使用的标签:form  

```
属性:  
    1.action:指定提交数据的url  
    2.method:指定提交方式。一共有7种,常有get/post  
        get:  
            1.请求参数会在地址栏中显示,会封装到请求行中
            2.请求参数的长度(url)是有限制的
            3.不太安全
        post:  
            1.请求参数不会再地址栏中显示,会封装在请求体中
            2.请求参数的大小没有限制
            3.较为安全
            
表单中的属性想要被提交必须指定其name属性
```
* 表单项标签:
    * <input>:可以通过type属性值,改变元素展示的样式.
    * <lable>:指定输入项的文字描述信息
        * 注意:lable的for属性一般会和input的id属性值对应,如果对应了,则点击lable区域,会让input输入框获取焦点 

```
<input>的type属性:
1.text:文本输入框,默认值
    placeholder:指定输入框的提示信息,当输入框内容发送改变时,提示信息自动清楚(请输入用户名)
2.password:密码输入框
3.radio:单选框
    注意:
    (1)要想让多个单选框实现单选的效果,则多个单选框的name属性值必须一致.
    (2)一般会给每一个单选框提供value属性,指定其被选择后提交的值.
4.checkbox:复选框(注意事项同上)
5.file:文件选择框
radio和checkbox共有的属性:checked = "checked"->默认被选中

按钮:
submit:提交
button:普通按钮(与JS结合使用)
image:通过src属性指定图片路径,图片按钮,一点击会提交表单

```

* select:下拉列表

```
子元素:option指定列表项
```

```html
    省份:<select name="province">
            <option value="">请选择</option>
            <option value="1">北京</option>
            <option value="2">上海</option>
            <option value="3">广东</option>
            <option value="4">四川</option>
        </select>
```
* textarea:文本域

```
cols:指定列数->每一行有多少列数
rows:指定行数
```

```html
    自我描述:
        <textarea cols="20" rows="5" name="des"></textarea>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表单标签</title>
</head>
<body>
    <!--
    form:用于定义表单的。可以定义一个范围,这个范围代表采集用户数据的范围
    -->
<form action="#"method="get">
    <label for="username">用户名:</label><input type="text" name="username" placeholder="请输入用户名" id="username"><br>
    <label for="password">密码:</label><input type="password" name="password" placeholder="请输入密码" id="password"><br>
    性别:<input type="radio" name="gender" value="male" checked> 男
    <input type="radio" name="gender" value="female"> 女 <br>
    爱好:<input type="checkbox" name="hobby" value="code" checked> 编程
    <input type="checkbox" name="hobby" value="basketball"> 打篮球
    <input type="checkbox" name="hobby" value="game"> 打游戏<br>
    上传文件:<input type="file" name="file"><br>
    生日:<input type="date" name="birthday"><br>
    生日:<input type="datetime-local" name="birthday"><br>
    邮箱:<input type="email" name="email"><br>
    省份:<select name="province">
            <option value="">请选择</option>
            <option value="1">北京</option>
            <option value="2">上海</option>
            <option value="3">广东</option>
            <option value="4">四川</option>
        </select><br>
    自我描述:
        <textarea cols="20" rows="5" name="des"></textarea>

    <input type="submit"value="登录"><br>
    <input type="button"value="一个按钮"><br>
<!--    <input type="image"src="img/机器学习.PNG" value="图片按钮">-->
</form>
</body>
</html>
```
## 注册页面案例
分析:  
    1.一个table,9行,2列  
    2.<form>table</form>
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>案例1_注册页面(html)</title>
</head>
<body>
<!--定义表单-->
<form action="#" method="post">
    <table border="1" align="center" width="500">
        <tr>
            <td>
                <label for="username">用户名</label>
            </td>
            <td>
                <input type="text" name="username" id="username">
            </td>
        </tr>

        <tr>
            <td>
                <label for="password">密码</label>
            </td>
            <td>
                <input type="password" name="password" id="password">
            </td>
        </tr>

        <tr>
            <td>
                <label for="email">邮箱</label>
            </td>
            <td>
                <input type="email" name="email" id="email">
            </td>
        </tr>

        <tr>
            <td>
                <label for="name">姓名</label>
            </td>
            <td>
                <input type="text" name="name" id="name">
            </td>
        </tr>

        <tr>
            <td>
                <label for="tel">手机号</label>
            </td>
            <td>
                <input type="text" name="tel" id="tel">
            </td>
        </tr>

        <tr>
            <td>
                <label>性别</label>
            </td>
            <td>
                <input type="radio" name="gender" value="male"> 男
                <input type="radio" name="gender" value="female"> 女
            </td>
        </tr>

        <tr>
            <td>
                <label for="birthday">出生日期</label>
            </td>
            <td>
                <input type="date" name="birthday" id="birthday">
            </td>
        </tr>

        <tr>
            <td>
                <label for="checkcode">验证码</label>
            </td>
            <td>
                <input type="text" name="checkcode" id="checkcode">
                <img src="img/验证码图片.jpg">
            </td>
        </tr>

        <tr>
            <td colspan="2" align="center">
                <input type="submit" value="注册">
            </td>
        </tr>
        >


    </table>
        
</form>
</body>
</html>
```
## CSS(进行页面布局控制和页面美化)

- 1.概念:Cascading Style Sheets(层叠样式表)
    - 层叠:多个样式可以作用在同一个Html的元素(标签)上,同时生效.

- 2.好处:
    - (1):功能强大
    - (2):将内容展示和样式控制分离
        - 降低耦合度(解耦)
        - 让分工协作更容易
        - 提高开发效率

- 3.CSS的使用:CSS与html结合方式
    - (1)内联样式:在标签内使用style属性指定css代码(不推荐使用,还是耦合在一起)
    - 如```<div style="color: red">hello css</div>```
    - (2)内部样式:在head标签内,定义style标签,style标签的标签体内容就是css代码.如:```    <style>
        div{
           color: red;
        }
    </style>```(作用域是当前页面)
    - (3)外部样式:  
        * 1):定义css资源文件  
        * 2):在head标签内,定义link标签,引入外部的资源文件。
        * 如:```<link rel="stylesheet" href="css/a.css">```
    - 注意:
        - 1,2,3种方式,css的作用域越来越大
        - 1不常用,2和3常用

### CSS基本语法:
- 格式:
    - 选择器{
        属性名1:属性值1;
        属性名2:属性值2;
        ...
    }
    - 选择器:筛选具有相似特征的元素
    - 注意:每一对属性需要使用;隔开,最后一对属性可以不加分号.
    
- 选择器:具有相似特征的元素
    - 分类:
        - 1.基础选择器:  
            1).id选择器  
            2).元素选择器  
            3).类选择器  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        #div1{
            color: red;
        }
        div{
            color: green;
        }
        .class1{
            color: aqua;
        }
    </style>
</head>
<body>
<!--1).id选择器(优先级更高)
        选择具体的id属性值的元素.建议在一个html页面中id值唯一
        #id属性值{}
-->
<!--2).元素选择器(优先级最低)
        选择具有相同标签名称的元素
        标签名称{}
-->
<!--3).类选择器(优先级次高)
        选择具有相同的class属性值的元素
        .class属性值{}
-->
<div id="div1">Ccc1Z</div>
<div id="div2">菜籽</div>
<p class="class1">重庆大学</p>
</body>
</html>
```
- 扩展选择器
    - 1).选择所有元素:  
        
        - 语法:*{}
        
    - 2).并集选择器:
        
        - 语法:选择器1,选择器2{}
        
    - 3).子选择器:筛选选择器1元素下的选择器2
        
        - 语法:选择器1 选择器2{}
        
    - 4):父类选择器:筛选选择器2上的父元素选择器1
        
        - 语法:选择器1 > 选择器2{} 
        
    - 5):属性选择器:选择元素名称,属性名=属性值的元素(一般用于选择<input的标签>)
    
        - 语法:元素名称[属性名='属性值']{}
    
    - 6):伪类选择器:选择一些元素具有的状态
    
        - 语法:元素:状态{} 如:<a>
               * 状态:
                 									* link初始化的状态
                 									* visited:被访问过的状态
                 									* active:正在访问状态
                 									* hover:鼠标悬浮状态
    
        ```html
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>扩展选择器</title>
            <style>
                div p{
                    color:red;
                }
                div > p{
                    border:1px solid;
                }
                input[type='text']{
                    border:5px solid
                }
                a:link{
                    color: pink;
                }
                a:visited{
                    color: red;
                }
                a:active{
                    color: green;
                }
                a:hover{
                    color: black;
                }
            </style>
        </head>
        <body>
            <div>
                <p>Ccc1Z</p>
            </div>
            <p>菜籽</p>
        
            <input type="text"><br>
            <input type="password"><br>
            <a href="#">www.baidu.com</a>
        </body>
        </html>
        ```
    
    - 属性(看文档,了解即可)

