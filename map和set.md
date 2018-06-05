{}是js的默认对象，但是里面键值对的key只能是字符串，这个设计就不是很合理
Map是一组键值对的结构，具有极快的查找速度。
```
//静态初始化数据
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
//动态初始化数据
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```
一个key只能对应一个value，所以，多次对一个key放入value，后者会覆盖前者
Set只能添加一个值，自动过滤重复的值，也是2种初始化方式
```
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```
add(key)方法可以添加元素到Set中，可以重复添加，但不会有效果；delete(key)方法可以删除元素；
```
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}

s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}

var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```
