### TypeScript 类

#### 1.TypeScript中类的意义

​		相对以前 JavaScript 不得不用 构造函数来充当”类“，TypeScript 类的出现可以说是一次技术革命。让开发出来的项目尤其是大中项目的可读性好，可扩展性好了不是一点半点。

​		TypeScrip 类的出现完全改变了前端领域项目代码编写模式，配合 TypeScript 静态语言，编译期间就能检查语法错误的优势【项目上线后隐藏语法错误的风险几乎为零，相比不用 TypeScript 开发项目，使用 TypeScript 后对前端项目尤其是大中项目的开发 或底层第三方插件，组件库的开发带来的优势已经超乎了想象】。

​		TypeScript 类让前端开发人员开发和组织项目或阅读各大前端框架源码的思维问题的方式变得更先进，更全面了许多。因为类是 OOP【面型对象编程】的技术基石，OOP 思想来自于生活，更利于开发人员思考技术问题。TypeScript 类已经成了每次前端面试的高频面试考点。

​		在前端各大流行框架开发的项目中，比如 Vue3 项目，Angular项目， 基于 Antd UI 库的项目 还是后端 Nodejs 框架，比如：Nestjs，亦或是 Vue3 底层源码，都可以频频见到类的身影。

​		尽管 TypeScript 类照搬了 Java 后端语言的思想，但 TypeScript 类的底层依然是基于 JavaScript 的，这一点对于前端工程师更深入理解 TypeScript 打开了一条理解之道，提升他们更深厚的 JavaScript 功底从而为面试加分和项目的运用都提供了间接的帮助。

#### 2.TypeScript中类的使用场景

​		TypeScript 类是 OOP（ 面向对象 ） 的技术基石，包括类、属性封装丶继承、多态、抽象丶泛型。紧密关联的技术包括方法重写，方法重载，构造器，构造器重载，类型守卫，自定义守卫，静态方法、属性，关联引用属性，多种设计模式等。

#### 3.什么是类

​		定义：类就是拥有相同属性和方法的一系列对象的集合，类是一个模具，是从这该类包含的所有具体对象中抽象出来的一个概念，类定义了它所包含的全体对象的静态特征和动态特征。

​		类有静态特征和动态特征【以大家最熟悉的人类为例】
静态特征【软件界叫属性】姓名，年龄,地址,身份证号码,联系方式,家庭地址,微信号
动态特征【软件界叫方法】吃饭，走路

【再看桌子类】

​		静态特征【属性】高度，宽度，颜色，价格，品牌，材质

​		动态特征【方法】承载

【来看订单类】 

​		静态特征 【属性】 订单号【订单id】，下单时间，下单顾客，订单详情，顾客微信，收件地址，负责客服

​		动态特征  【方法】 下单，修改订单，增加订单，删除订单，查询订单，退单 【这一些方法真正开发会归为OrderService 类】 但从广义来说都同属于订单系列类的方法。

#### 4.子类

（1）什么是子类？   

​		有两个类，比如 A 类和 B 类，如果满足 A 类  is a kind of  B类，那么 A 类就是 B 类的子类
比如：A 类是顾客类，B 类是人类，因为顾客类 a kind of 人类成立【顾客类是人类的一种】，所以顾客类是人类的子类。

（2）子类如何继承父类的属性和方法？

​		以顾客类为例子：顾客类继承了父类【人类】的非私有的属性和方法，也具备子类独有的属性和方法 。

​		顾客类继承父类【人类】的全部非私有的属性和方法外，还有哪些独有的属性和方法呢？
顾客类独有属性：顾客等级，顾客编号

顾客类独有方法：购买

#### 5.对象【对象也叫实例(instance)，对象变量也叫实例变量】

(1)  什么是对象【实例】？

​		就是一个拥有具体属性值和方法的实体，是类的一个具体表现，一个类可以创建1个或者多个对象

 (2)  如何通过类来创建对象 【实例】？

```javascript
let 对象变量名= new 类名()
const 对象变量名= new 类名()
```

 (3)  如何根据People类来创D建叫张三对象【实例】的人？【举例】

​		let  kateCust=new Customer()   kateCust 是对象变量名 ，new Customer()  表示 new 出来的是一个Customer对象，而且是运行期间才在堆中分配 Customer 对象的内存空间 【 new  就是分配内存空间的意思】

(4) 类的对象变量丶对象内存图展示

(5) 类的对象变量，对象的关系

​	类的对象变量存在栈中，对象变量存储着对象的首地址，对象变量通过这个地址找到它的对象

#### 6.示例

```typescript
class Person {
  //public  name:string |undefined//typescript4.0之前属性如果没有赋值的解决方法 增加一个undefined数据类型
  // 类上定义的属性一定是描绘这个类本身特征的变量，不能把一些无关的变量定义成类属性
  public name: string = "noname"//赋初值为noname
  public age: number = 0
  public phone: string = "11111"
  // 对象的变量 = 实例的变量 = 类的【非静态的】属性 = 简称属性、实例属性或者对象属性

  constructor(name_: string, age_: number, phone_: string) {
    this.name = name_;
    this.age = age_;
    this.phone = phone_;
  }

  //function  错误,类中定义方法不能用function
  // public play(): number {
  //   //return "df"//不能将类型“string”分配给类型“number”
  //  // return 3

  // }

  public doEat(who: string, address: string): void {//方法默认的返回值为void
    console.log(`${this.name}和${who}吃饭,在${address}吃饭`);
  }

  public play() {

  }
}
//let zhangSanPerson = new Person();
//给对象赋值的两种方式
// 方法1：通过类中属性或者方法来赋值 get/set选择器
// zhangSanPerson.name = "zhangSan"
// zhangSanPerson.age = 23
// zhangSanPerson.phone = "134123123"

// zhangSanPerson.doEat("李四", "王府井")

// 方法2： 通过构造函数 【构造器】来赋值
// 创建对象一共做了三件事
// 第一件事: 在堆中为类的某个对象【实例】分配一个空间
// 第二件事：调用对应的构造函数【构造器】并且把构造器中的各个参数值赋值给对象属性
//   new Person()自动匹配无参数的构造器
// 第三件事：把对象赋值给对象变量 【把实例赋值给实例变量】
let zhangSanPerson = new Person("zhangSan", 23, "134123123");
zhangSanPerson.doEat("李四", "王府井")

console.log(zhangSanPerson)

//let obj={username:"wangwu",playgame(){}}
```

内存图：

![内存图](C:\Users\Administrator\Desktop\ts学习\tssenior\source\2chapter\本章内存图\内存图.png)

以上Person类编译后的JS原生ES5语法代码：

```javascript
var Person = /** @class */ (function () {
    function Person(name_, age_, phone_) {
        this.name = "noname"; //赋初值为noname
        this.age = 0;
        this.phone = "11111";
        this.name = name_;
        this.age = age_;
        this.phone = phone_;
    }
    Person.prototype.doEat = function (who, address) {
        console.log(this.name + "\u548C" + who + "\u5403\u996D,\u5728" + address + "\u5403\u996D");
    };
    Person.prototype.play = function () {
    };
    return Person;
  }());
  
    var zhangSanPerson = new Person("zhangSan", 23, "134123123");
    zhangSanPerson.doEat("李四", "王府井");
    console.log(zhangSanPerson);
```

