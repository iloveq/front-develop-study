# Javascript的this用法 {#page-title}

this是Javascript语言的一个关键字，经常被用到，下面将记录下this 的相关知识。

**this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁**，**实际上this的最终指向的是那个调用它的对象**

学好this的知识前提下是你必须掌握 **面对对象编程**的基础

### 1）全局调用方法 ：this 指代 window

```js
function a() {
    var name = 'gershon';
    console.log(this.name); //undefined
    console.log(this); // window
}
a();
```

### 2）作为对象方法调用 ：this 指代上级对象

```js
var o = {
    name: "gershon",
    fn: function () {
        console.log(this.name);  // gershon
    }
};
o.fn();
```

这里的this指向的是对象o，因为你调用这个fn是通过o.fn\(\)执行的，那自然指向就是对象o，这里再次强调一点，this的指向在函数创建的时候是决定不了的，在调用的时候才能决定，谁调用的就指向谁，一定要搞清楚这个。

为了阐述更清晰点，这里将继续讲这个知识点。

```js
var o = {
    name: "gershon",
    fn: function () {
        console.log(this.name); //gershon
    }
};
window.o.fn();
```

这段代码和上面的那段代码几乎是一样的，但是这里的this为什么不是指向window，如果按照上面的理论，最终this指向的是调用它的对象，这里先说个而外话，window是js中的全局对象，我们创建的变量实际上是给window添加属性，所以这里可以用window点o对象。

这里先不解释为什么上面的那段代码this为什么没有指向window，我们再来看一段代码。

#### 

```js
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //12
        }
    }
}
o.b.fn()
```

这里同样也是对象o点出来的，但是同样this并没有执行它。请看以下三种情况：

> 1. 情况1：如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window，这里需要说明的是在js的严格版中this指向的不是window，但是我们这里不探讨严格版的问题，你想了解可以自行上网查找。
> 2. 情况2：如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象。
> 3. 情况3：如果一个函数中有this，**这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象**

```js
var o = {
    a:10,
    b:{
        // a:12,
        fn:function(){
            console.log(this.a); //undefined
        }
    }
}
o.b.fn();
```

尽管对象b中没有属性a，这个this指向的也是对象b，因为this只会指向它的上一级对象，不管这个对象中有没有this要的东西。

### 3）作为构造函数调用：this 指代 new 出来的对象

```js
function Fn() {
    this.name = 'gershon';
}

var a  = new Fn();

console.log(a.name); // gershon
```

**更新一个小问题当this碰到return时**

```js
function Fn() {
    this.name = 'gershon';
    return {};
}

var a = new Fn();

console.log(a.name); // undefined
```

**如果返回值是一个对象，那么this指向的就是那个返回的对象，如果返回值不是一个对象那么this还是指向函数的实例。**

```js
function Fn() {
    this.name = 'gershon';
    return undefined;
}

var a = new Fn();

console.log(a.name); // gershon
```

### 4）call, apply, bind 改变 this 指向

call,apply,bind干什么的？为什么要学这个？

一般用来指定this的环境，在没有学之前，通常会有这些问题。

```js
var a = {
    name:"gershon",
    fn:function(){
        console.log(this.name);
    }
}
var b = a.fn;
b(); //undefined 因为这里是全局调用了fn
```

我们是想打印对象a里面的name却打印出来undefined是怎么回事呢？**如果我们直接执行a.fn\(\)是可以的**

* a.fn\(\) 对象a调用方法，所以this指向对象a

#### 4.1 call\(\)

```js
var a = {
    name:"gershon",
    fn:function(){
        console.log(this.name);
    }
}
var b = a.fn;
b.call(a); //gershon call 方法让 this 指向 a
```

通过在call方法，给第一个参数添加要把b添加到哪个环境中，简单来说，this就会指向那个对象。

call方法除了第一个参数以外还可以添加多个参数，如下：

```js
var a = {
    name: "gershon",
    fn: function (num1, num2) {
        console.log(this.name);   //gershon
        console.log(num1 + num2); //3
    }
}
var b = a.fn;
b.call(a, 1, 2)
```

#### 4.2 **apply\(\)**

apply方法和call方法有些相似，它也可以改变this的指向。

同样apply也可以有多个参数，但是不同的是，第二个参数必须是一个数组，如下

```js
var a = {
    name: "gershon",
    fn: function (num1, num2) {
        console.log(this.name); //gershon
        console.log(num1 + num2); //3
    }
}
var b = a.fn;
b.apply(a, [1, 2])
```

注意如果call和apply的第一个参数写的是null，那么this指向的是window对象

### 4.3 bind\(\)

bind方法和call、apply方法有些不同，但是不管怎么说它们都可以用来改变this的指向

```js
var a = {
    name: "gershon",
    fn: function () {
        console.log(this.name); 
    }
}
var b = a.fn;
b.bind(a);
```

我们发现代码没有被打印，对，这就是bind和call、apply方法的不同，实际上bind方法返回的是一个修改过后的函数

```js
var a = {
    name: "gershon",
    fn: function () {
        console.log(this.name);
    }
}
var b = a.fn;
var c = b.bind(a);
console.log(c); //ƒ () {console.log(this.name);}
```

那么我们现在执行一下函数c看看，能不能打印出对象a里面的 name

```js
var a = {
    name: "gershon",
    fn: function () {
        console.log(this.name);
    }
}
var b = a.fn;
var c = b.bind(a);
c(); // gershon
```

ok，同样bind也可以有多个参数，并且参数可以执行的时候再次添加，但是要注意的是，参数是按照形参的顺序进行的

```js
var a = {
    name: "gershon",
    fn: function (e, d, f) {
        console.log(this.name); // gershon
        console.log(e, d, f); //10 1 2
    }
}
var b = a.fn;
var c = b.bind(a, 10);
c(1, 2);
```

总结：call和apply都是改变上下文中的this并立即执行这个函数，bind方法可以让对应的函数想什么时候调就什么时候调用，并且可以将参数在执行的时候添加，这是它们的区别，根据自己的实际情况来选择使用。

### 5）箭头函数中的this

简要介绍：箭头函数中的this，指向与一般function定义的函数不同，比较容易绕晕，箭头函数this的定义：**箭头函数中的this是在定义函数的时候绑定，而不是在执行函数的时候绑定。**

1、何为定义时绑定

我们来看下面这个例子：

```js
var x=11;
var obj={
  x:22,
  say:function(){
    console.log(this.x)
  }
}
obj.say(); //console.log输出的是22
```

一般的定义函数跟我们的理解是一样的，运行的时候决定this的指向，我们可以知道当运行obj.say\(\)时候，this指向的是obj这个对象。

```js
var x=11;
var obj={
 x:22,
 say:()=>{
   console.log(this.x);
 }
}
obj.say(); //输出的值为11
```

这样就非常奇怪了如果按照之前function定义应该输出的是22,而此时输出了11，那么如何解释ES6中箭头函数中的this是定义时的绑定呢？

**所谓的定义时候绑定，就是this是继承自父执行上下文！！**中的this，比如这里的箭头函数中的this.x，箭头函数本身与say平级以key:value的形式，也就是箭头函数本身所在的对象为obj，而obj的父执行上下文就是window，因此这里的this.x实际上表示的是window.x，因此输出的是11。\(this只有在函数被调用，或者通过构造函数new Object\(\)的形式才会有this\)



