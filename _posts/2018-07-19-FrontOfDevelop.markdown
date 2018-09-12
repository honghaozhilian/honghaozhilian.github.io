---
layout:     post
title:      "javascript"
subtitle:   "面试题归纳"
date:       2018-07-19
author:     "honghaozhilian"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
---



## javascript

> 主要是javascript方面的

# 目录
- [offsetwidth和clientwidth区别](#offsetwidth和clientwidth区别)
- [js有几种数据类型](#js有几种数据类型)
- [null和undefined的区别](#null和undefined的区别)
- [js的原型和原型链](#js的原型和原型链)
- [js创建对象的几种方式](#js创建对象的几种方式)
- [js的继承](#js的继承)
- [new操作符具体干了什么](#new操作符具体干了什么)
- [闭包的概念及应用场景](#闭包的概念及应用场景)
- [js延迟加载的方式](#js延迟加载的方式)
- [如何创建一个ajax](#如何创建一个ajax)
- [如何解决跨域的问题](#如何解决跨域的问题)
- [页面编码和被请求的资源编码如果不一致如何处理](#页面编码和被请求的资源编码如果不一致如何处理)
- [AMD和CMD规范区别](#AMD和CMD规范区别)
- [cmd的核心原理是什么](#cmd的核心原理是什么)
- [commonjs与es6模块的区别](#commonjs与es6模块的区别)
- [事件的捕获冒泡的应用场景](#事件的捕获冒泡的应用场景)
- [编写一个工具类实现事件管理兼容](#编写一个工具类实现事件管理兼容)
- [事件节流与防抖动](#事件节流)
- [js作用域和变量声明提升](#js作用域和变量声明提升)
- [call和apply的区别](#call和apply的区别)
- [bind函数的实现](#bind函数的实现)
- [运算符的优先级](#运算符的优先级)
- [==的判断过程](#==的判断过程)
- [对象到字符串的转换](#对象到字符串的转换)
- [对象到数字的转换](#对象到数字的转换)
- [哪些操作会造成内存泄漏](#哪些操作会造成内存泄漏)
- [promise相关概念](#promise相关概念)
- [set与map结构](#set与map结构)
- [async与await的应用](#async与await的应用)
- [es6的迭代器](#es6的迭代器)

## offsetwidth和clientwidth区别
> - offsetWidth/offsetHeight返回值包含content + padding + border，效果e.getBoundingClientRect()相同
> - clientWidth/clientHeight返回值只包含content + padding，如果有滚动条，也不包含滚动条
> - scrollWidth/scrollHeight返回值包含content + padding + 溢出内容的尺寸

## js有几种数据类型
>  基本数据类型：number、string、boolean、undefined、null(空指针，不知道是否能算作基本类型？)、symbol;引用类型：Object

## null和undefined的区别
> null代表一个空指针对象，在栈中存放了一个指针却没有指向堆中的任何空间；undefined代表声明了却未初始化的值，在栈中占有空间却没有任何值

## js的原型和原型链
> 每个对象都会在其内部初始化一个属性指向__proto__(原型)，当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去原型里找这个属性，如此就形成了原型链<br/>
> 关系：instance.constructor.prototype = instance.__proto__<br/>
> __proto__和prototype的区别：<br/>
> -  __proto__: 每个JS对象一定对应一个原型对象，并从原型对象继承属性和方法;
> -  prototype:不像每个对象都有__proto__属性来标识自己所继承的原型，只有函数才有prototype属性;当你创建函数时，JS会为这个函数自动添加prototype属性，值是一个有 constructor 属性的对象，不是空对象,而一旦你把这个函数当作构造函数（constructor）调用（即通过new关键字调用），那么JS就会帮你创建该构造函数的实例，实例继承构造函数prototype的所有属性和方法（实例通过设置自己的__proto__指向继承构造函数的prototype来实现这种继承）。
<img src="{{ site.baseurl }}/img/20180719_01.jpg" alt="原型"/>


## js创建对象的几种方式
1. 生成实例对象的原始模式(对象字面量)
   - 缺点：一是如果多生成几个实例，写起来就非常麻烦；二是实例与原型之间，没有任何办法，可以看出有什么联系。
   - 改进：用一个函数返回对象，function Foo(obj) { return {...obj};}
2. 构造函数模式
    - 无参数：

            function Person(){}
            var person=new Person();//定义一个function，如果使用new"实例化",该function可以看作是一个Class
            person.name="Mark";
            person.age="25";
            person.work=function(){
            alert(person.name+" hello...");
            }
            person.work();

    - 有参数：

            function Pet(name,age,hobby){
                this.name=name;//this作用域：当前对象
                this.age=age;
                this.hobby=hobby;
                this.eat=function(){
                    alert("我叫"+this.name+",我喜欢"+this.hobby+",是个程序员");
                }
            }
            var maidou =new Pet("麦兜",25,"coding");//实例化、创建对象
        
   - 缺点：对于每一个实例对象的共同属性或方法，每一次生成一个实例，都必须为重复的内容，多占用一些内存。这样既不环保，也缺乏效率。
3. Prototype模式
    - 每一个构造函数都有一个prototype属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数的实例继承。

            function Dog(){

            }
            Dog.prototype.name="旺财";
            Dog.prototype.eat=function(){
                alert(this.name+"是个吃货");
            }
            var wangcai =new Dog();
            
4. 工厂模式

        var wcDog =new Object();
        wcDog.name="旺财";
        wcDog.age=3;
        wcDog.work=function(){
            alert("我是"+wcDog.name+",汪汪汪......");
        }

5. 用混合方式来创建

        function Car(name,price){
            this.name=name;
            this.price=price;
        }
        Car.prototype.sell=function(){
            alert("我是"+this.name+"，我现在卖"+this.price+"万元");
        }
        var camry =new Car("凯美瑞",27);


## js的继承
- 构造函数继承
    1. 构造函数绑定
        - 使用call或apply方法，将父对象的构造函数绑定在子对象上，即在子对象构造函数中加一行

                function Cat(name,color){
                    Animal.apply(this, arguments);
                    this.name = name;
                    this.color = color;
                }
                var cat1 = new Cat("大毛","黄色");

        - 优点:父类的引用属性不会被共享,子类构建实例时可以向父类传递参数;缺点：父类的方法不能复用，子类实例的方法每次都是单独创建的。

    2. prototype模式
        - 将所有子对象的prototype对象，指向一个要继承的父对象的实例

                Cat.prototype = new Animal();
                Cat.prototype.constructor = Cat;
                var cat1 = new Cat("大毛","黄色");

        - 缺点：1.父类实例属性为引用类型时，不恰当地修改会导致所有子类被修改；2.创建父类实例作为子类原型时，可能无法确定构造函数需要的合理参数，这样提供的参数继承给子类没有实际意义，当子类需要这些参数时应该在构造函数中进行初始化和设置(子类构建实例时不能向父类传递参数)

    3. 直接继承prototype
        - 对prototype模式的改进。由于Animal对象中，不变的属性都可以直接写入Animal.prototype。所以我们也可以让Cat()跳过 Animal()，直接继承Animal.prototype。

                function Animal(){ }
                Animal.prototype.species = "动物";

                Cat.prototype = Animal.prototype;
                Cat.prototype.constructor = Cat;//实际上将Animal.prototype的constructor也改了
                var cat1 = new Cat("大毛","黄色");
        
        - 优点是效率比较高（不用执行和建立Animal的实例了），比较省内存。缺点是 Cat.prototype和Animal.prototype现在指向了同一个对象，那么任何对Cat.prototype的修改，都会反映到Animal.prototype。上面的代码把Animal.prototype对象的constructor属性也改掉了

    4. 组合继承
        - 原型式继承和构造函数继承的组合，兼具了二者的优点。

                function SuperType() {   
                    this.name = 'parent';    
                    this.arr = [1, 2, 3];
                }
                SuperType.prototype.say = function() { 
                    console.log('this is parent')
                }
                function SubType() {    
                    SuperType.call(this) // 第二次调用SuperType
                }
                SubType.prototype = new SuperType() // 第一次调用SuperType

            - 优点：父类的方法可以被复用，父类的引用属性不会被共享，子类构建实例时可以向父类传递参数；缺点：调用了两次父类的构造函数，第一次给子类的原型添加了父类的name, arr属性，第二次又给子类的构造函数添加了父类的name, arr属性，从而覆盖了子类原型中的同名参数。这种被覆盖的情况造成了性能上的浪费。

    5. 利用空对象作为中介（Object.create()）

            var F = function(){};
            F.prototype = Animal.prototype;
            Cat.prototype = new F();
            Cat.prototype.constructor = Cat;
        
        - 解决了直接继承prototype的缺点，空对象几乎不占内存，所以也没有内存问题，缺点：父类的引用属性会被所有子类实例共享，子类构建实例时不能向父类传递参数

    6. 拷贝继承

            function Animal(){}
            Animal.prototype.species = "动物";

            function extend2(Child, Parent) {
                var p = Parent.prototype;
                var c = Child.prototype;
                for (var i in p) {
                    c[i] = p[i];
                }
                c.uber = p;
            }
            extend2(Cat, Animal);
            var cat1 = new Cat("大毛","黄色");

    7. 寄生组合继承

            function inheritPrototype(subType, superType){    
                var prototype = Object.create(superType.prototype); // 创建了父类原型的浅复制
                prototype.constructor = subType;             // 修正原型的构造函数   
                subType.prototype = prototype;               // 将子类的原型替换为这个原型
            }
            function SuperType(name){    
                this.name = name;    
                this.colors = ["red", "blue", "green"];
            }
            SuperType.prototype.sayName = function(){    
                alert(this.name);
            };
            function SubType(name, age){    
                SuperType.call(this, name);    
                this.age = age;
            }
            // 核心：因为是对父类原型的复制，所以不包含父类的构造函数，也就不会调用两次父类的构造函数造成浪费
            inheritPrototype(SubType, SuperType);
            SubType.prototype.sayAge = function(){
                alert(this.age);
            }

    8. ES6 Class extends
        - ES6继承的结果和寄生组合继承相似，本质上，ES6继承是一种语法糖。但是，寄生组合继承是先创建子类实例this对象，然后再对其增强；而ES6先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。
        - 不同：ES6继承中子类的构造函数的原型链指向父类的构造函数，ES5中使用的是构造函数复制，没有原型链指向。ES6子类实例的构建，基于父类实例，ES5中不是

- 非构造函数继承（比如两个普通对象之间如何实现继承）
    1. object()方法(即Object.create()的兼容写法)

            function object(o) {
                function F() {}
                F.prototype = o;
                return new F();
            }

    2. 浅拷贝与深拷贝
        - 把父对象的属性，全部拷贝给子对象，也能实现继承。

                function extendCopy(p) {
                    var c = {};
                    for (var i in p) { 
                        c[i] = p[i];
                    }
                    c.uber = p;
                    return c;
                }

                function deepCopy(p, c) {
                    var c = c || {};
                    for (var i in p) {
                        if (typeof p[i] === 'object') {
                    　　　　c[i] = (p[i].constructor === Array) ? [] : {};
                    　　　　deepCopy(p[i], c[i]);
                    　　} else {
                    　　　　　c[i] = p[i];
                    　　}
                    }
                    return c;
                }

## new操作符具体干了什么
1. 创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
2. 属性和方法被加入到 this 引用的对象中。
3. 新创建的对象由 this 所引用，并且最后隐式的返回 this 。

        var obj  = {};
        obj.__proto__ = Base.prototype;
        Base.call(obj);


## 闭包的概念及应用场景
- 闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量,利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。
- 作用：1、创建特权方法用于访问控制；2、事件处理程序及回调

## js延迟加载的方式
- defer和async、动态创建DOM方式（用得最多）、按需异步载入js

## 如何创建一个ajax

        var xhr = new XMLHttpRequest();
        open(method, url, asynchronous [, user, password])
        xhr.onreadyStateChange = function(data){
            if(xhr.status == 200 && xhr.readyState == 4){
                console.log(xhr.responseText)//作为字符串形式的来自服务器的完整响应
                console.log(xhr.responseXML)//Document对象，表示服务器的响应解析成的XML文档
            }
        };
        xhr.setRequestHeader(key,value);
        xhr.send(body);
        /*
        xhr.abort()//取消异步HTTP请求
        xhr.getAllResponseHeaders()//返回一个字符串，包含响应中服务器发送的全部HTTP报头。每个报头都是一个用冒号分隔开的名/值对，并且使用一个回车/换行来分隔报头行
        xhr.getResponseHeader(headerName)//返回headName对应的报头值
        */

## 如何解决跨域的问题
- 同源策略：协议相同，域名相同，端口相同
- 跨域通信：js进行DOM操作、通信时如果目标与当前窗口不满足同源条件，浏览器为了安全会阻止跨域操作。
- 常见方法：
    - 如果是log之类的简单单项通信，新建img,script,link,iframe元素，通过src，href属性设置为目标url。实现跨域请求
    - 如果请求json数据，使用script进行jsonp请求
    - 现代浏览器中多窗口通信使用HTML5规范的targetWindow.postMessage(data, origin);其中data是需要发送的对象，origin是目标窗口的origin。window.addEventListener('message', handler, false);handler的event.data是postMessage发送来的数据，event.origin是发送窗口的origin，event.source是发送消息的窗口引用
    - 内部服务器代理请求跨域url，然后返回数据
    - 跨域请求数据，现代浏览器可使用HTML5规范的CORS功能，只要目标服务器返回HTTP头部**Access-Control-Allow-Origin: ***即可像普通ajax一样访问跨域资源

## 页面编码和被请求的资源编码如果不一致如何处理
- 检查meta是否设置charset,在非utf-8的资源引用加属性charset="utf-8"

## AMD和CMD规范区别
1. 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.
2. CMD 推崇依赖就近，AMD 推崇依赖前置。

        // CMD
        define(function(require, exports, module) {
            var a = require('./a')
            a.doSomething()
            // 此处略去 100 行
            var b = require('./b') // 依赖可以就近书写
            b.doSomething()
            // ...
        })
        // AMD 默认推荐
        define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好
            a.doSomething()
            // 此处略去 100 行
            b.doSomething()
            // ...
        })


## cmd的核心原理是什么
- requirejs与之相似
1. 首先，通过 use 方法来加载入口模块，并接收一个回调函数， 当模块加载完成， 会调用回调函数，并传入对应的模块。use 方法会 check 模块有没有缓存，如果有，则从缓存中获取模块，如果没有，则创建并加载模块。
2. 获取到模块后，模块可能还没有 load 完成，所以需要在模块上绑定一个 “complete” 事件，模块加载完成会触发这个事件，这时候才调用回调函数。
3. 创建一个模块时，id就是模块的地址，通过创建 script 标签的方式异步加载模块的代码（factory），factory 加载完成后，会 check factory 中有没有 require 别的子模块:
>   - 如果有，继续加载其子模块，并在子模块上绑定 “complete” 事件，来触发本身 的 “complete” 事件；
>   - 如果没有则直接触发本身的 “complete” 事件。
>   - 如果子模块中还有依赖，则会递归这个过程。
4. 通过事件由里到外的传递，当所有依赖的模块都 complete 的时候，最外层的入口模块才会触发 “complete” 事件，use 方法中的回调函数才会被调用。
<img src="{{ site.baseurl }}/img/cmd.png" alt="cmd原理"/>

> 相关文档：http://annn.me/how-to-realize-cmd-loader/

## commonjs与es6模块的区别
- commonJS：
    1. 对于基本数据类型，属于复制，引入模块后改变不会影响另一个模块，对于引用类型属于浅拷贝
    2. 当使用require命令加载某个模块时，运行时才会加载，而且是同步加载
    3. 当使用require命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
    4. 循环加载时，属于加载时执行。即脚本代码在require的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。

            // b.js
            exports.done = false
            let a = require('./a.js')
            console.log('b.js-1', a.done)
            exports.done = true
            console.log('b.js-2', '执行完毕')

            // a.js
            exports.done = false
            let b = require('./b.js')
            console.log('a.js-1', b.done)
            exports.done = true
            console.log('a.js-2', '执行完毕')

            // c.js
            let a = require('./a.js')
            let b = require('./b.js')

            console.log('c.js-1', '执行完毕', a.done, b.done)

            node c.js
            b.js-1 false
            b.js-2 执行完毕
            a.js-1 true
            a.js-2 执行完毕
            c.js-1 执行完毕 true true

- es6
    1. ES6模块中的值属于【动态只读引用】
    2. 对于只读来说，即不允许修改引入变量的值，import的变量是只读的，不论是基本数据类型还是复杂数据类型。当模块遇到import命令时，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。
    3. import命令具有提升效果，会提升到整个模块的头部，首先执行,本质是import命令是编译阶段执行的，在代码运行之前。由于编译阶段执行所以import不能放于if等条件模块下作为动态加载，现有import()方法的提案来实现动态加载，与commonjs的require方法不同，此方法是异步加载
    4. 对于动态来说，原始值发生变化，import加载的值也会发生变化。不论是基本数据类型还是复杂数据类型。
    5. 循环加载时，ES6模块是动态引用。只要两个模块之间存在某个引用，代码就能够执行。

            // b.js
            import {foo} from './a.js';
            export function bar() {
                console.log('bar');
                if (Math.random() > 0.5) {
                    foo();
                }
            }

            // a.js
            import {bar} from './b.js';
            export function foo() {
                console.log('foo');
                bar();
                console.log('执行完毕');
            }
            foo();

            babel-node a.js
            foo
            bar
            执行完毕

            // 执行结果也有可能是
            foo
            bar
            foo
            bar
            执行完毕
            执行完毕

## 事件的捕获冒泡的应用场景
- addEventlistener的第三个参数决定事件触发应该在捕获阶段还是冒泡阶段（默认是冒泡阶段）
- 事件冒泡可用于事件委托，将事件绑定在父元素下，点击目标元素通过冒泡到父元素，判断目标元素触发相关事件，好处：提升性能（不需要绑定过多的事件），对于动态添加的元素事件绑定同样有效
- 事件捕获可用于父元素触发事件，而子元素不触发事件的时候


## 编写一个工具类实现事件管理兼容
        var EventUtil = {
            // 页面加载完成后
            readyEvent : function(fn) {
                if (fn==null) {
                    fn=document;
                }
                var oldonload = window.onload;
                if (typeof window.onload != 'function') {
                    window.onload = fn;
                } else {
                    window.onload = function() {
                        oldonload();
                        fn();
                    };
                }
            },
            // 视能力分别使用dom0||dom2||IE方式 来绑定事件
            // 参数： 操作的元素,事件名称 ,事件处理程序
            addEvent : function(element, type, handler) {
                if (element.addEventListener) {
                    //事件类型、需要执行的函数、是否捕捉
                    element.addEventListener(type, handler, false);
                } else if (element.attachEvent) {
                    element.attachEvent('on' + type, function() {
                        handler.call(element);
                    });
                } else {
                    element['on' + type] = handler;
                }
            },
            // 移除事件
            removeEvent : function(element, type, handler) {
                if (element.removeEventListener) {
                    element.removeEventListener(type, handler, false);
                } else if (element.datachEvent) {
                    element.detachEvent('on' + type, handler);
                } else {
                    element['on' + type] = null;
                }
            },
            // 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
            stopPropagation : function(ev) {
                if (ev.stopPropagation) {
                    ev.stopPropagation();
                } else {
                    ev.cancelBubble = true;
                }
            },
            // 取消事件的默认行为
            preventDefault : function(event) {
                if (event.preventDefault) {
                    event.preventDefault();
                } else {
                    event.returnValue = false;
                }
            },
            // 获取事件目标
            getTarget : function(event) {
                return event.target || event.srcElement;
            },
            // 获取event对象的引用，取到事件的所有信息，确保随时能使用event；
            getEvent : function(e) {
                var ev = e || window.event;
                if (!ev) {
                    var c = this.getEvent.caller;
                    while (c) {
                        ev = c.arguments[0];
                        if (ev && Event == ev.constructor) {
                            break;
                        }
                        c = c.caller;
                    }
                }
                return ev;
            }
        };

## 事件节流与防抖动
> 事件节流：throttle 策略的电梯。保证如果电梯第一个人进来后，50毫秒后准时运送一次，不等待。如果没有人，则待机。

        let throttle = (fn, delay = 50) => { 
            // 节流 控制执行间隔时间 防止频繁触发 scroll resize mousemove
            let stattime = 0;
            return function (...args) {
                let curTime = new Date();
                if (curTime - stattime >= delay) {
                    fn.apply(this, args);
                    stattime = curTime;
                }
            }
        }

> 防抖动：debounce 策略的电梯。如果电梯里有人进来，等待50毫秒。如果又人进来，50毫秒等待重新计时，直到50毫秒超时，开始运送。

        let debounce = (fn, time = 50) => { // 防抖动 控制空闲时间 用户输入频繁
            let timer;
            return function (...args) {
                let that = this;
                clearTimeout(timer);
                timer = setTimeout(fn.bind(that, ...args), time);
            }
        }

## js作用域和变量声明提升
作用域（Scope）即代码执行过程中的变量、函数或者对象的可访问区域，作用域决定了变量或者其他资源的可见性；作用域其实就是所谓的执行上下文（Execution Context），每个执行上下文分为内存分配（Memory Creation Phase）与执行（Execution）这两个阶段。<br>
作用域分为全局作用域，函数作用域，块级作用域<br>
1. 在创建步骤中会进行变量对象的创建（Variable Object）、作用域链的创建以及设置当前上下文中的 this 对象。所谓的 Variable Object ，又称为 Activation Object，包含了当前执行上下文中的所有变量、函数以及具体分支中的定义

        'variableObject': {
            // contains function arguments, inner variable and function declarations
        }

2. 在 Variable Object 创建之后，解释器会继续创建作用域链（Scope Chain）；作用域链往往指向其副作用域，往往被用于解析变量。

        'scopeChain': {
            // contains its own variable object and other variable objects of the parent execution contexts
        }

3. 执行上下文则可以表述为如下抽象对象：

        executionContextObject = {
            'scopeChain': {}, // contains its own variableObject and other variableObject of the parent execution contexts
            'variableObject': {}, // contains function arguments, inner variable and function declarations
            'this': valueOfThis
        }

- 变量生命周期与提升
> 变量的生命周期包含着变量声明（Declaration Phase）、变量初始化（Initialization Phase）以及变量赋值（Assignment Phase）三个步骤；其中声明步骤会在作用域中注册变量，初始化步骤负责为变量分配内存并且创建作用域绑定，此时变量会被初始化为 undefined，最后的分配步骤则会将开发者指定的值分配给该变量。<br>
> 变量提升只对 var 命令声明的变量有效,块级作用域中使用 let 声明的变量同样会被提升，只不过不允许在实际声明语句前使用<br>
- 函数生命周期与提升
> 基础的函数提升同样会将声明提升至作用域头部，不过不同于变量提升，函数同样会将其函数体定义提升至头部；<br>
> JavaScript 中提供了两种函数的创建方式，函数声明（Function Declaration）与函数表达式（Function Expression）；函数声明即是以 function 关键字开始，跟随者函数名与函数体。而函数表达式则是先声明函数名，然后赋值匿名函数给它<br>
> 在 ES5 中，是不允许在块级作用域中创建函数的；而 ES6 中允许在块级作用域中创建函数，块级作用域中创建的函数同样会被提升至当前块级作用域头部与函数作用域头部。不同的是函数体并不会再被提升至函数作用域头部，而仅会被提升到块级作用域头部

## call和apply的区别
> 传参的不同：call的参数个数不限，apply的参数以数组的形式传入

## bind函数的实现

        Function.prototype._bind = function(context) {
            let func = this;
            let params = [].slice.call(arguments, 1);
            let Fnop = function() {};
            let fbound = function() {
                params = params.concat([].slice.call(arguments, 0));
                return func.apply(this instanceof Fnop ?this : context, params);
            }
            Fnop.prototype = this.prototype;
            fbound.prototype = new Fnop();
            return fbound;
        }

## 运算符的优先级
1. . [] ()	字段访问、数组下标、函数调用以及表达式分组
2. ++ -- - ~ ! delete new typeof void	一元运算符、返回数据类型、对象创建、未定义值
3. * / %	乘法、除法、取模
4. + - +	加法、减法、字符串连接
5. << >> >>>	移位
6. < <= > >= instanceof	小于、小于等于、大于、大于等于、instanceof
7. == != === !==	等于、不等于、严格相等、非严格相等
8. &	按位与
9. ^	按位异或
10. |	按位或
11. &&	逻辑与
12. ||	逻辑或
13. ?:	条件
14. = oP=	赋值、运算赋值
15. ,	多重求值

## ==的判断过程
1. 如果两个值类型相同，按照===比较方法进行比较
2. 如果类型不同，使用如下规则进行比较
3. 如果其中一个值是null，另一个是undefined，它们相等
4. 如果一个值是数字另一个是字符串，将字符串转换为数字进行比较
5. 如果有布尔类型，将true转换为1，false转换为0，然后用==规则继续比较
6. 如果一个值是对象，另一个是数字或字符串，将对象转换为原始值然后用==规则继续比较
7. 其他所有情况都认为不相等

## 对象到字符串的转换
1. 如果对象有toString()方法，javascript调用它。如果返回一个原始值（primitive value如：string number boolean）,将这个值转换为字符串作为结果
2. 如果对象没有toString()方法或者返回值不是原始值，javascript寻找对象的valueOf()方法，如果存在就调用它，返回结果是原始值则转为字符串作为结果
3. 否则，javascript不能从toString()或者valueOf()获得一个原始值，此时throws a TypeError

## 对象到数字的转换
1. 如果对象有valueOf()方法并且返回元素值，javascript将返回值转换为数字作为结果
2. 否则，如果对象有toString()并且返回原始值，javascript将返回结果转换为数字作为结果
3. 否则，throws a TypeError

## 哪些操作会造成内存泄漏
> 内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
 垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
> - setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
> - 闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）

## promise相关概念
> Promise是异步编程的一种解决方案，Promise是一个许诺、承诺,是对未来事情的承诺，承诺不一定能完成，但是无论是否能完成都会有一个结果。有三种状态：Pending（进行中）、Resolved（已完成，又称Fulfilled）和Rejected（已失败）。一旦状态改变就不会再变。
> - 无法取消Promise，一旦新建它就会立即执行，无法中途取消。
> - 如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
> - 当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）
- 基本Api:
    1. PromiseObj.then(resolveFn,rejectFn)
    2. Promise.all():接受一个数组作为参数，数组中存储的多个 Promise 对象实例把多个 Promise 对象实例，将所有任务的执行结果都统一的放到一个数组中，然后传给自己的 then
    3. PromiseObj.catch()
    4. PromiseObj.resolve(),PromiseObj.reject()

## set与map结构
> Set是一种叫做集合的数据结构，Map是一种叫做字典的数据结构
- 集合是由一组无序且唯一(即不能重复)的项组成的，可以想象成集合是一个既没有重复元素，也没有顺序概念的数组
- ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值
- Set 本身是一个构造函数，用来生成 Set 数据结构
    - size:返回集合所包含元素的数量
    - add(value)：向集合添加一个新的项
    - delete(value)：从集合中移除一个值
    - has(value)：如果值在集合中存在，返回true,否则false
    - clear(): 移除集合里所有的项
    - keys()：返回一个包含集合中所有键的数组
    - values()：返回一个包含集合中所有值的数组
    - entries：返回一个包含集合中所有键值对的数组(感觉没什么用就不实现了)
    - forEach()：用于对集合成员执行某种操作，没有返回值
    - union(other):并集
    - intersect (other):交集
    - difference (other):差集
- 集合是以[值，值]的形式存储元素，字典是以[键，值]的形式存储
    - set(key, val): 向字典中添加新元素
    - get(key):通过键值查找特定的数值并返回
    - has(key):如果键存在字典中返回true,否则false
    - delete(key): 通过键值从字典中移除对应的数据
    - clear():将这个字典中的所有元素删除
    - keys():将字典中包含的所有键名以数组形式返回
    - values():将字典中包含的所有数值以数组形式返回
    - forEach()：遍历字典的所有成员

## async与await的应用
- async 函数返回一个 Promise 对象，
- async 函数返回的 Promise 对象，必须等到内部所有的 await 命令的 Promise 对象执行完，才会发生状态改变，
- 正常情况下，await 命令后面跟着的是 Promise ，如果不是的话，也会被转换成一个 立即 resolve 的 Promise
- 当 async 函数中只要一个 await 出现 reject 状态，则后面的 await 都不会被执行。解决办法：可以添加 try/catch。
- 应用：解决promise的回调问题，解决多个promise等待问题
- Promise的方式

        function promiseLoops () {  
        const api = new Api()
        api.getUser()
            .then((user) => {
            return api.getFriends(user.id)
            })
            .then((returnedFriends) => {
            const getFriendsOfFriends = (friends) => {
                if (friends.length > 0) {
                let friend = friends.pop()
                return api.getFriends(friend.id)
                    .then((moreFriends) => {
                    console.log('promiseLoops', moreFriends)
                    return getFriendsOfFriends(friends)
                    })
                }
            }
            return getFriendsOfFriends(returnedFriends)
            })
        }

- async

        async function asyncAwaitLoops () {
            const api = new Api()
            const user = await api.getUser()
            const friends = await api.getFriends(user.id)

            for (let friend of friends) {
            let moreFriends = await api.getFriends(friend.id)
            console.log('asyncAwaitLoops', moreFriends)
            }
        }


## es6迭代器
- 迭代器函数名前用“*”：function *gen(){}
- 迭代器遇到yield暂时中止执行，调用迭代器next方法继续执行
- 与promise co结合使用

        let bluebird = require('bluebirld');
        let co = require('co');
        let fs = require('fs');
        let read = bluebird.promisify(fs.readFile);
        function *r(){
            let content1 = yield read('./1.txt','utf8'); //内容 ./2.txt
            let content2 = yield read(content1,'utf8');
            return content2;
        }
        let it = r();
        //可以自动迭代generator
        co(it).then(function(data){
            console.log(data);
        });
        //实现异步代码同步化