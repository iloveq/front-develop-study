概念：对象是 JavaScript 的异步操作解决方案，为异步操作提供统一接口。它起到代理作用（proxy），充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口。Promise 可以让异步操作写起来，就像在写同步操作的流程，而不必一层层地嵌套回调函数。
设计思想：所有异步任务都返回一个 Promise 实例。Promise 实例有一个then方法，用来指定下一步的回调函数。
```
//传统的写法：
f1(f2)
//Promise链式写法
var p = new Promise(f1);
p.then(f2);
```
all操作符：Promise的all方法提供了并行执行异步操作的能力，并且在所有异步操作执行完后才执行回调（rxjava-zip）
```
Promise
.all([runAsync1(), runAsync2(), runAsync3()])
.then(function(results){
    console.log(results);
});
```
进阶：
```
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。
resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为rejected）， 在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
```
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```
then方法可以接受两个回调函数作为参数。
```
//第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```
catch方法用于指定发生错误时的回调函数
```
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```
Promise.resolve()的参数：
1）参数是一个 Promise 实例
```
const p1 = new Promise(function (resolve, reject) {
  // ...
});

const p2 = new Promise(function (resolve, reject) {
  // ...
  resolve(p1);
})
```
2）参数是一个thenable对象
```
//thenable对象指的是具有then方法的对象
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};
let p1 = Promise.resolve(thenable);
p1.then(function(value) {
  console.log(value);  // 42
});
```
3）参数不是具有then方法的对象，或根本就不是对象。
```
const p = Promise.resolve('Hello');

p.then(function (s){
  console.log(s)
});
// Hello
```
4）不带有任何参数
```
//如果希望得到一个 Promise 对象，比较方便的方法就是直接调用Promise.resolve方法。
const p = Promise.resolve();

p.then(function () {
  // ...
});
```
例子：
```
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
```
setTimeout(fn, 0)在下一轮“事件循环”开始时执行，Promise. resolve()在本轮“事件循环”结束时执行，console.log('one')则是立即执行，因此最先输出。
使用注意：
1）.then(f1).then(f2)f2不会等待f1都执行完，.then(f1(Promise.all)).then才可以
2）忘记使用catch，被吃掉的异常
3）忽略return
```
somePromise().then(function () {
  //忽略了then函数内部，应该：returen：someOtherPromise();
  someOtherPromise();
}).then(function () {
  // 我希望someOtherPromise() 状态变成resolved!
  // 但是并没有
});
```
