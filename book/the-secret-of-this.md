# this 解密

从C++、Java等基于类的面向对象编程语言中，我们都接触过“this”关键字。一看到“this”，我们就自然而然地认为这是一个对对象自身引用的指针，不会有任何歧义。

但是在JavaScript中，这个“this”由于JavaScript语言本身设计的原因，在不同的情况下有不同的表现。

首先，我们需要知道的是，this实际上是函数在被调用时的一个隐含的参数，另外一个隐含参数是arguments。所以，this的值是和函数被调用的方式紧密相关的，不同的调用方式导致this的不同取值。

在JavaScript中有四种不同的函数调用模式：

- 方法调用模式 (Method Invocation Pattern)
- 函数调用模式 (Function Invocation Pattern)
- 构造函数调用模式(Constructor Invocation Pattern)
- apply调用模式

## 方法调用模式 (Method Invocation Pattern)

如果一个对象的某个属性的值为函数时，我们将这个函数称之为方法（method）。当函数作为对象的方法被调用时，this就指向对象本身。

```javascript
var rect = {
	height: 3,
	width: 5,

	getArea: function() {
		return this.height * this.width;
  	}
};

console.log(rect.getArea());  // 15
```

对象rect的属性getArea的值为一个函数，rect.getArea()就是将该函数作为对象的方法进行调用。

**this的值是在函数被调用的时候才决定的**，比如在上面的例子中，getArea方法被调用时才将对象rect绑定到this变量上。这样就可以在对象的方法中通过this来访问本对象的属性。

## 函数调用模式 (Function Invocation Pattern)

函数调用模式也就是一般情况下函数被调用的情况。

在这种调用模式下，this被绑定到全局对象，而具体是什么全局对象要根据运行环境而定：

- 如果是在浏览器中运行，全局对象是window对象，那么this将会指向window对象。
- 如果是在Node.js中运行，全局对象就是一个名为global的对象，那么this将会指向这个global对象。

这样的设计在平常使用函数时，不会有什么问题。但是，在面向对象的编程中，可能会带来问题。这个问题就是在对象的方法中，如果有内部函数，对this的使用极有可能发生意想不到的错误。

比如，我们给前面的rect对象增加一个方法incr，用以将长和宽加1。为了解释这个情况，我们故意地使用了内部函数，如下：

```javascript
// 给上面的rect对象新增一个方法incr()
rect.incr = function() {
	// Use inner function  purposely 
	var foo = function() {
		this.height++;  // 当该函数作为普通函数被调用时，this参数会被绑定到全局对象上

		this.width++;
	};

	foo();  // 以普通函数调用的方式来调用函数foo()
};

rect.incr(); // Invoke incr as method

console.log(rect.height);  // Still 3
console.log(rect.width);   // Still 5
```

我们的原意是希望通过incr()方法来增加这个矩形的长和宽，但是我们可以看到上面的代码并没有达到预期目的。问题就出在对“this”的使用上。

内部函数foo在作为函数被调用时，foo函数里面的this被绑定到全局对象，而不是我们期望的rect对象本身。Douglas Crockford认为这是JavaScript设计上的一个错误。

为了避免这个问题，我们可以避免使用this变量，而另外定义一个变量，将这个变量命名为self：

```javascript
rect.incr = function() {
	var self = this;  // Assign the value of this to variable self
 
	// Use inner function  purposely 
	var foo = function() {
		self.height++;  
		self.width++;				
	};

	foo();  // Invoke foo as a normal function
};

rect.incr(); // Invoke incr as method

console.log(rect.height);  // 4
console.log(rect.width);   // 6
```

在incr()方法的开头，新定义一个变量self，然后将this的值赋给self。在上面讲过，当函数作为对象的方法被调用时，this就会被绑定到对象本身，也就是对rect对象本身的引用。那么变量self也就是对rect对象本身的一个引用，而且该变量不会被在内部函数中有歧义（只要不是故意地在内部函数中定义新的同名变量）。

这样，当内部函数foo被调用时，就通过变量self来访问对象rect的属性。

可以看到，这种内部函数实际上就是闭包。所以，当你在对象中使用闭包时，如果要访问对象的属性时，最好不要直接使用this关键字，而使用上面的self这种代理方式。

特别是在Node.js中，回调函数被普遍使用，所以最好不要直接使用this。

## 构造函数调用模式(Constructor Invocation Pattern)

当一个函数在调用时，如果前面加上了new关键字，就是将被调用的函数作为构造函数进行调用。

我们在[《构造函数》](constructor.md)一章中，已经详细地解释了构造函数在被调用时发生的隐含动作。构造函数调用的结果将会创建一个新对象，而构造函数中的this将会被绑定到该新创建的对象上。

## apply调用模式

函数本身也是对象，所以函数本身也可以拥有方法。

函数可以通过apply方法来进行调用，调用的方式如下：

	fun.apply(thisArg, [argsArray])

通过这种方式，我们可以明确地指定this参数的值，同时也可以通过一个数组来构造其它的函数参数。

实际上，这个apply方法来自Function.prototype。而Function.prototype是所有函数对象的原型对象，所以这个apply方法的调用是通过原型链从Function.prototype继承而来的。
