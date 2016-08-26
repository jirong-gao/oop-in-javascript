# 构造函数

## new关键字的作用

前面说过，在调用构造函数时需要加上new关键字。那么这个new究竟起来什么作用呢？

例如上面的构造函数Rectangle，我们稍微修改一下如下：

	var Rectangle = function(width, height) {
		this.width = width;
		this.height = height;
	};

	// Add a property "getArea" to Rectangle.prototype object
	Rectangle.prototype.getArea = function() {
		return this.width * this.height;
	};

当我们如下调用时：

	var rect = new Rectangle(314, 579);

当JavaScript引擎发现有new后，会隐含地将Rectangle()函数的行为修改为：

	var Rectangle = function(width, height) {
	  // Create a new object with hidden link to Rectangle.prototype
	  // var obj = Object.create(Rectangle.prototype);
	
	  // Set “this” variable to the newly created object
	  // this = obj;
	
	  this.width = width;
	  this.height = height;
	
	  // return this;
	};

加上new以后，就给函数加入了一些步骤变成：

1. 以Rectangle.prototype为原型创建一个新对象，作为要返回的对象
2. 将新创建的对象赋给this变量，也就是所说的把this绑定到该对象上
3. 执行函数的代码
4. 返回this所指向的对象


## 原型

