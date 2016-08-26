# 对象的创建

创建一个新的对象，有三种方法：

1. 在代码中直接写，我们给这种方法一个术语，称作**源文本（literal）**方法；
2. 使用new关键字，调用构造函数；
3. 调用Object.create()方法

## 源文本方法

源文本方法就是在JavaScript源代码中使用大括号“｛｝”的方式直接写，这是最简单、直接的方法。比如下面的例子：

	var emptyObj = {}; // An object with no properties

	var rectangle = {
		width: 3,
		height: 5,
		getArea: function() {
			return this.width * this.height;
		}
	};

上面的两个表达式分别描述了两个不同的对象，当代码被JavaScript引擎加载解析时，这些对象源文本表达式就是告诉引擎去创建一个新的对象，并赋值给不同的对象。比如第一个表达式就会创建一个空对象，然后将该空对象赋给变量emptyObj；第二个表达式则会创建一个拥有3个属性的对象，其中一个属性的值为函数，我们可称其为方法。

需要注意的是这个空对象，如果我们定义了多个空对象，它们并不是一样的。

	var obj1 = {};  // The 1st object with no properties
	var obj2 = {};  // The 2nd object with no properties
	var obj3 = {};  // The 3rd object with no properties

	obj1 === obj2;  // false
	obj1 === obj3;  // false
	obj2 === obj3;  // false


通过上面的代码，说明了JavaScript引擎会在解析每一个对象源文本表达式时都会创建一个新的对象。也就是说，这里的obj1、obj2和obj3在内存中存储于不同的内存地址。


## 使用new关键字

先看下面的代码：

	var getArea = function(width, height) {
		return width * height;
	};

	var Rectangle = function(width, height) {
		this.width = width;
		this.height = height;
	};

仔细看上面的两个语句有什么不一样嘛？相同的是，两个都是一个赋值语句，两个变量（getArea和Rectangle）的值都是一个函数。
从语法的角度看，不同的主要有三点：

1. 第二个函数体里面出现了this关键字；有C++/Java经验的同学都知道，this在C++/Java中是指向当前的实例对象，是OOP的标志性语法之一；
2. Rectangle不同于前面例子中的rectanlge对象，首字母大写了，这在大小写敏感的语言中通常意味着什么；
3. 第一个函数有明显的返回语句，而第二个没有。

对于第一个函数，我想大家一眼就看出来其目的，就是根据矩形的宽和高计算其面积，并返回。

那第二个函数在干什么呢？

首先，需要声明的是第二个也是一个函数，和第一个函数没有本质上的区别，都可以被调用：

	var area = getArea(3, 5);	// 15

	var rect = Rectangle(314, 579);

	typeof rect;  // undefined

	console.log(width);	// 314
	console.log(height);  // 579;

上面代码中，我们调用getArea()，获得了预期的结果，得到了面积值。

但令人惊讶的是，第二个调用却什么也没有返回（对啊，Rectanlge()函数没有返回值）；而且，更怪异的是，我们传给Rectangle()函数的参数怎么变成全局变量上去了，这个秘密将在[《this解密》](the-secret-of-this.md)一章中揭晓。

第二个函数实际上是希望在被调用时，前面要加上new关键字，如下：

	var rect = new Rectangle(314, 579);

	typeof rect;  // object

	console.log(rect); // {width: 314, height: 579}

采用这种方式调用的结果就是创建了一个对象，该对象的宽为314，高为579，这正是我们所预期的结果。

在JavaScript中，我们将这种形式的函数叫作构造函数（constructor）。正如我们在这里所看到的，虽然构造函数可以像普通函数一样被调用，但是只有在调用时加上new关键字的情况下，才会如我们所预期的一样正确地创建对象。否则，将会出现意向不到的情况。

为了避免构造函数在被调用时忘记加new关键字的情况，所以在JavaScript中约定将构造函数的首字母大写。但是这只是一个命名约定，并非错误，JavaScript引擎也不会对直接调用构造函数产生任何警告或者错误。[ESLint中的new-cap规则](http://eslint.org/docs/rules/new-cap)，提供了多个选项来检测代码中构造函数编写规范。

构造函数首字母大写、使用new、使用this，这些特点和基于类的面向对象编程方式看起来非常类似。这主要是由于基于类的方式太流行了，JavaScript的设计者才设计出来这么一套用法，这种方式被Douglas Crockford称为**“伪类模式”（pseudoclassical pattern）**。而正是由于这些相似之处，造成了误解，很多时候把这里的构造函数看作是类了。

对于[构造函数的深入理解](constructor.md)（建议看完本章和[《JavaScript中的原型实现》](prototype-in-javascript.md)一章后再阅读），将会帮助你消除在JavaScript中使用OO的许多疑惑，所以单列一章讲解构造函数。

JavaScript内置了一个构造函数Object，通过它也能创建一个空对象：

	var emptyObj1 = new Object(); // An object with no properties

在ES5中，在Object中引入了create()方法，这就有了第三种创建对象的方法。

## 调用Object.create()方法

使用Object.create()方法可以在创建新的对象时，为其指定原型对象。那么新创建的对象就会继承所指定的原型对象。

Object.create()的语法如下：

	Object.create(proto[, propertiesObject])

第一个参数proto是为将要创建的对象提供一个原型对象；后面的参数是可选的，可以为将要创建的对象定义一系列的属性。

	var o1 = Object.create(null);  // Create an object without prototype object
	
	Object.getPrototypeOf(o1);  // null

	var o2 = Object.create(Object.prototype); // o2 is like {} or new Object()

上面创建的对象o1没有原型对象，也就没有继承任何属性，就连基本的toString()都不能用。

可以认为Object.create()方法一般用在需要对象继承的场合。为了兼容ES5之前的版本，我们可以这样定义一个名为inherit的函数：

	// inherit() returns a newly created object that inherits properties from the
	// prototype object p. It uses the ECMAScript 5 function Object.create() if
	// it is defined, and otherwise falls back to an older technique.
	function inherit(p) {
		if (p == null) throw TypeError(); // p must be a non-null object

		if (Object.create) {// If Object.create() is defined...
			return Object.create(p); // then just use it.
		}
	
		var t = typeof p; // Otherwise do some more type checking
		
		if (t !== "object" && t !== "function") throw TypeError();
		
		function f() {}; // Define a dummy constructor function.
		f.prototype = p; // Set its prototype property to p.
		
		return new f(); // Use f() to create an "heir" of p.
	}

从上面这个inherit函数，可以看到，Object.create()方法实际上也是用到了构造函数的方法来创建对象。

在前面例子中的对象o2，出现了“Object.prototype”，这是什么？让我们来看看：

	typeof Object.prototype; // object

	Object.getOwnPropertyNames(Object.prototype);
	[ 'constructor',
  	  'toString',
      'toLocaleString',
      'valueOf',
      'hasOwnProperty',
      'isPrototypeOf',
      'propertyIsEnumerable',
      '__defineGetter__',
      '__lookupGetter__',
      '__defineSetter__',
      '__lookupSetter__',
      '__proto__' ]

	typeof Object.prototype.toString;  // function

	typeof Object.prototype.valueOf;  // function

可以看出，Object.prototype这个属性实际上是指向一个对象，这个对象拥有一些我们非常熟悉的属性，比如toString和valueOf。而且可以看到属性toString和valueOf的值都是函数。

对了，这就是我们经常会用到的toString()和valueOf()方法。

而Object.prototype所指向的对象是由JavaScript默认提供的，其本意是希望提供一个普遍适用的原型，意思就是说：“我这有一个原型对象，已经为你们考虑好了一些基本的属性。你们要创建新的对象，最好以此为原型”。为了方便，我们就直接将其称为Object.prototype对象。

这个Object.prototype对象有一点比较特殊，就是它自身并没有原型对象：

	Object.getPrototypeOf(Object.prototype);  // null

为什么这样设计呢？让我们看看[《JavaScript中的原型实现》](prototype-in-javascript.md)。
