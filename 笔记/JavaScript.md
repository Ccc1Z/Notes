# JavaScript

## 概念

- JavaScript是一门客户端脚本语言
  - 运行在客户端浏览器中的.每一个浏览器都有JavaScript的解析引擎
  - 脚本语言:不需要编译,直接就可以被浏览器解析执行



## 功能:

- 可以来增强用户和html页面的交互过程,可以控制html元素,让页面有一些动态的效果,增强用户的体验.

- JavaScript发展史:
  - 1992年,Nombase公司,开发出第一门客户端脚本语言,专门用于表单的校验。命名为:C--,后更名为:ScriptEase
  - 1995年,Netscape(网景)公司,开发了一门客户端脚本预压:LiveScript。后来青睐SUN公司专家,修改LiveScript,命名为JavaScrpit.
  - 1996年,微软抄袭JavaScript开发出JScript语言
  - 1997年,ECMA(欧洲计算机制造商协会),ECMAScript,就是所有客户端脚本语言的标准.

**JavaScript = ECMAScript + JavaScript自己特有的东西(BOM+DOM)**

## ECMAScript

- 基本语法

  - 与html的结合方式

    - 1.内部JS:

      - 定义<script>,标签体内容就是js代码

      ```html
      <script>
          alert("Hello");
      </script>
      ```

    - 2.外部JS:

      - 定义<script>文件,通过src属性引入外部的js文件

      ```html
      <script src="js/a.js"></script>
      ```

    - 注意:

      [^1]:script>可以定义在html页面的任何地方,但是定义的位置会影响执行顺序.
      [^2]:<script>可以定义多个

  - 注释：单行注释与多行注释与Java一样

  - 数据类型

    1.原始数据类型(基本数据类型):

    - number：表示数字。整数/小数/NaN(not a number 一个不是数组的数字类型)
    - string：字符串。字符/字符串 "abc"  "a"  'abc'
    - boolean：true/false
    - null：一个对象为空的占位符(typeof null时会返回一个Object,但从技术上来说它仍是一个原始数据类型) 
    - undefined：未定义。如果一个变量没有给初始化值，则会被默认赋值为undefined

    2.引用数据类型:对象

  - 变量

    - 什么是变量：`一块存储数据的内存空间`

    - Java语言是强类型语言，而JavaScript是弱类型的语言

      `强类型：int a = 3 申请空间时需要规定类型`

      `弱类型: var a = 3 申请空间时不需要规定类型`

    - 语法：

      var 变量名 = 初始化值;
      
      typeof(变量名):输出变量类型

  - 运算符

    - 一元运算符:只有一个运算数的运算符
    - 算术运算符
    - 赋值运算符: ===全等于符号.在比较之前,先判断类型,如果类型不一样则直接返回false
    - 比较运算符
    - 逻辑运算符
      - number转boolean:0或NaN为false,非0为true
      - string转boolean:除了空字符串(""),其他都是true
      - null和undefined:都是false
      - 所有对象都是true
    - 三元运算符

  - Js特殊语法

    - 语句以;结尾,如果一行只有一条语句则;可以省略(不建议)
    - 变量的定义使用var关键字,也可以不使用(不建议)
      - 用var:定义的变量是局部变量
      - 不用var:定义的变量是全局变量

  - 流程控制语句

    - if...else...

    - switch

      - 注意:在java中,switch语句可以接收的数据类型:byte int short char 枚举(1.5) String(1.7)
      - 在Js中,switch语句可以接收任意的原始数据类型

    - while

    - do...while

    - for

    - 99乘法表练习:

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>99乘法表</title>
          <style>
              td{
                  border: 1px solid;
              }
          </style>
          <script>
              document.write("<table  align='center'>");
              for(var i = 1 ; i <= 9 ;i++){
                  document.write("<tr>")
                  for (var j = 1; j <= i ; j++) {
                      document.write("<td>");
                      document.write(i + "*" + j + " = " + (i*j)+"&nbsp;&nbsp;&nbsp");
                      document.write("</td>");
                  }
                  document.write("<br>");
                  document.write("<tr>");
              }
              document.write("</table>");
          </script>
      </head>
      <body>
      
      </body>
      </html>
      ```

      

#### 基本对象

- Fuction对象:函数对象

```
Function:函数(方法)对象
1.创建:
    (1). var fun = new Function(形式参数列表,方法体);
    (2). function 方法名称(形式参数列表){方法体}
    (3). var 方法名 = function(形式参数列表){方法体}
2.方法:

3.属性
    length:形参的个数
4.特点
    (1).方法定义时:形参的类型不用写
    (2).方法时一个对象,如果定义名称相同的方法,会覆盖
    (3).在JS中方法的调用只与方法的名称有关,和参数列表无关.
    (4).在方法声明中有一个隐藏的内置对象(数组),arguments,封装所有的实际参数
5.调用:方法名称(实际参数列表)
```

```html
使用内置对象(数组)arguments实现任意数相加       
function add(){
            var sum = 0;
            for(var i = 0 ; i < arguments.length; i++){
                sum += arguments[i];
            }
            return sum;
        }
        var sum = add(1,3,5,3,1);
        alert(sum)
```

- Array对象

```
Array:数组对象
    1.创建:
        (1):var arr = new Array(元素列表);
        (2):var arr = new Array(默认长度);
        (3):var arr = [元素列表]
    2.方法:
        join(参数):将数组中的元素按照指定的分隔符拼接为字符串
        push():向数组的末尾添加一个或更多元素,并返回新的长度
    3.属性:
        length:表示数组的长度
    4.特点:
        (1).js中,数组元素的类型可变的
        (2).js中,数组的长度时可变的
```

- Date对象

```
Date：日期对象
    1.创建
        var date = new Date()
    2.方法
        toLocaleString():返回当前date对象对应的时间
        getTime():获取毫秒值。返回当前日期对象描述的时间到1970-01-01-0点的毫秒值差
```

- Math对象

```
Math：数学对象
    1.创建:
        * 特点:Math对象不用创建,直接使用。Math.方法名
    2.方法:
        random():产生一个[0-1)之间的随机数
        ceil(x):向上去整
        floor(x):向下取整
        round(x):四舍五入
    3.属性:
        PI
```

- RegExp:正则表达式对象

  - 正则表达式:定义字符串的组成规则

    - 单个字符:[ ] 如:[a]:单个字符a,[ab]:a或者b,[a-z]:a到z的其中一个字符

      - 特殊符号代表特殊含义的单个字符:\d:单个数字字符[0-9];\w:单个单词字符[a-zA-Z0-9_]

      - 量词符号:

        *:表示出现0次或多次

        ?:表示出现0次或1次

        +:表示出现1次或多次

        \w*:该字符串由单个单词构成 且出现0次或多次

        {m,n}:表示m<=数量<=n。

        ​	m如果缺省:{,n}:表示最多n次

        ​	n如果缺省:{m,}:表示最少m次

      - 开始结束符号

        - ^:开始
        - $:结束

- 正则对象:

  ```
  正则表达式对象:
      1.创建
          (1):var reg = new RegExp("正则表达式");
          (2):var reg = /正则表达式/;
      2.方法
          test(参数):验证指定字符串是否符合正则定义的规范
          
          
  ```

  ```
  var reg1 = new RegExp("^\\w{6,12}$");
  var reg2 = /\\w{6,12}/;
  
  var userName = "zhangsan";
  var flag = reg2.test(userName);
  alert(flag);
  ```

- Global对象:
  - 特点:全局对象,这个Global中封装的方法不需要对象就可以直接调用。方法名()
  - URL编码:
  - 方法:
    - encodeURI():url编码
    - decodeURI():url解码
    - encodeURIComponent():url编码,编码的字符更多
    - encodeURIComponent():url解码
    - parseInt():将字符串转为数字
      - 逐一判断每一个字符是否时数字,指导不是数字为止,将前面的字符全部转换为数字
    - isNaN():判断一个值是不是NaN.NaN六亲不认,连自己都不认,NaN参与的==比较全部为false
    - eval():将JavaScript字符串,并把它作为脚本代码来执行
      - var jscode = "alert(123)";
      - eval(jscode):会将代码执行

#### BOM

- 概念:Broswer Object Model 浏览器对象模型

  - 将浏览器的各个组成部分封装成对象
    - 浏览器本身是一个对象 Navigator对象
    - Window浏览器窗口对象
      - 地址栏 Location对象
      - 历史记录 History对象
      - DOM对象 Document对象
    - 显示器屏幕 Screen对象

- Window对象

  - ```
    /**
     * Window:窗口对象
     *  1.创建
     *  2.方法
     *      (1).与弹出框有关的方法
     *          alert() 显示带有一段消息和一个确认按钮的警告框。
     *          confirm() 显示带有一段消息以及确认按钮和取消按钮的对话框。
     *              如果用户点击确定按钮,则方法返回true
     *              如果用户点击取消按钮,则返回false
     *          prompt() 显示可提示用户输入的对话框。
     *      (2).与打开关闭相关等方法:
     *          close() 关闭浏览器窗口。(默认关闭当前窗口)
     *              谁调用close()就关闭谁
     *          open() 打开一个新的浏览器窗口或查找一个已命名的窗口。
     *              返回新的window对象
     *      (3).与定时器有关的方法
     *          setTimeout() 在指定的毫秒数后调用函数或计算表达式。
     *              参数:
     *                  1.js代码或者方法对象
     *                  2.毫秒值
     *				返回值:唯一标识,用于取消定时器
     *          clearTimeout() 取消由 setTimeout() 方法设置的 timeout。
     *          setInterval() 按照指定的周期（以毫秒计）来调用函数或计算表达式。
     *          clearInterval() 取消由 setInterval() 设置的 timeout。
     *  3.属性
    *      (1).获取其他BOM对象
    *          history
    *          location
    *          Navigator
    *          Screen
    *      (2).获取DOM对象
    *          document
     *  4.特点
     *      Window对象不需要创建可以直接使用window来使用。window.方法()
     *      Window引用可以省略 。 方法名()直接调用
     */
    ```

- Location 地址栏对象：包含有关当前URL的信息

  - 创建(获取)
    - window.location

  - 方法
    - reload() 重新加载当前文档(刷新)

  - 属性
    - 设置或返回完整的URL

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>自动跳转首页</title>
    <style>
        p{
            text-align: center;
        }
        span{
            color: red;
        }
    </style>
</head>
<body>
    <p id = 'p1'>
        <span id = "time">5</span>秒之后,自动跳转到首页...
    </p>

    <script>
        /**
         * 效果5秒之后自动跳转到首页
         *  1.显示页面效果 <p>
         *  2.倒计时读秒效果实现 定时器
         *      2.1 定义一个方法,获取span标签,修改span标签体的内容
         *      2.2 定义一个定时器,1秒执行一次该方法
         *  3.在方法中判断事件如果<=0,则跳转到首页
         *
         */
        var second = 5;
        vat time;
        function showTime(){
            second--;
            time = document.getElementById('time');
            time.innerHTML = second + "";
            if(second <= 0){
                location.href = "https://www.baidu.com";
            }
        }
        setInterval(showTime,1000);
    </script>
</body>
</html>
```

- history对象

  - History对象包含用户(在浏览器窗口中)访问过的URL

    - 方法

    - | [back()](https://www.w3school.com.cn/jsref/met_his_back.asp) | 加载 history 列表中的前一个 URL。   |
      | ------------------------------------------------------------ | ----------------------------------- |
      | [forward()](https://www.w3school.com.cn/jsref/met_his_forward.asp) | 加载 history 列表中的下一个 URL。   |
      | [go()](https://www.w3school.com.cn/jsref/met_his_go.asp)     | 加载 history 列表中的某个具体页面。 |

    - go(参数)

      - 正数:前进几个历史记录
      - 负数:后退几个历史记录

    - 属性

      - length:返回当前窗口历史列表中的URL记录

#### DOM

- 概念:Document Object Model 文档对象模型
  - 将标记语言文档的各个组成部分,封装为对象.可以使用这些对象,对标记语言进行CRUD的动态操作。
  - DOM将HTML文档表达为树结构
  - ![htmlDOM树](D:\JAVA基础\htmlDOM树.PNG)

- 功能:控制html文档的内容
- 获取页面标签(元素)对象:Element对象
  - document.getElementById("id值"):通过元素的id获取元素的对象

- 操作Element对象:
  - 修改属性值:
    - (1):明确获取的对象时哪一个?
    - (2):查看API文档,查找其中由哪些属性可以设置
  - 修改标签体内容
    - 属性:innerHTML

##### 核心DOM模型 https://www.w3school.com.cn/xmldom/dom_document.asp

- Document:文档对象
  - 创建(获取)：在html dom模型中可以使用window对象来获取(window.document)
  - 方法
    - 获取Element对象:
      - getElementById():根据id属性值来获取元素对象。Id属性值一般唯一
      - getElementsByTagName():根据元素名称获取元素对象们。返回值是一个数组
      - getElementsByClassName():根据Class属性值获取元素对象们。返回值是一个数组。
      - getElementsByName():根据name属性值获取元素对象们。返回值是一个数组
    - 创建其他DOM对象:
      - createAttribute(name)
      - createComment()
      - createElement()
      - createTextNode()
  - 属性
- Element:元素对象
  - 获取:通过document对象来获取和创建
  - 方法:
    - removeAttribute():删除属性
    - setAttribute():设置属性
- Node:节点对象,其他5个的父对象（节点对象代表文档树种的一个节点）
  - 节点对象可以是元素节点、属性节点、文本节点、或者也可以是"节点类型"那一节所介绍的任何一种节点
  - 特点：所有dom对象都可以被认为是一个节点
  - 方法：
    - CRUD dom树的方法:
      - lappendChild():向节点的子节点列表的结尾添加新的子节点。
      - removeChild():删除(并返回)当前节点的指定子节点
      - replaceChild():用新节点替换一个子节点
  - 属性:
    - parentNode 返回当前节点的父节点

##### 动态表格案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>动态表格</title>
    <style>
        table{
            border:1px solid;
            margin:auto;
            width:500px;
        }

        td,th{
            text-align: center;
            border:1px solid;
        }
        div{
            text-align: center;
            margin: 50px;
        }
    </style>
</head>
<body>
<div>
    <input type="text" id="id" placeholder="请输入编号">
    <input type="text" id="name" placeholder="请输入姓名">
    <input type="text" id="gender" placeholder="请输入性别">
    <input type="button" id="btn_add" value="添加">
</div>

<table>
    <caption>学生信息表</caption>
    <tr>
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>操作</th>
    </tr>

    <tr>
        <td>3</td>
        <td>岳不群</td>
        <td>?</td>
        <td><a href="javascript:void(0);" onclick="delTr(this)">删除</a></td>
    </tr>
</table>

<script>
    /**
     * 1.添加
     *      1).给添加按钮绑定单击事件。
     *      2).获取文本框的内容。
     *      3).创建tr，td，设置td的文本为文本框的内容。
     *      4).创建tr
     *      5).将td添加到tr种
     *      6).获取table,将tr添加到table种
     *
     * 2.删除
     *      1).确定点击的是哪一个超链接
     *
     *      2).怎么删除
     *          removeChild():通过父节点删除子节点
     */
        //1.获取按钮
    document.getElementById("btn_add").onclick = function (ev) {
        //获取文本框的内容
        var id = document.getElementById('id').value;
        var name = document.getElementById('name').value;
        var gender = document.getElementById('gender').value;
        var a = document.getElementsByTagName('a')[0];
        //创建td,赋值td的标签体
        var td_id = document.createElement('td');
        var text_id = document.createTextNode(id);
        td_id.appendChild(text_id);

        var td_name = document.createElement('td');
        var text_name = document.createTextNode(name);
        td_name.appendChild(text_name);

        var td_gender = document.createElement('td');
        var text_gender = document.createTextNode(gender);
        td_gender.appendChild(text_gender);

        var td_a = document.createElement('td');
        var ele_a = document.createElement('a');
        ele_a.setAttribute('href',"javascript:void(0);");
        ele_a.setAttribute('onclick',"delTr(this)");
        var text_a = document.createTextNode("删除");
        ele_a.appendChild(text_a);
        td_a.appendChild(ele_a);
        //创建tr
        var tr = document.createElement("tr");
        //添加td到tr种
        tr.appendChild(td_id);
        tr.appendChild(td_name);
        tr.appendChild(td_gender);
        tr.appendChild(td_a);
        //获取table,将tr添加到table中
        var table = document.getElementsByTagName('table')[0];
        table.appendChild(tr);
        }
    //删除方法
    function delTr(obj){
        var table = obj.parentNode.parentNode.parentNode;
        var tr = obj.parentNode.parentNode;
        table.removeChild(tr);
    }
</script>
</body>
</html>
```

##### 使用innerHTML完成添加操作

```html
document.getElementById('btn_add').onclick = function(){
    var id = document.getElementById('id').value;
    var name = document.getElementById('name').value;
    var gender = document.getElementById('gender').value;

    //获取table
    var table = document.getElementsByTagName('table')[0];

    //追加一行
    table.innerHTML += "<tr>\n" +
        "        <td>"+id+"</td>\n" +
        "        <td>"+name+"</td>\n" +
        "        <td>"+gender+"</td>\n" +
        "        <td><a href=\"javascript:void(0);\" onclick=\"delTr(this)\">删除</a></td>\n" +
        "    </tr>"
}
```

#### HTML DOM

- 标签体的设置和获取:innerHTML

- 使用html元素对象的属性

- 设置元素样式

  - 使用元素的style属性来设置

    - ```html
      <script>
          var div = document.getElementById('div1');
          div.onclick = function (ev) {
              //修改样式方式1
              div.style.border ='1px solid red';
          }
      </script>
      ```

#### 事件(事件的监听机制)

- 概念:某些组件被执行了某些操作后,触发某些代码的执行
  - 事件:某些操作。如: 单击，双击。
  - 事件源:组件。如按钮 文本输入框 ...
  - 监听器：代码。
  - 注册监听：将事件，事件源，监听器绑定在一起。 当事件源上发生了某个事件，则触发某个监听器代码。

- 如何绑定事件

  - 直接在html标签上,指定事件的属性,属性值就是js代码
  - 通过js获取元素对象,指定事件属性,设置一个函数

- ```
  var light2 = document.getElementById("light2");
  light2.onclick = fun;
  ```

- 常见的事件
  - 点击事件
    - onclick ：当用户点击某个对象时调用的事件句柄。
    - doubleclick：双击事件。
  - 焦点事件
    - onblur：失去焦点。
      - 一般用于表单验证
    - onfocus：获得焦点。
  - 加载事件
    - onload：一张页面或一幅图像完成加载。
      - 一般给body或者window加载
  - 鼠标事件
    - onmouse开头-查文档
  - 键盘事件
    - onkey开头-查文档
  - 选择和改变
    - onchange 域的内容被改变
    - onselect 文本被选中
  - 表单事件
    - onsubmit:确认按钮被点击[非常重要]
      - 可以阻止表单的提交,多用于表单的校验
      - 方法返回false则表单被阻止提交
    - onreset：重置按钮被点击