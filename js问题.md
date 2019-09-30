## js常用方法

### 1.undefined和null区别

Undefined类型只有一个值，即undefined。当声明的变量还未被初始化时，变量的默认值为undefined。

```
var value;

console.log("value:"+value);

输出结果：value:undefined

```
查找不存在的元素

```
var  id;

id=document.getElementById('notExistElement');
console.log("id:" + id);

输出结果：id:null
```

对比对象的类型
```
 var  type1=typeof undefined;
 var  type2=typeof null;
 console.log(type1);
 console.log(type2);
 console.log(undefined == null);
 console.log(undefined === null);

输出结果：undefined
         object
         true
         fasle

```



结论

- 一个对象中没有指定的变量，而要使用，会出现 undefined
- Html中没有的元素，通过document.getElementById("")查找的结果为： null
- Html中有的元素，但是没有任何值，通过document.getElementById("")查找的结果为：” ”; 不是null

### 2. js中两个等号和三个等号区别

简单来说： = = 代表相同， ===代表严格相同, 为啥这么说呢。

这么理解： 当进行双等号比较时候： 先检查两个操作数数据类型，如果相同， 则进行= = =代比较， 如果不同， 则愿意为你进行一次类型转换， 转换成相同类型后再进行比较，而= = =比较时， 如果类型不同，直接就是false.

```
var a= 1;

var b='1';

console.log(a==b);

console.log(a===b);

输出结果： true
           false

```


### 3.JS中函数参数值传递和引用传递


```
$(function () {
  var  num=0;
  var result=add(num);
  console.log(num);
  console.log(result);
}

// 定义方法：
function add(num) {
return num+10;
}

输出结果： 0
输出结果： 10
```
```
$(function () {
  var  person={"age":10};
  var result=addAge(person);
  console.log("person:"+person);
  console.log("result:"+result);
}

// 定义方法：
function addAge(person) {
    person.age=person.age+10;
    return person;
}

输出结果： "person:"{age:20}
           "result:"{age:20}
```

```
var person=new Object();
var obj = person;  // 赋值
obj.name="ABC";
obj=new Object();
obj.name="BCD";
console.log(person.name);
输出结果：  ABC
```

结论：
- 基本类型,参数传递是自身的值传递的，不会影响原变量
- 对象类型,参数传递是按引用的值传递的，会影响原变量的属性


### 4.Js 数据类型

字符串、数字、布尔、数组、对象、Null、Undefined

当您声明新变量时，可以使用关键词 "new" 来声明其类型,当您声明一个变量时，就创建了一个新的对象。
```
var carname=new String;
var x=      new Number;
var y=      new Boolean;
var cars=   new Array;
var person= new Object;
```

声明变量类型
```
var carname='haha';
var x= 1;
var y=true;
var cars=[]
var person={};
```

### 5.JS 原型与原型链

```

js里的__proto__和prototype到底有什么区别？

//首先声明一个对象和一个函数，console.log一下对象和函数的proto
var A = function () {};
var B ={};

console.log(A.__proto__)
console.log(B.__proto__)


console.log(A.prototype)
console.log(B.prototype)

```
![](https://i.imgur.com/OADjySy.png)

- js里所有的对象和函数都有_ _proto_ _ 属性，指向构造该对象的构造函数的原型。

- 只有函数function才具有prototype属性。这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做constructor，这个属性包含了一个指针，指回原构造函数。

第一种定义对象方式

```
   var animal = {
        name: "animal",
        eat: function () {
            console.log(this.name + "is eating");
        }
    };
    animal.color = "red";
    console.log(animal.color);


    var dog = {
        name: "dog",
        __proto__: animal
    };

    var cat = {
        name: "cat",
        __proto__: animal
    };

    dog.eat();
    cat.eat();


  js中没有类的概念，对象的__proto__ 默认指向对象构造方法的原型,对象的继承通过__proto__ 指定原型继承,形成原型链，最终指向Object。通过原型链查找，这样子类对象可以使用父类对象的方法。

```
![](http://mmbiz.qpic.cn/mmbiz_png/KyXfCrME6ULNsLZbiaoLXPeaB05FyJPYnGZBptoCLZCzowGyVV1bLcrpXxf9r0K58icd2YhmUP4AW50hCwWqcXibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
第二种利用构造方法定义对象
```
     function Animal(name) {
        this.name=name;
        this.eat=function () {
            console.log(this.name + " is eating");
        }
    }

    function Dog(name) {
        Animal.call(this,name);
    }

    function Cat(name) {
        Animal.call(this,name);
    }

    var dog  =new Dog("dog");
    var cat =new Cat("cat");

    dog.eat(); //dog is eating
    cat.eat(); //cat is eating
```

定义构造方法，定义其原型对象
```
    function Animal(name) {
        this.name=name;
    }

    Animal.prototype.eat=function () {
        console.log(this.name + " is eating");
    };

    //原型对象的构造方法属性指针指向对象的构造方法
   console.log(typeof  Animal.prototype.constructor); // function
   console.log(Animal.prototype.constructor===Animal);//true


    function Dog(name) {
        Animal.call(this,name);
    }

    function Cat(name) {
        Animal.call(this,name);
    }

    var dog  =new Dog("dog");
    var cat =new Cat("cat");


    console.log(typeof  Dog.prototype); // object
    console.log(typeof  Dog.__proto__); // function
    console.log( Dog.__proto__===Cat.__proto__); // true

    console.log(typeof  dog.prototype); // undefined
    console.log(typeof  dog.__proto__); // object
    console.log(dog.__proto__ === Dog.prototype); //true

    console.log(typeof dog.__proto__.constructor); // function
    console.log(dog.__proto__.constructor === cat.__proto__.constructor);// false
    console.log(dog.__proto__.constructor === Animal);// false
  js中没有类的概念，定义构造方法，相当于定义一个模板，可以利用这个构造实例化多个对象，不用重复定义属性;同时如果有公共方法，可以使用prototype定义一个原型对象,里面可以定义方法和属性,同一类构造方法实例化的对象都可以直接使用。可以看到prototype将构造方法和原型对象联系起来。这样，对象的属性和方法都有模板,代码复用性提高。

```


定义构造方法，修改其prototype指向，实现继承

```
    function Animal(name) {
        this.name=name;
    }

    Animal.prototype.eat=function () {
        console.log(this.name + " is eating");
    };

    //原型对象的构造方法属性指针指向对象的构造方法
   console.log(typeof  Animal.prototype.constructor); // function
   console.log(Animal.prototype.constructor===Animal);//true

    function Dog(name) {
        Animal.call(this,name);
    }

    function Cat(name) {
        Animal.call(this,name);
    }

    //改变Dog的prototype指针，指向一个 Animal 实例
    Dog.prototype=new Animal("animal");
    Cat.prototype=new Animal("animal");


    var dog  =new Dog("dog");
    var cat =new Cat("cat");

    dog.eat(); //dog is eating
    cat.eat(); //cat is eating



    console.log(typeof  Dog.prototype); // object  原型对象
    console.log(typeof  Dog.__proto__); // function
    console.log( Dog.__proto__===Cat.__proto__); // true

    //实例对象的内部__proto__ 属性指针指向了原型对象
    console.log(typeof  dog.prototype); // undefined
    console.log(typeof  dog.__proto__); // object
    console.log(dog.__proto__ === Dog.prototype); //true

    console.log(typeof dog.__proto__.constructor); // function
    console.log(dog.__proto__.constructor === cat.__proto__.constructor);// true
    console.log(dog.__proto__.constructor === Animal);// true
```

### 6.JS  prototype 继承

所有的 JavaScript 对象都会从一个 prototype（原型对象）中继承属性和方法：

    Date 对象从 Date.prototype 继承。
    Array 对象从 Array.prototype 继承。
    Person 对象从 Person.prototype 继承。

所有 JavaScript 中的对象都是位于原型链顶端的 Object 的实例。

JavaScript 对象有一个指向一个原型对象的链。当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。

Date 对象, Array 对象, 以及 Person 对象从 Object.prototype 继承。


### 7.JS函数闭包

闭包是一种保护私有变量的机制，在函数执行时形成私有的作用域，保护里面的私有变量不受外界干扰。

直观的说就是形成一个不销毁的栈环境。

```
 function add1() {
        var counter = 0;

        function plus() {
            counter += 1;
        }

        plus();
        return counter;
    }

    console.log(add1());
    console.log(add1());
    console.log(add1());


    var  sum1=function (a,b) {
        return a+b;
    };
    console.log(typeof  sum1);
    console.log(typeof  sum1());
    console.log(sum1(1,2));

    sum1 = function (a,b,c) {
        return a+b+c;
    };

    console.log(sum1(1,2,3));


    function sum2(a,b) {
        return a+b;
    }
    console.log(sum2(1,2));

    console.log('----------');

    var  addTemp=function () {
        //定义方法的私有变量
        var counter = 0;
        return function () {
            //内部函数执行逻辑
            return counter += 1;
        }
    };

    var add2 = addTemp();

    console.log(add2());
    console.log(add2());
    console.log(add2());


    console.log('----------');

    //获得的是内部方法的指针
    var  outerFunction=function () {
        //定义外部方法的私有变量
        var counter = 0;
        var  innerFunction=function () {

            console.log('counter：'+counter);

            //内部函数执行逻辑
            return counter += 1;
        };
        //返回是内部函数的指针
        return innerFunction;
    };
    //执行外部方法,返回内部方法的引用
    var resultFunction = outerFunction();
    //执行内部方法
    console.log(resultFunction());
    //执行内部方法
    console.log(resultFunction());
    //执行内部方法
    console.log(resultFunction());
    //执行之完前，外部方法的变量不会销毁
```

### 8.JS初始化
页面
```
<!DOCTYPE html>
<html lang="en">

<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试js方法</title>

    <script type="text/javascript" src="../js/jquery-1.8.3.min.js"></script>
    <script type="text/javascript" src="../js/test6.js"></script>

</head>

<body>

<div class="container" style="margin-left: 15px;margin-right: 15px">

  <h1 id="test01">测试文字1</h1>

  <h1 id="test02">测试文字2</h1>

  <h1 id="test03">测试文字3</h1>
</div >



</body>
</html>
```

js初始化方法
```
//原生js

function myFunction1() {
    var obj = document.getElementById("test01");
    obj.innerHTML = "Hello jQuery";
    console.log(obj)
}

onload = myFunction1;


//jquery
function myFunction2()
{
    $("#test02").html("Hello jQuery");
}
$(document).ready(myFunction2);


//jquery
$(function () {

    var obj = $("#test03");
    console.log(obj);
    obj.html("Hello jQuery")
});


```
