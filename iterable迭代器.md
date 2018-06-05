ES6标准引入了新的iterable类型，Array、Map和Set都属于iterable类型。具有iterable类型的集合可以通过新的for ... of循环来遍历。注意浏览器兼容
```
'use strict';
var a = ['A', 'B', 'C'];
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
var s = new Set(['A', 'B', 'C']);
for (var x of a) { // 遍历Array
    console.log(x); 
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
    // 1=x;2=y;3=z
}
for (var x of s) { // 遍历Set
    console.log(x);
}
```
和for..in区别？
for ... in循环由于历史遗留问题，它遍历的实际上是对象的属性名称。
for ... of循环则完全修复了这些问题，它只循环集合本身的元素。
```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    console.log(x); // '0', '1', '2', 'name'
}
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    console.log(x); // 'A', 'B', 'C'
}
```
为什么要引入新的for ... of循环？
答：函数式编程，iterable内置的forEach方法，它接收一个函数，每次迭代就自动回调该函数。
```
'use strict';
var a = ['A', 'B', 'C'];

a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
});
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);
});
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
})
//如果对某些参数不感兴趣，由于JavaScript的函数调用不要求参数必须一致，因此可以忽略它们。
var a = ['A', 'B', 'C'];
a.forEach(function (element) {
    console.log(element);
});
```
