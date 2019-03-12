## 字符串操作

返回指定位置的字符

```js
var str = "abc";
console.log(str.charAt(0)); //a
```

返回在指定的位置的字符的 Unicode 编码。

```js
var str = "abc";
console.log(str.charCodeAt(1)); //98
```

字符串切割、分割为数组

```js
// 第一种，使用slice():
var myStr = "I,love,you,Do,you,love,me";
var subStr = myStr.slice(1, 5); //",lov"

// 第二种，使用substring():
var subStr = myStr.substring(1, 5); //",lov"

// 第三种，使用substr():
var subStr = myStr.substr(1, 5); //",love"
// 与第一种和第二种不同的是，substr()第二个参数代表截取的字符串最大长度，如上结果所示。

// 分割为数组
var substrArray = myStr.split(","); // ["I", "Love", "You", "Do", "you", "love", "me"];
var arrayLimited = myStr.split(",", 3); // ["I", "Love", "You"];
```

字符串替换

```js
// 单单查到字符串应该还不会停止，一般题目都还经常会遇到让你查到并替换为你自己的字符串，例如：
var myStr = "I,love,you,Do,you,love,me";
var replacedStr = myStr.replace("love", "hate"); //"I,hate,you,Do,you,love,me"

// 默认只替换第一次查找到的，想要全局替换，需要置上正则全局标识，如：
var myStr = "I,love,you,Do,you,love,me";
var replacedStr = myStr.replace(/love/g, "hate"); //"I,hate,you,Do,you,hate,me"
```

扩展 String 方法，replaceAll 全部替换

```js
let s = "a/b/c/d";
String.prototype.replaceAll = function(FindText, RepText) {
    let regExp = new RegExp(FindText, "g");
    return this.replace(regExp, RepText);
};
s = s.replaceAll("/", "-");
console.log(s);
// 输出：a-b-c-d
```

截取文件格式

```js
// slice 和 lastIndexOf 的实际应用
var file = "英文客户端.一带一路.CMS.原型及需求_v1.1_20190226.txt";
var fileType = file.slice(file.lastIndexOf(".") + 1, file.length); // txt
```

大小写转换

```js
var myStr = "I,love,you,Do,you,love,me";
var lowCaseStr = myStr.toLowerCase(); //"i,love,you,do,you,love,me";
var upCaseStr = myStr.toUpperCase(); //"I,LOVE,YOU,DO,YOU,LOVE,ME"
```

获取 URL 中的参数

```js
function getUrlParam(name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    var r = window.location.search.substr(1).match(reg);
    if (r != null) return decodeURI(r[2]);
    return null; //返回参数值
}
```

## 数组操作

判断是否为数组 （IE8 及以下不支持，其它主流浏览器都支持）[查看兼容性详情](https://caniuse.com/#search=isArray)

```js
// 下面的函数调用都返回 true
Array.isArray([]);
Array.isArray([1]);
Array.isArray(new Array());
// 下面的函数调用都返回 false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(17);
Array.isArray("Array");
Array.isArray(true);
Array.isArray(false);
Array.isArray({ __proto__: Array.prototype });
```

数组元素的添加

```js
arrayObj.push(item1, item2, item3); // 将一个或多个新元素添加到数组结尾，并返回数组新长度
arrayObj.unshift(item1, item2, item3); // 将一个或多个新元素添加到数组开始，数组中的元素自动后移，返回数组新长度
arrayObject.splice(index,howmany,item1,.....,itemX); // splice() 方法可删除从 index 处开始的零个或多个元素，并且用参数列表中声明的一个或多个值来替换那些被删除的元素。
```

数组元素的删除

```js
arrayObj.pop(); //移除最后一个元素并返回该元素值
arrayObj.shift(); //移除最前一个元素并返回该元素值，数组中元素自动前移
arrayObj.splice(deletePos, deleteCount); //删除从指定位置deletePos开始的指定数量deleteCount的元素，数组形式返回所移除的元素
```

数组的截取和合并

```js
arrayObj.slice(start, [end]); //以数组的形式返回数组的一部分，注意不包括 end 对应的元素，如果省略 end 将复制 start 之后的所有元素
arrayObj.concat([item1[, item2[, . . . [,itemN]]]]); //将多个数组（也可以是字符串，或者是数组和字符串的混合）连接为一个数组，返回连接好的新的数组
```

数组的拷贝

```js
arrayObj.slice(0); //返回数组的拷贝数组，注意是一个新的数组，不是指向
arrayObj.concat(); //返回数组的拷贝数组，注意是一个新的数组，不是指向
```

数组元素的排序

```js
arrayObj.reverse(); //反转元素（最前的排到最后、最后的排到最前），返回数组地址
arrayObj.sort(); //对数组元素排序，返回数组地址
```

数组元素的排序

```js
arrayObj.join(separator); //返回字符串，这个字符串将数组的每一个元素值连接在一起，中间用 separator 隔开。

var arr = ["A", "B", "C", 1, 2, 3];
arr.join("-"); // 'A-B-C-1-2-3'

// toLocaleString 、toString：可以看作是join的特殊用法，不常用;
// 如果Array的元素不是字符串，将自动转换为字符串后再连接。
```

`valueOf` 与 String 类似，Array 也可以通过 indexOf()来搜索一个指定的元素的位置：

```js
var arr = [10, 20, "30", "xyz"];
arr.indexOf(10); // 元素10的索引为0
arr.indexOf(30); // 元素30没有找到，返回-1
arr.indexOf("30"); // 元素'30'的索引为2
```

`lastIndexOf` 返回在数组中搜索到的与给定参数相等的元素的最后（最大）索引。

```js
var arr = [1, 2, 3, 4, 2, 6, 2, 7];
arr.lastIndexOf(2); // 6
```

## 对象操作

Object.assign() [查看兼容性详情](http://kangax.github.io/compat-table/es6/#test-Object_static_methods_Object.assign)

!>注意，该方法是 ES6 中合并对象用的，IE 和一些低版本浏览器目前不支持该方法。在没有 babel 转义的情况下请慎用。

```js
// Object.assign()
// 1、可以用作对象的复制
var obj = { a: 1 };
var copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }

// 2、可以用作对象的合并
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };

var obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1); // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。

// 上面我们看到，目标对象o1自身也发生了改变。假如我们不想让o1改变，我们可以把三个对象合并到一个空的对象中，操作如下：
var obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1); // { a: 1 }
```

Object.keys()

?> 这个方法会返回一个由给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致 （两者的主要区别是 一个 for-in 循环还会枚举其原型链上的属性）。

```js
/* 类数组对象 */

var obj = { 0: "a", 1: "b", 2: "c" };
alert(Object.keys(obj));
// 弹出"0,1,2"

/* 具有随机键排序的数组类对象 */
var an_obj = { 100: "a", 2: "b", 7: "c" };
console.log(Object.keys(an_obj));
// console: ['2', '7', '100']
```

删除对象中某个值

```js
var o = {
    p: 1
};
delete o.p;
console.log(o); // {}
```

遍历对象

```js
var data = {
    '张三'：69,
    '李四'：72,
    '王五'：90,
    '二麻子'：88
}
for (var i in data) {
    console.dir(i); //输出姓名
    console.dir(data[i]); //输出分数
}
```

## Number 操作

基本知识

```js
Math.ceil(); //向上取整。

Math.floor(); //向下取整。

Math.round(); //四舍五入。

Math.random(); //0.0 ~ 1.0 之间的一个伪随机数。【包含0不包含1】 //比如0.8647578968666494

Math.ceil(Math.random() * 10); // 获取从1到10的随机整数 ，取0的概率极小。

Math.round(Math.random()); //可均衡获取0到1的随机整数。

Math.floor(Math.random() * 10); //可均衡获取0到9的随机整数。

Math.round(Math.random() * 10); //基本均衡获取0到10的随机整数，其中获取最小值0和最大值10的几率少一半。
```

把数字转换为字符串

```js
var a = new Number(2).toString(); // '2'
```

把数字转换为字符串，结果的小数点后有指定位数的数字

```js
var numObj = 3.1415926;
var newNum = numObj.toFixed(2); // 3.14
```

js 生成[n,m]的随机数

```js
//生成从minNum到maxNum的随机数
function randomNum(minNum, maxNum) {
    switch (arguments.length) {
        case 1:
            return parseInt(Math.random() * minNum + 1, 10);
            break;
        case 2:
            return parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10);
            break;
        default:
            return 0;
            break;
    }
}
```
