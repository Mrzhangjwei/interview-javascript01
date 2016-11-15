### 1.介绍JavaScript的基本数据类型?

 JavaScript的基本类型是: Undefined、Null、Boolean、Number、String

 详细讲解: http://www.jianshu.com/p/4841fcc6b4e7

### 2.JavaScript原型,原型链?有什么特点?

 每个对象都会在其内部初始化一个属性，就是prototype(原型)，当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，
于是就这样一直找下去，也就是我们平时所说的原型链的概念。
关系：`instance.constructor.prototype = instance.__proto__`
//
特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本，当我们修改原型时，与之相关的对象也会继承这一改变。
//
当我们需要一个属性时，JavaScript引擎会先看当前对象中是否有这个属性，如果没有的话，就会查找它的prototype对象是否有这个属性，如此递推下去，一致检索到Object内建对象。
```
function Func(){}
Func.prototype.name = "Xiaosong";
Func.prototype.getInfo = function() {
  return this.name;
}
var person = new Func();
console.log(person.getInfo());
//"Xiaosong"
console.log(Func.prototype);
//Func { name = "Xiaosong", getInfo = function() }
```

### 3.JavaScript如何实现继承?

- (1)构造继承
- (2)原型继承
- (3)实例继承
- (4)拷贝继承
//
原型prototype机制或apply和call方法去实现较简单，建议使用构造函数与原型混合方式。
```
function Parent() {
  this.name = 'song';
}
function Child() {
  this.age = 28;
}
Child.prototype = new Parent(); //通过原型,继承了Parent
//
var demo = new Child()l;
alert(demo.age);
alert(demo.name); //得到被继承的属性
```

### 4.JavaScript创建对象的几种方式?

 javascript创建对象简单的说,无非就是使用内置对象或各种自定义对象，当然还可以用JSON；但写法有很多种，也能混合使用。

- (1)对象字面量的方式
`person={firstname:"Mark",lastname:"Yun",age:25,eyecolor:"black"};`
- (2)用function来模拟无参的构造函数
```
function Person(){}
var person = new Person(); //定义一个function，如果使用new"实例化",该function可以看作是一个Class
person.name = "Xiaosong";
person.age = "23";
person.work = function() {
  alert("Hello " + person.name);
}
person.work();
```

- (3)用function来模拟参构造函数来实现（用this关键字定义构造的上下文属性）
```
function Person(name,age,hobby) {
  this.name = name; //this作用域：当前对象
  this.age = age;
  this.work = work;
  this.info = function() {
      alert("我叫" + this.name + "，今年" + this.age + "岁，是个" + this.work);
  }
}
var Xiaosong = new Person("WooKong",23,"程序猿"); //实例化、创建对象
Xiaosong.info(); //调用info()方法
```

- (4)用工厂方式来创建（内置对象）
```
var jsCreater = new Object();
jsCreater.name = "Brendan Eich"; //JavaScript的发明者
jsCreater.work = "JavaScript";
jsCreater.info = function() {
  alert("我是"+this.work+"的发明者"+this.name);
}
jsCreater.info();
```

- (5)用原型方式来创建
```
function Standard(){}
Standard.prototype.name = "ECMAScript";
Standard.prototype.event = function() {
  alert(this.name+"是脚本语言标准规范");
}
var jiaoben = new Standard();
jiaoben.event();
```

- (6)用混合方式来创建
```
function iPhone(name,event) {
  this.name = name;
  this.event = event;
}
iPhone.prototype.sell = function() {
  alert("我是"+this.name+"，我是iPhone5s的"+this.event+"~ haha!");
}
var SE = new iPhone("iPhone SE","官方翻新机");
SE.sell();
```

###　5.JavaScript作用域链?

 JavaScript中所有的量都是存在于某一个作用域中的
 除了全局作用域, 每一个作用域都是存在於某个作用域中的
 在试图访问一个变量时JS引擎会从当前作用域开始向上查找直到Global全局作用域停止
例如
```
var A;//全局作用域
function B()
{
    var C;//C位于B函数的作用域
    function D()
    {
        var E;//E位于D函数的作用域
        alert(A)
    }
}
```


当alert(A)时, JS引擎沿着D的作用域, B的作用域, 全局作用域的顺序进行查找.
这三个作用域组成的有序集合就成为作用域链
至于为什么叫链, 你可以理解和链表有相似之处, 深层的作用域会能够访问到上层作用域, 就如同链表中两个连续节点能够单向访问一样

### 6.谈谈this对象的理解。

 详细讲解: http://www.cnblogs.com/nuanriqingfeng/p/5794632.html
 
 this是函数运行时自动生成的一个内部对象，只能在函数内部使用，但总指向调用它的对象。

通过以下几个例子加深对this的理解。

- （1）作为函数调用
```
var name = 'Jenny';
function person() {
    return this.name;
}
console.log(person());  //Jenny
```

上面这个例子在全局作用域中调用person()，此时的调用对象为window，因此this指向window，在window中定义了name变量，因此this.name相当于window.name，为Jenny。

- （2）作为对象的方法调用
```
var name = 'Jenny';
var obj = {
    name: 'Danny',
    person: function() {
        return this.name;
    }
};
console.log(obj.person());  //Danny
```

在这个例子中，person()函数在obj对象中定义，调用时是作为obj对象的方法进行调用，因此此时的this指向obj，obj里面定义了name属性，值为Danny，因此this.name = "Danny"。

- （3）作为构造函数调用

作为构造函数调用和普通函数调用的区别是，构造函数使用new关键字创建一个实例，此时this指向实例对象。

```
function person() {
    return new person.prototype.init();
}
person.prototype = {
    init: function() {
        return this.name;
    },
    name: 'Brain'
};
console.log(person().name); //undefined
```

- （4）apply和call

使用apply()和call()可以改变调用函数的对象，第一个参数为改变后调用这个函数的对象，其中apply()的第二个参数为一个数组，call的第二个参数为每个参数。

```
function person() {
    return this.name;
}
var obj = {
    name: 'Jenny',
    age: 18
};
console.log(person.apply(obj));  //Jenny
```

### 7.什么是window对象? 什么是document对象?

 详情请看参考资料: http://www.w3school.com.cn/jsref/dom_obj_window.asp

 简单来说，document是window的一个对象属性。
Window 对象表示浏览器中打开的窗口。
如果文档包含框架（frame 或 iframe 标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象。
所有的全局函数和对象都属于Window 对象的属性和方法。
document   对 Document 对象的只读引用。

### 8.null 和 undefined 有何区别？

 null        表示一个对象被定义了，值为“空值”；
undefined   表示不存在这个值。

- typeof undefined
  //"undefined"
  undefined :是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是还没有定义。当尝试读取时会返回 undefined； 
  例如变量被声明了，但没有赋值时，就等于undefined。
- typeof null
  //"object"
  null : 是一个对象(空对象, 没有任何属性和方法)；
  例如作为函数的参数，表示该函数的参数不是对象；

注意：
  在验证null时，一定要使用　=== ，因为 == 无法分别 null 和　undefined

### 9.写一个通用的事件侦听器函数(机试题)。

```
markyun.Event = {
  //页面加载完成后
  readyEvent: function(fn) {
      if (fn == null) {
          fn = document;
      }
      var oldonload = window.onload;
      if (typeof window.onload != 'function') {
          window.onload = fn;
      }else{
          window.onload = function() {
              oldonload();
              fn();
          };
      }
  },
  //视能力分别使用 demo0 || demo1 || IE 方式来绑定事件
  //参数：操作的元素，事件名称，事件处理程序
  addEvent: function(element,type,handler) {
      if (element.addEventListener) {
          //事件类型、需要执行的函数、是否捕捉
          element.addEventListener(type,handler,false);
      }else if (element.attachEvent) {
          element.attachEvent('on' + type, function() {
              handler.call(element);
          });
      }else {
          element['on' + type] = handler;
      }
  },
  //移除事件
  removeEvent: function(element,type,handler) {
      if (element.removeEventListener) {
          element.removeEventListener(type,handler,false);
      }else if (element.datachEvent) {
          element.datachEvent('on' + type,handler);
      }else{
          element['on' + type] = null;
      }
  },
  //阻止事件（主要是事件冒泡，因为IE不支持事件捕获）
  stopPropagation: function(ev) {
      if (ev.stopPropagation) {
          ev.stopPropagation();
      }else {
          ev.cancelBubble = true;
      }
  },
  //取消事件的默认行为
  preventDefault: function(event) {
      if (event.preventDefault) {
          event.preventDefault();
      }else{
          event.returnValue = false;
      }
  },
  //获取事件目标
  getTarget: function(event) {
      return event.target || event.srcElemnt;
  },
  //获取event对象的引用，取到事件的所有信息，确保随时能使用event；
  getEvent: function(e) {
      var ev = e || window.event;
      if (!ev) {
          var c = this.getEvent.caller;
          while(c) {
              ev = c.argument[0];
              if (ev && Event == ev.constructor) {
                  break;
              }
              c = c.caller;
          }
      }
      retrun ev;
  }
};
```

### 10.["1", "2", "3"].map(parseInt) 答案是多少？

 [1,NaN,NaN]
 
因为 parseInt 需要两个参数(val,radix)，其中 radix 表示解析时用的基数。
map 传了3个(element,index,array)，对应的 radix 不合法导致解析失败。

### 11.什么是闭包（closure），为什么要用它？

 闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量，利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。

闭包特性：

- (1)函数内再嵌套函数
- (2)内部函数可以引用外层的参数和变量
- (3)参数和变量不会被垃圾回收机制回收

```
//li节点的onclick事件都能正确的弹出当前被点击的li索引
<ul>
  <li> index = 0 </li>
  <li> index = 1 </li>
  <li> index = 2 </li>
  <li> index = 3 </li>
</ul>
<script type="text/javascript">
  var nodes = document.getElementsByTagName('li');
  for(i = 0;i<nodes.length;i+=1) {
      nodes[i].onclick = function() {
          console.log(i+1); //不使用闭包的话，值每次都是4
      }(4);
  }
</script>
```

### 12.new操作符具体干了什么呢?

- (1)创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
- (2)属性和方法被加入到 this 引用的对象中。
- (3)新创建的对象由 this 所引用，并且最后隐式的返回 this 。
```
var obj = {};
obj.__proto__ = Base.prototype;
Base.call(obj);
```

### 13.用原生JavaScript的实现过什么功能吗？

 原生JavaScript能实现的功能,详情: http://bbs.csdn.net/topics/390565578

### 14.Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

 这个函数是: `hasOwnProperty`
//
JavaScript 中 hasOwnProperty 函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。
//
使用方法：
object.hasOwnProperty(proName)
其中参数object是必选项，一个对象的实例。
proName是必选项，一个属性名称的字符串值。
//
如果 object 具有指定名称的属性，那么JavaScript中hasOwnProperty函数方法返回 true，反之则返回 false。

### 15.对JSON的了解？

 JSON(JavaScript Object Notation)是一种轻量级的数据交换格式。
它是基于JavaScript的一个子集。数据格式简单，易于读写，占用带宽小。

如： `{"age":"12", "name":"back"}`

### 16.Ajax 是什么? 如何创建一个Ajax？

 ajax的全称：Asynchronous Javascript And XML。
异步传输+js+xml。
所谓异步，在这里简单地解释就是：向服务器发送请求的时候，我们不必等待结果，而是可以同时做其他的事情，等到有了结果它自己会根据设定进行后续操作，与此同时，页面是不会发生整页刷新的，提高了用户体验。

- (1)创建XMLHttpRequest对象,也就是创建一个异步调用对象
- (2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息
- (3)设置响应HTTP请求状态变化的函数
- (4)发送HTTP请求
- (5)获取异步调用返回的数据
- (6)使用JavaScript和DOM实现局部刷新

### 17.DOM操作——怎样添加、移除、移动、复制、创建和查找节点?

- (1)创建新节点
createDocumentFragment()    //创建一个DOM片段
createElement()   //创建一个具体的元素
createTextNode()   //创建一个文本节点

- (2)添加、移除、替换、插入
appendChild()
removeChild()
replaceChild()
insertBefore() //在已有的子节点前插入一个新的子节点

- (3)查找
getElementsByTagName()    //通过标签名称
getElementsByName()    //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)
getElementById()    //通过元素Id，唯一性

### 18 .call() 和 .apply() 的作用和区别？

 1、call，apply都属于Function.prototype的一个方法，它是JavaScript引擎内在实现的，因为属于Function.prototype，所以每个Function对象实例(就是每个方法)都有call，apply属性。既然作为方法的属性，那它们的使用就当然是针对方法的了，这两个方法是容易混淆的，因为它们的作用一样，只是使用方式不同。
 
2、语法：foo.call(this, arg1,arg2,arg3) == foo.apply(this, arguments) == this.foo(arg1, arg2, arg3);

3、相同点：两个方法产生的作用是完全一样的。

4、不同点：方法传递的参数不同。

### 19.数组对象有哪些原生方法，列举一下？

 详细用法参考: http://www.jb51.net/article/34711.htm

 赋值方法 (Mutator methods): 这些方法直接修改数组自身.
 
 - pop 和 push 
 - shift 和 unshift 
 - splice 
 - reverse 
 - sort 

访问方法(Accessor methods) : 这些方法只是返回相应的结果，而不会修改数组本身 ;

- concat 
- join 
- slice
- toString 
- indexOf 和 lastIndexOf

迭代方法（Iteration methods）:

- forEach 
- map
- filter 
- every 和 some
- reduce 和 reduceRight

### 20.JavaScript中的作用域与变量声明提升？

#### 作用域

 作用域详情: http://www.cnblogs.com/lhb25/archive/2011/09/06/javascript-scope-chain.html

 任何程序设计语言都有作用域的概念，简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。在JavaScript中，变量的作用域有全局作用域和局部作用域两种。

#### 变量声明，命名，和提升

 变量声明提升详情: http://blog.csdn.net/sunxing007/article/details/9034253
 
在javascript，变量有4种基本方式进入作用域：

- 1.语言内置：所有的作用域里都有this和arguments；(译者注：经过测试arguments在全局作用域是不可见的)
- 2.形式参数：函数的形式参数会作为函数体作用域的一部分；
- 3.函数声明：像这种形式：function foo(){}；
- 4.变量声明：像这样：var foo；

