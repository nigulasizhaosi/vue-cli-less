# vue-cli-less
在vue-cli 应用less教程
LESS 在 Vue-cli 中的安装
1， cd到vue-cli工程目录。
2，npm install less --save-dev
3,npm install less-loader --save-dev
在vue-cli中已经配置好了less 和 sass所以安装后就能使用了。
在组件中。Style标签加上 lang=’less’ 就可以用less的语法了。
比如
<style type="text/css" lang='less'>
	@color: #4D926F;
	a{
	   color: @color;
	}
</style>
然后让我们愉快的在组件里用 less吧。
一 通用的class样式
在之前写css文件的时候，或者说像bootstrap这样的css类库，通常会出现 
.btn{
Outline: None;
Border: 1px solid #ccc;
	...
}
这样的样式，然后将btn 添加input 节点里，这样做的好处是，复用css样式。
而在less中，我们可以将某一个style 值变成全局的。
比如说 @color : red; 注意变量名前面一定要加@符号，我之前忘了加@符号困扰了好久，总以为less没有安装好。
当然，我们在做个人网站的时候，经常由于找不到合适的配色而困扰，现在，我们可以写定一定的颜色变量，减少这些无用功的时间。
二 混合class和类函数式传参。

1，通用的btn样式的弱点。
 正如开篇所说，再如bootstrap这样的css库来说。
.btn{
Outline: None;
Border: 1px solid #ccc;
	...
}
像这样可以给每一个叫 btn 的按钮添加统一的样式。如果网站某天要换风格，那么改一下btn这个类名就可以了。但是如果要在某个按钮上添加不一样的样式，就需要给这个按钮继续增加class名。这样显得极不优雅。而less 动态的css语言对于这种情况就有对应的解决方法。
2，less的解决方式
首先创建一个通用的btn类变量。
.btn(@radious:5px){
  border-radius: @radious;
  border:@radious solid #ccc;
}
这里的类名后面跟了一个@radious变量，跟上篇的全局变量不同的是，这相当于类函数的局部变量。这个通用的类名就可以被其他类进行嵌套或称之为调用。
#input{
  .btn;
}
像这样input节点调用了btn类，也继承了所有的样式，当改节点样式需要修改时，可直接在.btn后面添加属性覆盖继承的样式。

三 可运算的属性
Less的css属性大多都可以进行加减乘除的运算，例子如下。

@width200:200px;
@height200:200px;
.div{
  width: @width200+200;
  height: @height200+200;
  border:1px solid #ccc;
}
这给了我们写css样式更大的灵活性。
四 条件语句
假设我们现在有一个需求，要让类名为div的元素大于50像素的时候变成红色背景，小于50像素的时候变成黑色背景。如果用css实现的话不太好实现。可能需要些两个类名。
.less50 和 .more50
那如果需求变了，50像素变成100像素，同样的效果，我们可能就傻眼了，需要去改每一个类名。
如果过了几天，产品经理又觉得50像素正好一点。我们就欲哭无泪了。对于这种情况，less引入了条件语句。就按照上面的案例写例子吧。
.less50(@px) when (@px<50){
  background:red;
}
.less50(@px) when (@px>=50){
  background: black;
}
@width200:200px;
@height200:200px;
.div{
  width: @width200+200;
  height: @height200+200;
  border:1px solid #ccc;
  .less50(@width200);
}
至此，如果需要改变判断条件，我们只需要修改less50，以及类名为div内部的less50变量即可。
当然还有 与 或 非的逻辑写法
或
.less50(@width,@height) when (@width>50) or (@height>50){
  background:red;
} 
与
.less50(@width,@height) when (@width>50) and(@height>50){
  background:red;
} 
非
.less50(@width,@height) when not (@height>50){
  background:red;
} 



我希望你能在编辑器里把以上再写一遍。并且改变div的长度或宽度来看看它做出的不同反应。
五 字符串注入
  平时我们在开发的时候，经常需要把静态资源的相对路径改为绝对路径。如果工程小，尚可一个一个改，但是如果工程大，那工作量就不止一点了。当然，一些编辑器给我们方便的功能，比如我平时开发用sublime，使用快捷键 ctrl+F  点击 find all，然后就可以批量修改路径。这样做也行，但less提供了另一种解决方案。
  比如在css文件顶部配置好路径
@imgURL : 'http://img4.imgtn.bdimg.com/';
.img{
  width: 200px;
  height: 200px;
  background:url('@{imgURL}it/u=2611079001,3896435225&fm=26&gp=0.jpg');
}
通过在字符串里 注入 @{imgURL} 该变量，则可将绝对路径注入到图片相对路径的前面。

六 JavaScript 表达式
在css里面写JavaScript表达式，这简直颠覆了我对css的认识。不过不要紧，我们要时刻知道，less只是css的预编译工具。尽管它可以写的天花乱坠，但按一下F12，依然是我们熟悉的css样式。它只是方便我们编写css，最终的结果总是不变得。
@upperStr: `"hello".toUpperCase()`;
像这样用 ` 包裹表达式。
.div:after{
  content:@upperStr;
}
再在伪类标签里注入该变量。即可看到变大写的HELLO。
