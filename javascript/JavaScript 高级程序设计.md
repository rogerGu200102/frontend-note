# JavaScript 高级程序设计

## 第三章 语言基础

### 3.3 变量

#### 3.3.1 var 关键词

使用var定义变量，且没有初始化时，变量保存一个特殊值undefined

1.声明作用域

去掉操作符定义后，为全局变量

在函数中定义时，将成为该函数的局部变量，在函数退出时销毁。即作用域为函数作用域

2.声明提升

会将所有的var声明拉到函数作用域的顶部，所以因此，即使多次使用var声明同一个变量也不会报错。

```javascript
//接下来的两端代码是等效的
function test(){
  var age = 10;
  var age = 11;
  var age = 22;
}
//实际将age的定义提升到了最顶部
function test1(){
  var age;
  age = 10;
  age = 11;
  age = 22;
}
```

#### 3.3.2 let 声明

let 声明和var声明非常相似，区别有以下两点：

1.let声明的作用域是块作用域。

2.let 声明不允许重复，否则会报错。

```javascript
let age;
let age; // SyntaxError； 标识符age已经声明过了
```

3.暂时性死区

let声明的变量不会在作用域提升。

在解析代码时，JavaScript引擎会注意后面的let声明，但是在此之前不能以任何方式引用并未声明的变量。在let声明之前的时间段称之为**“暂时性死区”**，在此阶段引用任何后面才声明的变量都会抛出**ReferenceError**。

#### 3.3.3 const声明

const和let基本一致，除了声明变量时必须同时初始化变量，并且不允许修改

```javascript
const age = 26;
age = 36; // TypeError: 给常量赋值
```

const 声明的限制只适用于它指向的变量的引用。也就是说，如果const变量引用的是一个对象，修改该对象内部的属性不违反const的限制。

注：如果想让const声明的引用值变量不被修改，可以采用Object.freeze()，这样赋值不会报错，但是会静默失败。

```javascript
const object= Object.freeze({});
o3.name= 'Jack';
console.log(o3.name);//undefined
```







### 3.4 数据类型

ECMAScript有6种简单数据类型，分别为：Undefined, Null, Boolean, Number, String和Symbol。

还有一种复杂数据类型Object(对象)。

#### 3.4.1 typeof 操作符

对一个值使用typeof操作符会返回下列字符串之一：

- "undefined"表示值未定义；
- "boolean"表示值为布尔值；
- "string"表示值为字符串；
- "number"表示值为数值；
- "object"表示值为对象（而不是函数） 或null；
- **"function"表示值为函数；**
- "symbol"表示值为符号。 

注：虽然Function也是时对象，但是函数有自己的特殊属性，有必要用typeof 操作符来区分函数和其他对象

#### 3.4.2  Symbol 类型



## 第四章 变量、作用域与内存

#### 4.1 原始值与引用值

原始值时最简单的数据，引用值时由多个值构成的对象。

保存原始值的变量是按值访问的。

保存引用值的变量是按引用访问的。

##### 4.1.1 动态属性

引用值可以动态增加属性

##### 4.1.2 复制值

原始值是直接赋值 赋值流程如下：

![1](D:\顾哲瑞\前端笔记\1.PNG)

引用值是赋值指针引用，指针指向堆内存中的对象 赋值流程如下：

![2](D:\顾哲瑞\前端笔记\2.PNG)

##### 4.1.3 传递参数

JS中的参数传递均为值传递，对于引用值的传递为传递指针

### 4.3 垃圾回收

浏览器的发展史上，主要使用过两种标记策略，标记清理和引用计数。

#### 4.3.1 标记清理

原理：

1、当变量进入上下文，比如函数内部声明一个变量时，这个变量会被加上存在于上下文中的标记。当变量离开上下文时，也会被加上离开上下文的标记。

2、垃圾回收程序运行时，会标记内存中的所有变量。之后，它会将所有在上下文中的变量，及被在上下文中变量引用的变量的标记去掉。在此之后再被加上标记的变量就是待删除的了。随后垃圾回收程序销毁带标记的所有值并收回内存

#### 4.3.2 引用计数

原理：维护一张变量-引用数的表，当有人使用变量赋值，计数+1，当该赋值被覆盖，引用数-1；

#### 4.3.4 内存管理

为了减少浏览器的内存负担，有以下几种方法可以提升浏览器性能：

1、通过let和const声明提升

2、隐藏类和删除操作（对象构造函数声明完全，尽量避免动态属性赋值，补充对象属性；尽量避免使用delete方法，采用赋值null的方式）

3.内存泄露（闭包）

4.使用静态分配和对象池

#### 



## 第五章 基本引用类型

### Date

#### 1、构造函数

##### 基础构造函数

```javascript
let now = new Date();
```

在不传参数的情况下，创建的对象将保存当前日期和时间

要基于其他日期和时间创造日期对象，需要传入毫秒表示；

```javascript
let date = new Date(miliseconds)//参数为毫秒
```

miliseconds 参数是一个Unix时间戳，它是一个整数值，表示1970年以来的毫秒数。

##### 辅助函数

ECMAScript 提供了Date.parse()和Date.UTC()方法进行转化。

###### Date.parse()

Date.parse()接受的转化格式如下：

```javascript
//"月/日/年"
let dateString1 = "5/23/2019"
//"月名 日名，年"
let dateString2 = "May 23, 2019"
//"周几 月名 日名 年 时:分:秒 时区"
let dateString3 ="Tue May 23 2019 00:00:00
GMT-0700"
//ISO 8601扩展格式“YYYY-MM-DDTHH:mm:ss.sssZ”
let dateString4 = "2019-05-
23T00:00:00"
```

如果构造函数直接传入字符串，构造函数将自动调用Date.parse()进行转化

```javascript
//可以使用如下方式进行转化
let date = new Date(dateString);
```

###### Date.UTC()

Date.UTC()的传入参数为，年，月数-1，日，时，分，秒，毫秒。只有年，月是必须参数，其余参数若无输入将默认为0.

和Date.parse()一致，若输入参数满足上述特征，将会隐式调用Date.UTC()进行数据转化。

Date.UTC()的转化不是GMT日期，而是本地时间

```javascript
//本地时间2000年6月6日17点55分55秒
let milisecond=Date.UTC(2000,5,6,17,55,55);
```

###### Date.now()

返回方法执行时日期和时间的毫秒数

#### 2、继承的方法

Date类型重写了toLocaleString(), toString() 和 valueOf()方法

toLocaleString()返回本地环境的时间，通常意味着格式含有AM或者PM但是不包含时区信息。

toString()返回的是包含时区信息的时间。

示例如下：

```javascript
toLocaleString() - 2/1/2019 12:00:00 AM
toString() - Thu Feb 1 2019 00:00:00 GMT-0800 (太平洋时间)
```

valueof 返回的是毫秒表示，即初始化的miliseconds

#### 3、日期格式化方法

toDateString()显示日期中的周几、月、日、年

```javascript
let date = new Date();
console.log(date.toDateString());
//Tue Nov 29 2022
```

toTimeString()显示日期中的时、 分、 秒和时区（格式特定于实
现） ； 

```javascript
let date = new Date();
console.log(date.toTimeString());
//18:41:27 GMT+0800 (中国标准时间)
```

toLocaleDateString()显示日期中的周几、 月、 日、 年（格式特定
于实现和地区） ；
toLocaleTimeString()显示日期中的时、 分、 秒（格式特定于实现
和地区） ；
toUTCString()显示完整的UTC日期（格式特定于实现）  

## 第6章 集合引用类型

### 6.1 Object

## 第8章 对象、类与面向对象编程

### 8.1 理解对象

#### 8.1.1 属性的类型

属性共分两种，数据属性和访问器属性

##### 01.数据属性


数据属性包含一个保存数据值的位置。 值会从这个位置读取， 也会写入到这个位置。 数据属性有4个特性描述它们的行为。

- [[Configurable]]： 表示属性是否可以通过delete删除并重新定义， 是否可以修改它的特性， 以及是否可以把它改为访问器属性。 默认情况下， 所有直接定义在对象上的属性的这个特性都是true， 如前面的例子所示。
- [[Enumerable]]： 表示属性是否可以通过for-in循环返回。 默认情况下， 所有直接定义在对象上的属性的这个特性都是true， 如前面的例子所示。
- [[Writable]]： 表示属性的值是否可以被修改。 默认情况下，所有直接定义在对象上的属性的这个特性都是true， 如前面的例子所示。
- [[Value]]： 包含属性实际的值。 这就是前面提到的那个读取和写入属性值的位置。 这个特性的默认值为undefined。 

要修改属性的默认特性，必须使用Object.defineProperty（）的方法。这个方法接收三个参数：对象，属性名，描述符对象。描述符对象上的属性可以包含：configurable，enumerable，writable和value。

##### 02.访问器属性

访问器属性不包含数据值。 相反， 它们包含一个获取（getter） 函数和一个设置（setter） 函数， 不过这两个函数不是必需的。 访问器属性有4个特性描述它们的行为。

- 
  [[Configurable]]： 表示属性是否可以通过delete删除并重新定义， 是否可以修改它的特性， 以及是否可以把它改为数据属性。 默认情况下， 所有直接定义在对象上的属性的这个特性都是true。

- [[Enumerable]]： 表示属性是否可以通过for-in循环返回。 默认情况下， 所有直接定义在对象上的属性的这个特性都

  是true。

- [[Get]]： 获取函数， 在读取属性时调用。 默认值为undefined。

- [[Set]]： 设置函数， 在写入属性时调用。 默认值为undefined。 

必须使用Object.defineProperty()进行定义。

多个属性使用Object.defineProperties()进行定义，定义时采用字面量的方式进行设置。

#### 8.1.3 读取属性的特性

使用Object.getOwnPropertyDescriptor()方法可以获取指定属性的属性描述符。参数为target（指定对象），和propertyName（属性名）。

### 8.2 创建对象

#### 8.2.2 工厂模式

示例：

```javascript
function createPerson(name, age, job) {
let o = new Object();
o.name = name;
o.age = age;
o.job = job;
o.sayName = function() {
console.log(this.name);
};
return o;
}let person1 = createPerson("Nicholas", 29, "Software Engineer");
let person2 = createPerson("Greg", 27, "Doctor");
```

缺点：并未解决对象标识的问题

#### 8.2.3 构造函数

通过new 修饰构造函数可以创建对象实例，具体操作如下：

(1) 在内存中创建一个新对象。
(2) 这个新对象内部的[[Prototype]]特性被赋值为构造函数的prototype
属性。
(3) 构造函数内部的this被赋值为这个新对象（即this指向新对象） 。
(4) 执行构造函数内部的代码（给新对象添加属性） 。
(5) 如果构造函数返回非空对象， 则返回该对象； 否则， 返回刚创建的新对象。

##### 注意

01.构造函数本质也是函数，因此若不通过new操作符，构造函数的this同样应当指向Global对象。

02.构造函数同样有缺点。同类对象无法共享同一个方法（Function对象需要重新生成，赋值。）而若使用全局函数变量，将扰乱全局作用域。

#### 8.2.4 原型模式(原型链)

每个函数都会创建一个prototype属性，这个属性是一个对象，包含应该由特定引用类型的实例共享的属性和方法。即原型。

##### 01.理解原型

无论何时，只要创建一个函数，就会按照特定的规则为这个函数创建一个prototype属性（指向原型对象）。默认情况下，所有原型对象自动获得一个名为constructor的对象，指回与之关联的构造函数。然后因构造函数而异，可能会给原型对象添加其他属性和方法。

当使用构造函数创建一个新的实例时，实例的[[Prototype]]指针会指向构造函数的原型对象。在一般情况下，是无法进行访问的，但是在Chorme和Safari中有暴露这个接口，即__proto__属性

可以以可视化的方式进行理解，具体如下：

![3](.\3.PNG)

虽然在标准接口中并没有暴露[[Prototype]]属性，但是可以通过isPrototypeOf()接口来确定原型对象和实例对象的继承关系。

同时可以使用getPrototypeOf()来获取原型对象，或者setPrototypeOf()来修改继承关系。

为了避免setPrototype()可能造成的性能下降，可以选择Object.create()来创建一个新对象，同时为其指定原型。

##### 02.原型层级

在通过对象访问属性时，会按照这个属性的名称开始搜索。搜索开始于实例对象本身。如果没有找到这个属性，会沿着[[Prototype]]指针进入原型对象，并重复之前的查找操作。

注意：同名属性，实例对象上的值会掩盖原型对象上的值。

hasOwnProperty()方法用于确定某个属性是在实例上还是实例对象上。

```js
let obj={};
//hasOwnProperty(prop:string)
console.log(obj.hasOwnProperty("name"));

```



 注意：Object.getOwnPropertyDescriptor()方法只对实例属性有效。

##### 03. 原型和in操作符

有两种方法使用in操作符：**单独使用**和**在for-in循环中使用**

在单独使用时，in操作符会在可以通过对象访问指定属性时返回true，无论属性是在实例还是原型上。

在for-in循环中使用in操作符，可以通过对象访问且可以被枚举的属性都会放回（即[[Enumerable]]属性为True）;

如果需要列出所有的实例属性，无论是否可以枚举，可以使用Object.getOwnPropertyNames()获取属性名。

在出现Symbol类型后，出现了获取符号属性的属性。Object.getOwnPropertySymbols。

##### 04.属性枚举顺序

1、for-in循环顺序不确定。

2、Object.getOwnPropertyNames()、Object.getOwnPropertySymbols(),Object.assign()的枚举顺序是确定性的。**先以升序枚举数值键，然后以插入顺序枚举字符串和符号键。**

#### 8.2.5 对象迭代

ECMAScript新增了两个静态方法，Object.values()和Object.entries()。values()返回对象值的数组，entries()返回键值对的数组。

注意，非字符串属性会被转换成字符互传输出。同时，这两个方法执行的是对象的浅克隆。符号属性会被忽略

##### 01.其他原型语法

可以使用字面量去修改原型，但是这样会导致，constructor属性的[[Enumerable]]属性的错误。

推荐修改写法：

```js
function Person() {}
Person.prototype = {
	name: "Nicholas",
	age: 29,
	job: "Software Engineer",
	sayName() {
		console.log(this.name);
	}
};
// 恢复constructor属性
Object.defineProperty(Person.prototype, "constructor", {
	enumerable: false,
	value: Person
});
```

##### 02.原型的动态性

在实例对象中,[[Prototype]]属性存放的是指向原型对象的指针，因此如果先创建了示例对象，在修改构造函数的prototype属性，实例对象的指针将不会指向新原型对象。

##### 03.原生对象原型

不建议对原生对象原型进行修改。

##### 04.原型的问题

1、弱化了向构造函数传递初始化参数的能力

2、共享特性导致引用值属性的随意改写。

### 8.3 继承

在ECMAScript中，只支持实现继承。这主要是通过原型链实现。

#### 8.3.1原型链

原型链基本思想是通过原型继承多个引用类型的属性和方法。

每个构造函数有一个原型对象，原型有一个属性（constructor)指回构造函数，而实例有一个内部指针指向原型。

如果原型也是另一个类型的实例，那么意味着也会有对应的构造函数和原型对象。

这样就在实例和原型之间构造了一条原型链。

实现原型链涉及如下代码模式

```js
function SuperType() {
	this.property = true;
} 
SuperType.prototype.getSuperValue = function() {
return this.property;
};
function SubType() {
	this.subproperty = false;
} /
/ 继承SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function () {
return this.subproperty;
};
let instance = new SubType();
console.log(instance.getSuperValue()); // true
```

以上代码定义了两个类型： SuperType和SubType。 这两个类型分别定义了一个属性和一个方法。 这两个类型的主要区别是SubType通过创建SuperType的实例并将其赋值给自己的原型SubType.prototype实现了对SuperType的继承。 这个赋值重写了SubType最初的原型， 将其替换为SuperType的实例。 这意味着SuperType实例可以访问的所有属性和方法也会存在于SubType.prototype。 这样实现继承之后， 代码紧接着又给
SubType.prototype， 也就是这个SuperType的实例添加了一个新方法。最后又创建了SubType的实例并调用了它继承的getSuperValue()方法。
下图展示了子类的实例与两个构造函数及其对应的原型之间的关系。

![原型链](.\原型链.PNG) 

##### 01.默认原型

实际上， 原型链中还有一环。 默认情况下， 所有引用类型都继承自
Object， 这也是通过原型链实现的。 任何函数的默认原型都是一
个Object的实例， 这意味着这个实例有一个内部指针指向Object.prototype。 这也是为什么自定义类型能够继承包括toString()、 valueOf()在内的所有默认方法的原因。 因此前面的例子还有额外一层继承关系。 下图![4](.\4.PNG)展示了完整的原型链。



### 8.4 类

## 第10章 函数

### 10.1 箭头函数

箭头函数虽然语法简洁，但是也有很多场合不适用。

箭头函数不能使用arguments，super和new.target，也不能用作构造函数。此外，箭头函数没有prototype属性



