# 对象与object

"对象与object"，这标题咋这么怪呢？难道这俩不是一回事？

一提起JavaScript的面向对象编程时，可能最常听到的就是“everything is object”。全部都是对象，那代码写起来肯定是相当地优雅。

当我们一谈到“对象”时，脑海里的第一反应肯定是面向对象的概念（仅限于程序猿人群，生活中的对象是相亲，别当着你的对象谈面向对象:)）。但在JavaScript中的object是一个数据类型，所以我特意没有将其翻译为中文。

## JavaScript中的类型

JavaScript定义了两种类型，一种是基本类型（primitive type），包括：

- 数值（number）
- 字符串（string）
- 布尔值（boolean）- true或者false
- null
- undefined

关于基本类型的详细讲解可以参见[阮一峰的教程](http://javascript.ruanyifeng.com/grammar/types.html)，写得非常详尽易懂。

这里面最容易引起误解的是null和undefined，同样在[阮一峰的教程中](http://javascript.ruanyifeng.com/grammar/types.html#toc2)可以找到具体的解释。

另一种类型就叫作object类型（object type），为了避免混淆，在涉及类型的讨论中，我将一直使用"object"这个英文单词。

## object类型

与基本类型不同的是，**object类型是一种复合类型，是一种无序的数据集合，由若干的“键值对”（key-value）构成**。


注意，这个概念非常重要，JavaScript的面向对象实现就是基于这个“键值对”的数据结构。理解、记住它，JavaScript中面向对象的很多疑点、难点就会迎刃而解。

    var book = {
    	title: 'JavaScript之面向对象编程',
    	language: '中文'
    };

	console.log(book.title); // 'JavaScript之面向对象编程'
	console.log(book.language); // '中文'
    
上面代码定义了一个包含两个键值对的object类型的值。在JavaScript中，将每个“键值对”称为一个“属性”（property），每个属性都具有属性名和属性值。

这里的book有两个属性，其中名为“title”的属性的值为“JavaScript之面向对象编程”。通常，我们称之为“属性title的值是xxxxx”。
我们可以通过点运算符的方式对属性进行访问。

但实际上，这种叫法已经在开始慢慢误导我们了。请牢记，从数据结构上看，object类型就只包含若干无序的属性（键值对），没有其它的了。

除了这个一般的object类型，JavaScript还定义了两个特殊的object类型：数组（array）和函数（function）。

属性的值可以是基本类型的值，也可以是复合类型的值（即另外一个object类型，类似于嵌套结构），当然也就可以是数组和函数。对了，**函数可以作为值，而且在JavaScript函数是一个真正意义上的值**。你可以把它想像成类似一个数字或者字符串，可以把一个函数赋值给一个变量，或者在函数的调用中返回一个函数。

    // 定义一个变量foo，并将一个函数赋值给该变量
	var foo = function() {
    	console.log("This function is being invoked!");
    };

	typeof foo;  // function
    
    foo(); // 通过在变量名foo后添加小括号运算符“（）”，来调用foo所指向的函数

让我们来看看面向对象的概念。

## 面向对象编程

面向对象编程（Object-oriented programming, OOP）与其说是一种编程的方式，倒不如说是一种思考问题、解决问题的思维方式。

在面向对象编程出现之前，主要是面向过程的编程方式。面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了。

面向对象则是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描述某个事物在整个解决问题的步骤中的行为。对象概念的提出是从真实世界中实实在在存在的物体而来的，比如我们身边的人、永不离手的手机、这个暴热夏天里必备的空调等等。

这些物体的共性是：都具有**属性**和**行为**。人有属性（名字、性别、肤色、国籍）和行为（跑步、高兴、吃饭），手机有属性（品牌、颜色、大小）和行为（响铃、拍照、播放音乐）等等。相对于面向过程的方式，面向对象的思考方式在问题越复杂的情况下，越显得自然、优势明显，这也是面向对象编程流行起来的原因。

OOP中的对象概念与真实世界中的对象类似，每个对象都包含有属性和行为。属性通常是存储在对象内部的变量上，而通过方法来展现其行为；在很多编程语言中，方法的具体表现形式是函数。

在JavaScript语言中，就是基于object类型来实现的OOP里面的对象。属性很好理解，比如下面这个例子：

	var rectangle = {
		width: 3,
		height: 5
	};

rectangle是一个具有两个属性的object，分别为width和height（英文中的矩形讲宽和高，中文中是长和宽），这还只是一个数据结构。如果我们以面向对象的角度来思考问题，将其看作一个矩形对象，具有width和height属性，而且在需要时能够返回面积。那么：

	var rectangle = {
		width: 3,
		height: 5,
		getArea: function() {
			return this.width * this.height;
		}
	};

	// 获得rect的面积值
	console.log(rectangle.getArea()); // 15

注意到getArea了嘛？这仍然是rectangle的一个属性，只不过这个属性的值是一个函数，可以被调用执行。在JavaScript的面向对象编程语境下，将这样的属性称为方法。这样object类型就非常好地实现了OOP中对象的核心概念。

此时，通常也就将rectangle称为rect对象，完成了从object类型到对象的演变。从代码上来看，并没有什么变化，变化的是你的思考问题的方式。比如说在面向过程的思考方式下，我们就会如下来使用rect：

	// 声明一个函数用来计算矩形的面积
	function getArea(rect) {
		return rect.width * rect.height;
	}

	var area = getArea(rectangle);

	console.log(area);	// 15

这体现的就是OOP的封装思想。

这里要注意的是，由于getArea是rectangle的一个属性，它的值是可以变化的，比如我们将其改为一个字符串：

	rectangle.getArea = 'It\'s a string now, not a function any more!';

	console.log(rectangle.getArea); // It's a string now, not a function any more!

	rectanlge.getArea(); // TypeError: rectangle.getArea is not a function

此时，getArea仍然是一个属性，不过其值已经变为一个字符串了，这时它也不能叫作方法了。这和C++、Java大不一样，这也体现了JavaScript作为动态语言的特点。

所以，对象的方法在JavaScript中只是为了沟通交流方便的一个叫法，从严格意义上说都是属性，没有方法。这也是为什么要花大量篇幅来说明object类型与对象区别的原因，在后面还会经常碰到这样的情况。

## 英文中的object

在[维基百科上](https://en.wikipedia.org/wiki/Object_(computer_science))，对object在计算机领域里的释义为：


*In computer science, an object can be a **variable**, a **data structure**, or a **function**, and as such, is a location in memory having a value and possibly referenced by an identifier.*

*In the class-based object-oriented programming paradigm, "object" refers to a particular **instance** of a class where the object can be a combination of variables, functions, and data structures.*

可以看到，object在英文中本来就是多种含义，而OOP里的对象概念只是其中的一种。母语为英文的人在阅读、沟通交流时，脑海中也就具有这样的背景知识，能够自动区分在不同语境下的区别。这就像我们在中文环境中，你的母亲大人叫你周末去面见对象，我想你的脑海中肯定不会呈现什么属性和方法的。

# 小结

罗里吧嗦说了这么多，主要是希望在JavaScript的面向对象编程中建立以下几个关键的概念：

1. object类型是一种复合类型，是一种无序的数据集合，由若干的“键值对”（key-value）构成；

2. 这个“键值对”又称为属性，包括属性名和属性值；属性值可以是基本类型，也可以是object类型，或者是函数；

3. 这个object类型不等同于面向对象中的对象概念，object类型可以很好地在面向过程的编程方式下使用；

4. 面向对象编程中的核心概念-“对象”，是借助于object类型实现的；属性值为函数的属性，可以称之为方法

等会儿，在上面维基百科对object的解释中，提到基于类的面向对象编程方式（class-based object-oriented programming paradigm）。难道面向对象的编程方式还有多种？不都是先设计类，再基于类的创建对象实例的嘛？

是的，除了基于类的面向对象编程模式，还有一种叫作基于原型的面向对象编程方式（prototype-based object-oriented programming paradigm）。而JavaScript就采用了这种模式。

我说嘛，JavaScript里面的OO理解起来总是有点磕磕绊绊的。原来除了上面的对象与object的微妙差别以外，还有[这么一出](class-prototype-oop.md)。





