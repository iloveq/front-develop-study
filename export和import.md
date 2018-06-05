 ```
//export 变量
export const name = '微笑';
export const age = 18;

//部分导入
import { name } from './test'
console.log(name); //'微笑'

//全局导入
import * as Student from './test'
console.log(Student.name); //'微笑'
console.log(Student.age); //18

// ---------------------------- ---------------------------- ---------------------------- ----------------------------
//export function函数
export const sayName = function(name){
    console.log(name);
}

import { sayName } from './test'
sayName('微笑'); //'微笑' 
```
