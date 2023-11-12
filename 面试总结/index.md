# 面试总结

## 招商银行-招行网络科技（杭州）

笔试：

1. 素质测试：一堆测性格的题目（20 分钟）
2. 专业笔试

> css 字体大小写转换属性：

text-transform 属性用于指定文本中的大写和小写字母。

- uppercase：将字母转为大写
- lowercase：将字母转为小写
- capitalize：将每个单词首字母转为大写

> 跨域的几种方式：

- 代理服务器：可以是 ngix 或者 node
- jsonp
- 后段设置 cors

> 拖动事件

拖动元素：

- ondrag: 鼠标拖动元素
- ondragstart: 拖动开始
- ondragend: 拖动结束

目标元素

- ondragenter: 拖动进入
- ondragover: 在里面
- ondrop: 拖动放下
- ondragleave: 拖动离开

> Date 的基本方法

```js
var date = new Date("2016-01-10");
var time = date.getTime(); //返回该date对象对应的毫秒数，与valueOf返回的结果相同
date.setTime(1); //以毫秒数设置日期，这常常会改变整个日期对象
var year = date.getFullYear(); //取得四位数的年份，如2016而非16
date.setFullYear(2012); //设置年份，传入的参数必须是四位数字
var month = date.getMonth(); //返回该date对象的月份（0-11）
date.setMonth(0); //设置月份，参数必须为0-11的数字
var day = date.getDate(); //返回该date对象月份中的天数（1-31）
date.setDate(11); //设置月份中的天数，参数必须为1-31之间的数字
var week = date.getDay(); //返回该date对象星期中的天数（0-6）
var hours = date.getHours(); //返回该date对象一天中的小时数（0-23），对应的有setHours
var minutes = date.getMinutes(); //返回日期中的分钟数（0到59），对应的有setMinutes
var seconds = date.getSeconds(); //返回日期中的秒数（0-59），对应的有setSeconds
```

三，获取当前时间对应的毫秒数

这常常用在监测一段代码运行了多长时间。
方法一：var start=Date.now();
方法二：var end=+new Date();
方法三：var end=new Date().getTime()

> 加粗标签

b 或者 strong

> 高亮标签

mark 标签
