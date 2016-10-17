# wei-browserify-
Browserify 跑在浏览器上的Node程序
新建4个文件：

    multiply.js: 乘法计算
    square.js: 平方计算，依赖multiply.js
    index.js: node启动程序，调用square.js
    index.html: 用于显示的HTML网页

新建文件：multiply.js


~ vi multiply.js

module.exports = function (a, b) {
    console.log("js:multiply");
    return a * b;
};

新建文件：square.js


~ vi square.js

var multiply = require('./multiply');
module.exports = function (n) {
    console.log("js:square");
    return multiply(n, n);
};

新建文件：index.js


~ vi index.js

var square = require('./square');
console.log("js:index");
console.log(square(125));

在node环境中运行


~ node index.js
js:index
js:square
js:multiply
15625

用browserify编译index.js文件到bundle.js


~ browserify index.js > bundle.js

新建文件：index.html


<!DOCTYPE html>
<html>
<head>
<title>Browserify</title>
<style>input{width:50px;}</style>
</head>
<body>
<h1>Browserify</h1>
<script src="bundle.js"></script>
</body>
</html>

在index.html中，我们加载刚才生成的bundle.js文件。

在浏览器中预览效果：

在浏览器中模块化调用Nodejs函数

新建文件：

    multiply.js: 同上面的文件
    module.html: 用于显示的HTML网页

查看文件：multiply.js


~ vi multiply.js

module.exports = function (a, b) {
    console.log("js:multiply");
    return a * b;
};

用browserify编译multiply.js文件到bundle.js，作为模块

~ browserify -r ./multiply.js > bundle.js

新建文件：module.html


<!DOCTYPE html>
<html>
<head>
<title>Browserify</title>
<style>input{width:50px;}</style>
</head>
<body>
<form>
<p>
<input type="text" id="x" value="2"/> * <input type="text" id="y" value="3" /> = <span id="result"></span>
</p>
<input type="button" onclick="add()" value="M1"/>
<input type="button" onclick="add2()" value="M2"/>
</form>
<script src="bower_components/jquery/jquery.min.js"></script>
<script src="bundle.js"></script>
<script>
function add(){
var x = parseInt($('#x').val());
var y = parseInt($('#y').val());
console.log(x*y);
$('#result').text(x*y);
}

var m = require('./multiply.js');
function add2(){
var x = parseInt($('#x').val());
var y = parseInt($('#y').val());
console.log(m);
console.log(x*y);
$('#result').text(m(x,y));
}
</script>
</body>
</html>

在浏览器中打开：
