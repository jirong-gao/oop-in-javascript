# JavaScript中的原型实现

假设你对基于原型的面向对象编程方式有了一定的了解（如果没有，请参看[《JavaScript的面向对象编程实现方式》](class-prototype-oopp.md)）。OOP的一大特性“继承”就是通过原型来实现的。

那么具体在JavaScript语言中，这是如何实现的呢？

JavaScript内置了一个对象，就是我们在[《对象的创建》](how-to-create-objects.md)中看到的“Object.prototype对象”。需要注意的是，虽然我们将其称为Object.prototype对象，实际上prototype是Object的一个属性，JavaScript将这个内置对象赋值给Object.prototype。

这个对象具备了一些基本的属性，比如toString()和valueOf()，JavaScript希望新建的对象都直接或间接地以Object.prototype对象为原型对象，也就是直接或间接地继承Object.prototype对象。

我们再来看一下Object.prototype对象的属性：

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


其中，属性“__proto__”记录的就是当前对象的原型对象。不过，__proto__名字里的两个下划线表明这不是一个标准的属性，不能保证所有的JavaScript引擎都支持这个属性（有可能使用其它的名字）。由于在流行的浏览器JavaScript引擎中，都支持这个__proto__属性。所以，在最近的ES6版本的标准中，为了兼容才将这个__proto__属性写进了标准中。不过，在我们的代码中还是不建议直接使用这个__proto__属性，而使用Object.getPrototypeOf()这个方法来返回一个对象的原型对象。下面的例子中，我们有可能会直接访问__proto__属性，其目的是为了演示，而不是推荐这种用法。

## 使用源文本创建的对象

在[《对象的创建》](how-to-create-objects.md)中，我们提到的第一种创建对象的方法是使用源文本，这也是最直接简单的方法。比如：

	var emptyObj = {}; // An object with no properties

	// An object with properties
	var rectangle = {
		width: 3,
		height: 5,
		getArea: function() {
			return this.width * this.height;
		}
	};

	Object.getPrototypeOf(emptyObj) === Object.prototype; // true

	Object.getPrototypeOf(rectangle) === Object.prototype; // true

从上面的例子可以看到，两个以源文本方式创建的对象emptyObj和rectangle的原型对象都是Object.prototype。

实际上，在JavaScript中，以源文本创建的对象，其默认就是以Object.prototype对原型对象，也可以说是继承了Object.prototype对象。所以emptyObj和rectangle都可以使用Object.prototype对象的属性，比如：

	emptyObj.toString();  // [object Object]
	rectangle.toString();  // [object Object]

	// 也可以访问__proto__属性来查看其原型对象
	emptyObj.__proto__ === Object.prototype; // true

	rectanlge.__proto__ === Object.prototype;  // true

所以，我们说在JavaScript中很少有真正意义上的空对象，就是因为在一般情况下对象都直接或间接地继承了Object.prototype对象这个原因。

当然，JavaScript是一种非常灵活的语言，如果你希望有一个没有原型对象的“裸对象”，也可以。可以把原型对象去掉就可以了：

	var rawObj = {}: // 此时的rawObj还是继承Object.prototype对象

	rawObj.__proto__ = null; // 把rawObj的原型对象拿掉（即设为null），那么rawObj就没有原型对象了

	// 那么就不能再使用toString()方法了
	rawObj.toString(); // TypeError: rawObj.toString is not a function

说到这，JavaScript内置的这个Object.prototype对象也是没有原型对象的：

	Object.getPrototypeOf(Object.prototype);  // null

这样设计的目的是为了在原型链中能有一个终结点。

## 原型链



我们在[《对象与object》](object-and-object.md)一章中讲过，面向对象编程中的对象在JavaScript语言中是基于object类型（一种“键值对”的数据结构）的。

