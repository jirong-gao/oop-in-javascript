# JavaScript简史

## JavaScript的诞生

JavaScript是在1995年5月由布兰登·艾奇（Brendan Eich）设计的。

布兰登·艾奇是1995年4月加入Netscape公司的，当时Netscape 1.1已经发布。Netscape的计划是提供一种“胶水式的语言”，可在浏览器上运行，目的是为了让网页设计者、程序员在页面构建时拥有更多的控制力。为了赶上Nescape 2.0 Beta版本的发布计划，布兰登·艾奇被告知他只有10天的时间。于是，他就用了10天的时间编写了第一个版本,当时这个语言的名称叫Mocha。同年9月，改名为LiveScript；然后在12月，在Netscape Navigator 2.0 beta 3的发布版本中又被改名为JavaScript。

由于时间紧迫，加上当时Netscape管理层的一些要求以及布兰登·艾奇自身的一些技术背景，JavaScript借鉴了当时的多个语言，自出身就像一个大杂烩，详细的情况可以参看 [《JavaScript: Designing a Language in 10 Days》](https://www.computer.org/csdl/mags/co/2012/02/mco2012020007.pdf)。

JavaScript在设计之初主要的一些参考来源：

- 基本语法：借鉴C语言
- 函数是第一等公民： 借鉴Scheme语言（函数是一个值，可以像int或string类型的值一样传来传去）
- 面向对象模式：借鉴Self语言（基于原型的面向对象编程模式）
- 正则表达式：借鉴Perl
- ... ...

谁曾想到，这个只花了10天时间设计出来的语言，现在已经演变为互联网的第一大语言。随着Node.js对JavaScript在服务器端的强力支持，JavaScript逐渐显露出前后通吃的迹象。


## 标准化过程

1996年11月，Netscape将JavaScript提交给Ecma国际（Ecma International）进行标准化。Ecma国际（Ecma International）是一家国际性会员制度的信息和电信标准组织。1994年之前，名为欧洲计算机制造商协会（European Computer Manufacturers Association）。因为计算机的国际化，组织的标准牵涉到很多其他国家，因此组织决定改名，在名称后加了一个“International”表明其国际性。

1997年6月，Ecma International正式发布了第一版的语言规范，称为ECMAScript 1（ES1），标准号为ECMA-262。

有了规范以后，其他的浏览器厂商就可以基于标准规范提供不同的实现。比如微软的JScript（注意JScript和JavaScript不是一回事），还有在Flash中使用的ActionScript。

1999年12月发布的ECMAScript 3.0版本是一个巨大的成功，在业界得到广泛支持，成为通行标准，奠定了JavaScript语言的基本语法。

由于争议，ECMAScript 4.0没有被正式发布。2009年12月，ECMAScript 5.0版正式发布，ES5来自于ECMAScript 3.1。

现在，标准的制定者们计划每年发布一次，并且使用年份作为版本号。2015年6月，ECMASCript 6.0正式发布，也被称为ECMASCript 2015，代号为harmony。所以，**ECMASCript 6.0 = ES6 = ECMASCript 2015 = ES2015 = ES6 Harmony**。

ES6的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。因此，在ES6中引入了新的语法，较大的变化就是支持class、module。这两个其实都是为了更好地实现重用，class是代码级的重用，而module更多API级的重用。

不过，需要注意的是，不是说JavaScript是从ES6才支持面向对象编程的。它自出生以来就支持面向对象编程，虽然说不支持class。

啊？！不支持类（class），也能叫面向对象编程。是的，请看[《对象与object》](object-and-object.md)。

## 参考资料

- [ECMAScript® 5.1中文版](http://lzw.me/pages/ecmascript/)
- [ECMAScript® 2015 Language Specification (pdf)](http://www.ecma-international.org/ecma-262/6.0/ECMA-262.pdf)
- [ECMAScript® 2016 Language Specification](http://www.ecma-international.org/ecma-262/7.0/index.html)

