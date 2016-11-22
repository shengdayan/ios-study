##js学习笔记

###1. client家族 可视区域
offsetWidth :盒子自己的宽度 width+padding+border
clientWidth: width + padding
scrollWidth ：取决于内容的宽高
###2.检测屏幕宽度（可视区域）
ie9及以上版本：
window.innerWidth
标准模式：
document.documentElement.clientWidth
怪异模式：
document.body.climentWidth

###3.window.onscroll  屏幕滚动事件
window.onresize 窗口改变事件 
window.screen.width返回的是电脑的分辨率  跟浏览器没有关系

###4.冒泡问题(父亲里包着儿子，当两者都有操作事件时，儿子执行完后在执行父亲的，一层一层的执行)
以下事件不能冒泡：blur load unload focus
阻止冒泡的方法： 
1> var event = event || window.event;
event.stopPropagation();
2> IE使用方法 event.cancelBubble = true;

###5.隐藏滚动条
document.body.style.overflow = "hidden";

###6.判断当前对象
event.target.id
event.srcElement.id(IE 678支持)

判断方法：
var  targetId = event.target ? event.target.id : event.scrElement.id;

###7.得到用户选中的内容
window.getSelection().toString() //获得的内容转化为字符串   
document.selection.creatRange().text;

###8.取整
Math.ceil(1.1)  向上取整  2
Math.floor(1.8)  向下取整  1
Math.round()    四舍五入

###9.toggleClass对设置或移除被选元素的一个或多个类进行切换,如果这个类就添加，如果有就删除，实现切换效果

###10.end结束当前遍历的筛选，并把匹配元素还原成当前状态

###11.legend 元素表示作为 legend 元素的父元素的 fieldset 元素的其余内容的标题（caption）。
legend 元素应作为 fieldset 元素的第一个子元素来使用。

###12.js的执行分为两个阶段，欲解析：把所有的函数定义提前，所有的变量声明提前，变量的赋值不提前；执行：从上到下执行，一些事件函数需要触发执行

###13.闭包：保存函数的私有空间，保存变量的作用域不被销毁

###14.js的函数作用域有全局作用域和函数作用域两种
