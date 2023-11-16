# 面试题

1. 说一下 css 盒模型

html 的 dom 元素可以看成一个盒子，这个盒子就称为盒模型，该盒子分为四个部分

- margin
- border
- padding
- content

盒模型有两种类型

- 标准盒模型：content+ border+ padding + content
- ie 盒模型：margin + content（border+padding）

盒模型可以通过 box-sizing 去改变：content-box(标准盒模型)和 border-box(ie 盒模型)

2. css 的特性有哪些？

- 继承性：子类样式会继承父级样式
- 层叠性：css 特性可以被覆盖
- 优先级：通过不同方式设置的 css 具有不同的优先级

3. css 的优先级怎么划分

!important（无限大） > 行内样式（1000） > id（100） > 类/伪类/属性（10） > 标签/伪元素（1） > 通配符选择器（0）

4. 隐藏元素的方式有哪些？

- display:none; 元素消失，不占据空间;
- opacity: 0; 元素透明，占据空间;
- visibility: hidden; 元素消失，占据空间;

5. 重排和重绘制的区别？

- 重排发生在布局阶段：页面中布局有变动则会重新绘制，因此改变的 dom 元素的几何属性，元素的消失和隐藏，页面布局的变动，都会引起重排。
- 重绘发生了绘制阶段：根绝 dom 盒模型的位置、大小、和其他一些属性，浏览器进行页面绘制。

因为重排发生在前，因此重排肯定重绘，重绘不一定重排。

6. 浏览器渲染流程

- 主线程：Parse HTML（解析 html） -> Recaculate Style(样式计算) -> Layout(布局) -> Update Layer Tree（分层） -> paint（绘制）
- 合成线程：分块 -> 光栅化 -> 画（变形阶段）

7. css 的哪些属性可以被继承？

- 字体属性：font-size
- 文本属性：line-height
- 元素的可见性：visibility
- 表格布局属性:
- 列表的属性：
