# 构造函数

根据前面所讲的，我们知道构造函数是一个希望在调用时加上new关键字的函数，它的作用主要有两个：

- 创建一个新的对象
- 为要创建的对象构造原型对象，并将新创建的对象与其原型对象关联起来

## new关键字的作用

如此可见，这个new关键字对于构造函数而言非常关键。那么这个new究竟起了什么作用呢？

比如下面的Rectangle这个构造函数：

	var Rectangle = function(width, height) {
		this.width = width;
		this.height = height;
	};

	// 在 Rectangle.prototype 对象中添加一个属性“getArea”，该属性的值为一个函数
	Rectangle.prototype.getArea = function() {
		return this.width * this.height;
	};
	
	// 生成一个rect对象
	// 该rect对象拥有两个属性（width和height）和一个方法（getArea）
	var rect = new Rectangle(5, 7);

	console.log(rect.getArea());  // 35

从以上代码上看，这个Rectangle非常像一个类，而rect对象是Rectangle类的一个实例。这也是为什么使用构造函数的这种方式被Douglas Crockford称为“伪类模式”（pseudoclassical pattern）的原因。

估计这主要是由于基于类的编程方式太流行了，JavaScript的开发者们为了争取更多的使用者而这样设计的。在很多文章中，也如此解释这个构造函数。当实际上这是不正确的，我们必须要从基于原型的思想来理解这个构造函数，这样才不会用着用着就出现哪不对的情况。

当new这个关键字出现在函数调用的前面时，实际上是在告诉JavaScript引擎这是一个构造函数的调用。而JavaScript引擎就会隐含地对被调用函数进行一些修改，比如Rectangle()函数在运行时将会被修改为：

	var Rectangle = function(width, height) {
	  // Create a new object with hidden link to Rectangle.prototype
	  // var obj = Object.create(Rectangle.prototype);
	
	  // Set “this” variable to the newly created object
	  // this = obj;
	
	  // Execute original code
	  this.width = width;
	  this.height = height;
	
	  // return this;
	};

正如上面的注释，当加上new以后，就如同给函数加入了一些步骤，从而变成：

1. 以Rectangle.prototype为原型创建一个新对象，作为要返回的对象
2. 将新创建的对象赋给this变量，也就是所说的把this绑定到该对象上
3. 执行原有的函数代码
4. 返回this所指向的对象



