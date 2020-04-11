# BootStrap

- 概念：一个前端开发的框架
  - 框架：一个半成品软件，开发人员可以在框架基础上，再进行开发，简化编码。
  - 好处:
    - 定义了很多的css样式和js插件。我们开发人员直接可以使用这些样式和插件得到丰富的页面效果。
    - **响应式布局。**
      - 同一套页面可以兼容不同分辨率的设备。

- 快速入门
  - 在项目中将这三个文件复制
  - 创建htlm页面，引入必要的资源文件

- 模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="./css/bootstrap-theme.min.css" rel="stylesheet">

</head>
<body>
<h1>你好，世界！</h1>

<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="./js/jquery-3.2.1.min.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="./css/bootstrap.min.css"></script>
</body>
</html>
```

## 响应式布局

- 同一套页面可以兼容不同分辨率的设备
- 实现：依赖于栅格系统：将一行平均分成12个格子，可以指定元素占几个格子

- 步骤:
  - 定义容器。相当于之前的table
    - 容器分类：
      - container：固定宽度，在每种设备上的宽度是不一样的
      - container-fluid：每一种设备都是100%宽度
  - 定义行。相当于之前的tr
  - 定义元素。相当于之前的td 指定该元素在不同的设备上，所占格子数目。 样式:：col-设备代号-格子数目
    - 设备代号
      - xs：超小屏幕 手机(<768px)：col-xs-12
      - sm：小屏幕 平板(>=768px)
      - md：中等屏幕 桌面显示器(>=992px)
      - lg：大屏幕 大桌面显示器(>=1200px)
  - 注意事项：
    - 一行中如果格子数目超过12，则超出部分自动换行
    - 定义的栅格类属性可以向上兼容。栅格类适用于与屏幕宽度大于或等于分界点大小的设备。
    - 如果真实设备的宽度小于了设置的 栅格类属性的设备代码的最小值，默认会一个元素占满一整行。

## CSS样式和JS插件

#### 全局CSS样式

- 全局CSS样式
  - 按钮 ```class="btn btn-default"```
  - 图片```class="img-responsive"```图片在任意尺寸都占100%  ```class="img-rounded"```图片的形状
  - 表格```<table class="table table-bordered table-hover">```
  - 表单
- 组件
  - 导航条
  - 分页条

#### JS插件

- 轮播图 Carousel