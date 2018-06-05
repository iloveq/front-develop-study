######Object.is()
比较两个值是否严格相等（===）
######Object.assign()
Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
同名属性后者覆盖前者
```
const target = { a:1 };
Object.assign(target, { b:2 }, { c : 3});
console.log(target);// {a:1, b:2, c:3}

const stream = Object.assign({},target) // 浅拷贝 创建引用，内存地址不变
```
######Object.getOwnPropertyDescriptor()
对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为,该方法获取Descriptor对象
```
let obj = { foo: 123 };
const oDescriptor = Object.getOwnPropertyDescriptor(obj, 'foo') ;
console.log(oDescriptor);
//{value: 123, writable: true, enumerable: true, configurable: true}
```
######Object.keys()，Object.values()，Object.entries()
```
const obj = { foo:123 };
Object.keys(obj); //["foo"]
Object.values(obj); //[123]
Object.entries(obj); // [["foo", 123]]
```
######for...in 
```
let obj = { foo: 123 };
for (let key in obj){
    console.log(key)
} // 'foo'
```
