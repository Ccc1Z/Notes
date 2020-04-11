# XML

## 概念

- Extensible Markup Language 可扩展标记语言
- 可扩展：标签都是自定义的

## 功能

- 存储数据
  - 配置文件
  - 在网络中传输(xml是纯文本的，可以跨语言跨平台)

## 与html的区别

- xml标签都是自定义的，html标签是预定义
- xml的语法严格，html语法松散
- xml是存储数据，html是展示数据

## 快速入门

### 基本语法

- xml文档的后缀名 .xml
- xml第一行必须定义为文档声明
- xml文档中有且仅有一个跟标签
- 属性值必须使用引号(单双都可)引起来
- 标签必须正确关闭
- xml标签名称区分大小写

```xml
<?xml version='1.0' ?>

<users>
	<user id = "1">
		<name>zhangsan</name>
		<age>23</age>
		<gender>male</gender>
	</user>	

		<user id = "2">
		<name>lisi</name>
		<age>25</age>
		<gender>female</gender>
	</user>	



</users>
```

## 组成部分

- 文档声明
  - 格式：```<?xml 属性列表?>```
  - 属性列表：
    - version：版本号，必须的属性 一般用1.0
    - encoding：编码方式 告知解析引擎当前文档使用的 字符集，默认值：ISO-8859-1
    - standalone：是否独立

- 指令(了解)：结合css的
- 标签：标签名称自定义的
  - 规则：
    - 数字或标点符号不能开头
    - 不能以xml开头
    - 不能包含空格
- 属性：
  - id属性值唯一
- 文本
  - CDATA区：在该区域中的数据会被原样展示
  - 格式：```<![CDATA[要展示的数据]]>```

## 注意

- 谁编写xml?--用户，软件使用者

- 谁解析xml?--软件

## 约束

- 规定xml文档的书写规则
  - 作为框架的使用者：
    - 能够在xml中引入约束文档
    - 能够简单的读懂约束文档
- 分类：
  - DTD：一种简单的约束技术
  - Schema：一种复杂的约束技术
- DTD：
  - 引入dtd文档到xml中
    - 内部dtd：将约束规则定义在xml文档中
    - 外部dtd：将约束规则定义在外部的dtd文件中
      - 本地：```<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">```
      - 网络```<!DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL">```

- Schema

### 解析

- 操作xml文档，将文档中的数据读取到内存中
  - 操作xml文档
    - 解析(读取)：将文档中的数据读取到内存中
    - 写入：将内存中的数据保存到xml文档中。持久化打的存储
- 解析xml的方式：
  - DOM：将标记语言文档一次性加载进 内存，在内存中形成一颗dom树
    - 优点：操作方便，可以对文档进行CRUD的所有操作
    - 缺点：dom树结构太占内存
  - SAX：逐行读取，基于事件驱动的
    - 读一行进内存，指针下移然后释放已读取的行并将当前行放进内存。
    - 缺点是只能读取不能CRUD
  - 服务器端一般使用DOM思想，移动端使用SAX思想。
- xml常见的解析器
  - JAXP：sun公司提供的解析器，支持dom和sam两种思想。
  - DOM4J：一款非常优秀的解析器。
  - JSOUP：一款java的html解析器，可以直接解析某个URL地址、HTML文本内容
  - PULL：Android操作系统内置的解析器，sax方式。

## Jsoup

- 快速入门
  - 导入相关jar包
  - 获取Document对象
  - 获取对应的标签Element对象
  - 获取数据

