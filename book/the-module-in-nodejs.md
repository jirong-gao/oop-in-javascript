# Node.JS中的模块（Module）

通过面向对象的编程模式，JavaScript实现了源代码级别的复用。但是，要实现类似其它语言的二进制级别的复用（通过静态库，或者动态库），还必须借助模块系统。

Node.JS中的模块系统实现，遵循了CommonJS规范，实现了[CommonJS规范定义的模块系统](http://wiki.commonjs.org/wiki/Modules)。

## 模块类型和加载过程

通过模块系统，Node.js很好地实现了复用。每个模块形成了一个闭合的作用域，模板内部的变量是本地私有变量，外部只能获取模块通过module.exports（或者exports）对象对外暴露的API。

Node.JS对模块系统这样设计，很好地减少了对全局作用域的污染，避免了命名冲突，也简化了代码的复用。

**Node.js中的模块有两种组成形式：**

1. 单一文件形式的模块，一个文件就是一个模块
2. 目录形式的模块（包括该目录下的子目录和文件）

如果模块是以目录形式存在的，模块的入口文件按下列顺序进行查找（参看 13.1）：

1. 目录下的package.json文件，查找”main”属性；
2. 如果package.json文件中没有”main”属性，Node.js会报错，说找不到该模块。
3. 如果没有找到package.json文件，则尝试目录下的index.js文件；
4. 如果没有找到，则尝试目录下的index.json文件；
5. 如果没有找到，则尝试目录下的index.node文件。

虽然在Node.js官方文档中说有两种形式的模块，但是从根本上来说只有一种类型的模块，就是单一文件形式的模块（一个文件对应一个模块）。

目录形式的模块，实际上只是为了从代码组织、作为单独的库（或者叫作包）进行发布而这样称呼的。在加载目录形式模块的过程中，Node.js会首先进行路径解析，找到该模块的入口文件（比如index.js），然后将这个入口文件作为一个模块加载进来。而不是将这个目录作为一个模块整体加载进来。这个入口文件再通过require函数加载其它的文件。

在Node.js源码中，定义了一个Module构造函数，源码可见[module.js](http://https://github.com/nodejs/node-v0.x-archive/blob/master/lib/module.js)。这是Node.js模块系统的核心源码。Node.js加载的每个模块都是这个Module构造函数所创建的一个实例，这个Module构造函数如下：

```javascript
function Module(id, parent) {
	this.id = id;
	this.exports = {};
	this.parent = parent;
	// ...

```

这里的exports属性就是我们在每个模块中用来对外暴露API的。

我们常用的require函数实际上是当前Module对象实例中的require方法，见下：

```javascript
// Loads a module at the given file path. Returns that module's
// `exports` property.
Module.prototype.require = function(path) {
	assert(path, 'missing path');
	assert(util.isString(path), 'path must be a string');
	return Module._load(path, this);
};

```

require函数中会调用Module._load函数，对模块进行加载。加载成功后，返回该模块对象的”exports”属性。当前Module对象实例就会成为被加载模块（会创建一个新的Module对象实例）的父模块。

而Module._load会做如下工作：
```javascript
Module._load = function(request, parent, isMain) {
	// 1. Check Module._cache for the cached module.
	// 2. Create a new Module instance if cache is empty.
	// 3. Save it to the cache.
	// 4. Call module.load() with your the given filename.
	//    This will call module.compile() after reading the file contents.
	// 5. If there was an error loading/parsing the file,
	//    delete the bad module from the cache
	// 6. return module.exports
};
```

Module._load负责加载新的模块和管理模块缓存(module cache)。

在Module._load会先找到需要加载的文件：

- 如果是单一文件形式的模块，就很简单，就是该文件
- 如果是目录形式的模块，就需要寻找到该模块的入口文件

然后检查在缓存中是否已经存在该模块

如果没有，就为文件创建一个Module对象实例，上面的step 2：
```javascript
var module = new Module(filename, parent);
```

将新创建的Module对象实例存入缓存中。

然后，使用这个新创建的Module对象实例加载该文件：
```javascript
module.load(filename);
```

这会首先读取要加载文件的内容，然后调用module._compile:
```javascript
Module.prototype._compile = function(content, filename) {
	// 1. Create the standalone require function that calls module.require.
	// 2. Attach other helper methods to require.
	// 3. Wraps the JS code in a function that provides our require,
	//    module, etc. variables locally to the module scope.
	// 4. Run that function
};
```

在这里，首先会定义一个特殊的require函数，而这个require函数就是我们在写代码时，每个源文件中用到那个。
```javascript
function require(path) {
    return self.require(path);
}
```

**当require完成后，整个被加载的源代码将会被包在一个新的函数中：**
```javascript
(function (exports, require, module, __filename, __dirname) {
  // YOUR CODE INJECTED HERE!
});
```

**这将会创建一个新的函数作用域，这个作用域仅对该新建的Module对象实例可见，从而就避免了污染全局空间。**

**实际上，这里传进来的require函数，就是我们在代码中使用的require函数。**
**在代码中使用的module.exports中的module对象，就是这里传进来的module对象。**
**在代码中使用的exports（不是上面module.exports中的exports）,就是这里传进来的exports。**

而这个exports是指向module.exports，引入这个exports估计是为了遵循CommonJS规范对Module的定义。因为在代码中，可以仅使用module.exports就可以了，更多细节见13.3。


`__`filename和`__`dirname是Node.js在查找该模块后找到的模块名称和模块绝对路径。

## Node.JS的Module实现总结

- 一个文件与一个模块对应

- Node.js为每一个被加载的文件创建一个模块对象，该对象是由Module构造函数创建的，该模块对象的原型对象是Module.prototype。该模块对象的一些属性：
	- 属性”id”，字符串，标识自己，通常是该文件被解析出来的全路径名
	- 属性”exports”，对象，本模块对外暴露的接口
	- 属性”parent”，指向一个模块对象，是第一个加载（通过require方法）本模块的模块


- 当加载完成后，整个源代码会被包在一个新的函数中（这就是源代码的运行时刻形态）：
```javascript
(function (exports, require, module, __filename, __dirname) {
	// YOUR CODE INJECTED HERE!
});
```
	- 可以想像成我们在编写源代码时，在当前文件的外部就裹了一层函数声明
	- 我们之所以可以在代码中使用require函数、exports对象和module对象，是因为这些变量作为包裹函数的参数被传进来了
	- exports对象指向module.exports对象
	- require函数返回的是module.exports对象

