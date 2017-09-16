第一部分 编程风格
程序是写给人读的，只是偶尔让机器执行以下。--Donald Knuth
两个检查代码风格工具 JSLint JSHint

第一章 基本的格式化
1.1 缩进层级
制表符优缺点 (tab)
优点：1. 制表符和缩进层之间一一对应；2. IDE可配置制表符长度，想长就长，想短就短；
缺点：1. 各系统对缩进符解释不一
空格 （space）
两个空格，八个空格，或者四个空格（折中方案）
各个IDE中表现一致

1.2 语句结尾
省略分号：自动分号插入 (Automatic Semicolon Insertion, ASI)
规则难记，推荐不省略。

1.3 行的长度
     80个字符之内

1.4 换行
好的换发：在运算符之后换行，第二行追加两个缩进
callAFunction (document, callBack, "Say some thing...",
          parameters)

1.5 空行
     推荐：1. 在方法之前；2. 在方法中的局部变量和第一条语句之间；3. 在单行和多行注释之间；4. 方法内的逻辑片段之间。

1.6 命名
     小驼峰式camelCase
1.6.1 变量和函数

- 变量名用名词作前缀；
- 2. count、length、size为数量，name、title、message为字符串，i、j、k在循环中使用；

- 常见函数名前缀can, has, is, get, set

1.6.2 常量
     使用大写字母加下划线表示 MAX_COUNT
1.6.3 构造函数
     名字使用大写字母开始(Pascal Case) function Person(name) {}

1.7 直接量
1.7.1 字符串
     使用双引号使风格统一

var longString = "Here's the story, of a man " +     //使用加号链接多行，而非使用反斜杠连接。
                 "named Brady."
1.7.2 数字
     避免歧义，补全小数的整数位于小数位
     var number = 0.0
1.7.3 null
     什么时候使用null：把null作为一个占位符placeholder
1.7.4 undefined
     尽量避免使用undefined

// foo 未
var person;
console.log(typeof person);     //"undefined"
console.log(typeof foo);           //"undefined"
// 好的做法
var person = null;
console.log(person === null);     //true
1.7.5 对象直接量
     所有属性包于花括号内，并且保持缩进。

var book = {
     title: "Maintainable Javascript",
     year: 1992
};
1.7.6 数组
var colors = new Array("red", "green", "blue");     //不简洁
var colors = ["red", "green", "blue"];     //不简洁

第二章 注释
2.1 单行注释

- 独占一行，解释下一行代码，之前空一行；

- 代码行尾部注释，与代码隔一个缩进；

- 批量注释大量代码（编译器）。

2.2 多行注释
/*
 * 首行空出
 * 正文部分单星号开始
 * 尾行空出
 */

2.3 使用注释
     当代码不够清晰时添加注释（1. 难以理解的代码；2. 可能被误认为错误的代码；3. 浏览器特性hack），而代码不是很明了时不应添加。

2.4 注释文档
/**
对函数的解释
@method merge
@param {Object} 被合并的一个或多个对象
@return {Object} 一个新的合并后的对象
**/

第三章 语句和表达式
     不论块语句（block statement）包含多行还是单行代码，都应当总是使用花括号。
if (condition) {
     doSomething();
}
doSomethingElse();

3.1 花括号的对齐方式
// 推荐第一种，继承于Java
if (condition) {
     // work
} else {
     // other work
}

// 不推荐第二种，继承于C#，可能引起错误的分号自动插入
if (condition)
{
     // work
}
else
{
     // other work
}

3.2 块语句间隔
if(condition){}     // 过于紧凑
if (condition) {}     // 合适
if ( condition ) {}     // 源于jQuery

3.3 switch
switch (condition) {

     case "first":
     case "second":
          // code
          break;

     case "third":
          // code

     default:
          // code
}

3.4 with
with (book) {
     message += title;
     message += year;
}
// with在严格模式中禁止使用。尽量避免使用with

3.5 for
     尽可能避免使用continue，使用if语句检测；但也没理由完全禁止。

3.6 for-in
     for-in不仅遍历对象，还会遍历从原型继承来的属性。
     一个好的做法是使用hasOwnProperty()方法来为for-in循环做过滤。
var prop;

for (prop in object) {
     if (object.hasOwnProperty(prop)) {
          // work
     }
}
     尽量不要用for-in遍历数组。

第四章 变量、函数和运算符
4.1 声明变量
因为存在hoist，变量声明一定要提前在function头部。
当变量之间有关联性时，才允许使用单var语句。
为了保持成本最低，推荐合并var语句，代码更短、下载更快。

4.2 函数声明
函数任然会被hoist。推荐总是先声明，再使用。
避免在语句块内声明函数：
// 各浏览器表现各不相同，大多数情况下：不论condition的值，只有后一次声明的函数执行。
if (condition) {
     function doSomething() {
          // work
     }
} else {
     function doSomething() {
          // work
     }
}

4.3 函数调用间隔
function(item);

4.4 立即调用函数IIFE
用括号把立即执行函数包裹，便于区分出。
var value = (function() {
     // work

     return message;
}());

4.5 严格模式
可以用于全局作用域，但不推荐：当多个文件合并时，一个文件定义了全局严格模式，其他所有代码都受影响。
如果想避免影响其他文件，又少些多行代码，可以使用IIFE：
(function() {
     "use strict";
     // code...
})();

4.6 相等
强制类型转换机制（type correction）
使用===与!==
4.6.1 eval()
严格限制eval()的使用，但可用于回调中解析JSON。
4.6.2 原始包装类型
var name = "Harry Liu";
// 调用toUpperCase()时解释器会创建一个String的临时新实例，但会立马销毁。
console.log(name.toUpperCase());

var name = "Harry Liu";
// 创建了临时实例。
name.author = true;
// 随后实例立马被销毁。
console.log(name.author);     // undefined