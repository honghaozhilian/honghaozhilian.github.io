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
- [事件的捕获冒泡的应用场景](#事件的捕获冒泡的应用场景)
- [编写一个EventUtil工具类实现事件管理兼容](#编写一个EventUtil工具类实现事件管理兼容)
- [事件节流与防抖动](#事件节流)
- [js作用域和变量声明提升](#js作用域和变量声明提升)
- [es6的块式作用域和let](#es6的块式作用域和let)
- [call和apply的区别](#call和apply的区别)
- [bind函数的实现](#bind函数的实现)
- [运算符的优先级](#运算符的优先级)
- [==的判断过程](#==的判断过程)
- [对象到字符串的转换](#对象到字符串的转换)
- [对象到数字的转换](#对象到数字的转换)

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

## 事件的捕获冒泡的应用场景
- addEventlistener的第三个参数决定事件触发应该在捕获阶段还是冒泡阶段（默认是冒泡阶段）
- 事件冒泡可用于事件委托，将事件绑定在父元素下，点击目标元素通过冒泡到父元素，判断目标元素触发相关事件，好处：提升性能（不需要绑定过多的事件），对于动态添加的元素事件绑定同样有效
- 事件捕获可用于父元素触发事件，而子元素不触发事件的时候


## 编写一个EventUtil工具类实现事件管理兼容

## 事件节流与防抖动

## js作用域和变量声明提升

## es6的块式作用域和let

## call和apply的区别

## bind函数的实现

## 运算符的优先级

## ==的判断过程

## 对象到字符串的转换

## 对象到数字的转换