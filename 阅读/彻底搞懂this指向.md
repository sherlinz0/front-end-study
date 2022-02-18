# 彻底搞懂 this 指向

**想要理解 this, 先记住一下两点:**

- this 永远指向一个对象
- this 的指向完全取决于函数调用的位置

看下面这个例子:

```javascript
function fun(){
    console.log(this.s);
}
var obj = {
    s:'1',
    f:fun
}
var s = '2';
obj.f(); //1
fun(); //2
```

this 的指向为什么会发生改变, this 指向的改变到底是什么时候发生的.
在 JavaScript 中, `数组`, `函数`, `对象` 都是引用类型, 在参数传递时也就是引用传递.

上面的例子中, `obj` 对象有两个属性, 但是属性的值类型是不同的, 在内存中的表现形式也是不同的.

![](https://pic1.zhimg.com/80/v2-d8344e9d298a58727e08d7a36ea4c8d0_720w.jpg)

调用时就成了这个样子:

![](https://pic2.zhimg.com/80/v2-ddd9067ff2cbccbd380604d656775a4d_720w.jpg)

再来看下面的例子:

```JavaScript
var A = {
    name: '张三',
    f: function () {
        console.log('姓名：' + this.name);
    }
};
var B = {
    name: '李四'
};
B.f = A.f;
B.f()   // 姓名：李四
A.f()   // 姓名：张三
```

上面例子中, `A.f` 属性被赋给 `B.f`, 也就是 `A` 对象将匿名函数的地址赋给 `B` 对象.\
那么在调用时, 函数分别根据运行环境的不同, 指向对象 `A` 和 `B`.

![](https://pic2.zhimg.com/80/v2-cbc152bda151b900e508a217cd4d2bb9_720w.jpg)

再来看一个例子, 加深理解:

```JavaScript
function foo() {
    console.log(this.a);
}
var obj2 = {
    a: 2,
    fn: foo
};
var obj1 = {
    a: 1,
    o1: obj2
};
obj1.o1.fn(); // 2
```

`obj1` 对象的 `o1` 属性值是 `obj2` 对象的地址, 而 `obj2` 对象的 `fn` 属性的值是函数 `foo` 的地址.\
函数 `foo` 的调用环境是在 `obj2` 中的, 因此 this 指向对象 `obj2`.

**this 使用最频繁的有一下 5 中情况:**

## 对象中的方法

对象中的方法已经在上文说明.

## 事件绑定中的 this

事件绑定共有 3 中方式: 行内绑定, 动态绑定, 事件监听.

行内绑定的两种情况:

```JavaScript
<input type="button" value="按钮" onclick="clickFun()">
<script>
    function clickFun(){
        this // 此函数的运行环境在全局window对象下，因此this指向window;
    }
</script>

<input type="button" value="按钮" onclick="this">
<!-- 运行环境在节点对象中，因此this指向本节点对象 -->
```

动态绑定与事件监听:

```JavaScript
<input type="button" value="按钮" id="btn">
<script>
    var btn = document.getElementById('btn');
    btn.onclick = function(){
        console.log(this) // this指向本节点对象
    };
</script>
```

## 构造函数中的 this

```JavaScript
function Pro(){
    this.x = '1';
    this.y = function(){};
}
var p = new Pro();
```

![](https://pic2.zhimg.com/80/v2-7ce5f71bd0865872b513a88fabb597fd_720w.jpg)

## window 定时器中的 this

```javascript
var obj = {
	fun: function () {
		this;
	}
}
setInterval(obj.fun,1000);      // this指向window对象
setInterval('obj.fun()',1000);  // this指向obj对象
```

`setInterval(obj.fun, 1000)` 的第一个参数是 `obj` 对象的 `fun`, 因为 JS 中函数可以被当作值来做引用传递, 实际就是将这个函数的地址当作参数传递给了 `setInterval` 方法, 换句话说就是 `setInterval` 的第一参数接受了一个函数, 那么此时 1000 毫秒后, 函数的运行就已经实在 `window` 对象下了.\
`setInterval('obj.fun()', 1000)` 中的第一个参数， 实际则是传入一段可执行的 JS 代码. 1000 毫秒后当 JS 引擎来执行这段代码时, 则是通过 `obj` 对象来找到 `fun` 函数并调用执行, 那么函数的运行环境依然在对象 `obj` 内.

## 函数对象的 call(), apply() 方法

函数作为对象提供了 `call()`, `apply()` 方法, 他们也可以用来调用函数, 这两个方法都接受一个对象作为参数, 用来指定本次调用时函数中 this 的指向.

### call() 方法

```javascript
var lisi = {names:'lisi'};
var zs = {names:'zhangsan'};
function f(age){
    console.log(this.names);
    console.log(age);
    
}
f(23);//undefined
//将f函数中的this指向固定到对象zs上；
f.call(zs,32);//zhangsan
```

### apply() 方法

```javascript
var lisi = {name:'lisi'};
var zs = {name:'zhangsan'};
function f(age,sex){
	console.log(this.name+age+sex);
}
//将f函数中的this指向固定到对象zs上；
f.apply(zs,[23,'nan']);
```

**参考**:\
[1] 彻底搞懂JavaScript中的this指向问题 https://zhuanlan.zhihu.com/p/42145138
