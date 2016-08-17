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

与基本类型不同的是，**object类型是一种复合数据类型，是一种无序的数据集合，由若干的“键值对”（key-value）构成**。


注意，这个概念非常重要，JavaScript的面向对象实现就是基于这个“键值对”的数据结构。理解、记住它，JavaScript中面向对象的很多疑点、难点就会迎刃而解。

    var book = {
    	title: 'JavaScript之面向对象编程',
    	language: '中文'
    };
    
上面代码定义了一个包含两个键值对的object类型的值。在JavaScript中，将每个“键值对”称为一个“属性”（property），每个属性都具有属性名和属性值。

这里的book有两个属性，其中名为“title”的属性的值为“JavaScript之面向对象编程”。通常，我们称之为“属性title的值是xxxxx”。

但实际上，这种叫法已经在开始慢慢误导我们了。请牢记，从数据结构上看，object类型就只包含若干无序的属性（键值对），没有其它的了。

咦，属性，有点面向对象的意思了。是不是想到方法（method）了，别急，慢慢来。

除了这一般的object类型，还有两个特殊的object类型：数组（array）和函数（function）。

数组（array）是一组按顺序排列的数据集合，数组中每个值称为“元素”，每个元素都有相应的位置编号（数组下标，从0开始）。

函数（function）的特殊之处在于它关联了可执行的代码，函数可以被调用以执行这些代码并返回结果。在JavaScript中，函数本身是一个值，这是非常重要的一点。

    var arr = [1, 2, 3];
    
    typeof arr;  // object
    
    var foo = function() {
    	console.log('Hello World!');
    };
    
    typeof foo; // function





