# javascript-package
整理一下js面向对象中的封装和继承。

1.封装
 　　js中封装有很多种实现方式，这里列出常用的几种。

1.1 原始模式生成对象
 　　直接将我们的成员写入对象中，用函数返回。 缺点：很难看出是一个模式出来的实例。

复制代码 代码如下:


        function Stu(name, score) {
             return {
                 name: name,
                 score: score
             }
         }
         var stu1 = Stu("张三", 80);
         var stu2 = Stu("李四", 90);
         console.log(stu1.name); // 张三



1.2 生成构造模式对象


　　js帮我们提供了一个使用构造函数生成对象的模式，¨所谓“构造函数”，其实就是一个普通函数，但是内部使用了this变量。当使用new关键字对构造函数生成实例后，this变量则会绑定在实例对象上。


复制代码 代码如下:


       function Stu(name, score) {
             this.name = name,
             this.score = score
         }
         var stu1 = new Stu("张三", 80);
         var stu2 = new Stu("李四", 90);
         console.log(stu1.name + "/" + stu2.score); // 张三  90
         console.log((stu1.constructor == Stu) + "/" + (stu2.constructor == Stu)); // true  true
         console.log((stu1 instanceof Stu) + "/" + (stu2 instanceof Stu)); // true  true


1.3 Prototype模式

　　在js中，每一个构造函数都有一个prototype属性，这个对象的所有属性和方法，都会被构造函数的实例继承。那么我们直接给prototype添加成员就相当于在C#中声明静态成员了。

复制代码 代码如下:


       function Stu(name, score) {
             this.name = name,
             this.score = score
         }
         Stu.prototype.type='学生';
         Stu.prototype.log = function (s) {
             console.log(s);
         }
         var stu1 = new Stu("张三", 80);
         var stu2 = new Stu("李四", 90);
         console.log(stu1.type + "/" + stu2.type); // 学生 学生
        stu1.log('hello');  // hello
         console.log(stu1.log == stu2.log);  // true


2.继承

2.1 构造函数绑定


　　在子函数中直接调用 call或apply方法，将父对象的构造函数绑定在子对象上。
 


复制代码 代码如下:


    function Stu(name, score) {
             Grade.apply(this, arguments);
             //Grade.call(this, arguments);
             this.name = name,
             this.score = score
         }
         function Grade() {
             this.code = "初中";
             this.ask = function () {
                 console.log("大家好");
             }
         }
         var stu1 = new Stu("张三", 80);
         var stu2 = new Stu("李四", 90);
         console.log(stu1.code); // 初中
        stu1.ask(); // 大家好



　　这里的apply做了两件事情，把第一个参数this给Grade构造函数(调用者)，然后再执行Grade里的代码。就相当于将Grade中用this定义的成员在Stu中再执行一遍。

2.2 通过prototype继承
 　

复制代码 代码如下:


     function Stu(name, score) {
             this.name = name,
             this.score = score
         }
         function Grade() {
             this.code = "初中";
         }
         Stu.prototype = new Grade();
         Stu.prototype.constructor = Stu; //防止继承链的紊乱,手动重置声明
        var stu1 = new Stu("张三", 80);
         var stu2 = new Stu("李四", 90);
         console.log(Stu.prototype.constructor); // 自己的构造函数
        console.log(stu1.code); // 初中


2.3 拷贝继承

　　把父对象的所有属性和方法，拷贝进子对象，实现继承。

复制代码 代码如下:


     function Stu(name, score) {
             this.name = name,
             this.score = score
         }
         function Grade() {}
         Grade.prototype.code = "初中";
     }
         //函数封装
        function extend(C, P) {
             var p = P.prototype;
             var c = C.prototype;
             for (var i in p) {
                 c[i] = p[i];
             }
         }
         extend(Stu, Grade);
         var stu1 = new Stu("张三", 80);
         var stu2 = new Stu("李四", 90);
         stu1.code='高中';
         console.log(stu1.code); // 高中
        console.log(stu2.code); // 初中
        console.log(Stu.prototype.constructor);
         console.log(Grade.prototype.constructor)
