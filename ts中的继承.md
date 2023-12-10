### 深度掌握TS继承(上)

#### 一、TS继承的重要丶长远意义

##### 1.练就更深厚的 JS 原型，原型链功底

TS编译后的JS中有经典的JS原型和原型链的源码实现，虽然稍显复杂，但源码并不长，这将是练就更深厚的 JS 原型，原型链功底的绝佳场景。只有深度掌握TS继承，才会拥有更深厚的 JS 原型、原型链功底，也能为阅读Vue3,React 源码或其他流行框架源码铺路，因为不管是哪种源码，JS原型链继承一定会用到！

##### 2.提升前端项目架构的根基技术

如果要你现在用开发一个工具库，组件库，你打算怎么开发 ? 可以写出n多个版本的代码，都可以实现，但版本和版本之间的价值却差别巨大，你可以用 JS 原型写出1年左右工作经验的前端水准的代码，当然，上乘之选肯定是用 TS 来开发，你也可以灵活运用TS继承，多态等多种技术写出高水准的代码。但如果你不具备后端思维能力，就算你工作了5年，你也不一定能有这样的思维，甚至随时有可能被一个拥有了后端思维的只有1到2年工作经验水准的前端工程师超越。

##### 3.晋级中丶高级前端工程师必会技能

如果你只掌握了单个类的使用，而不知道如何运用继承，那这也是技能缺失，将会限制日后技术发展的高度，限制技术视野，让前端变得过于前端化。

如果说深度掌握了 TS 继承就能突破所有的前端技术瓶颈，那显然是有些夸大其词，但要想突破前端技术瓶颈，深度掌握继承必然是其中一项技能，而且是根基技术之一，可见继承的重要性不言而喻。

比如一个简单的汽车租赁项目，让你来实现，你把前端功能实现了，展示在页面上了，但是打开你用 TS 写的 Vuex 代码，用 TS 写的 Nodejs 代码，过于前端化的思维让你编写的代码可能让人不堪入目。这里不单单是说用到封装继承，多态，解耦这些技术，更多的是你过于前端化的思维编写的项目可扩展性将非常差，可读性也差，可重用性【复用性】也低，而这些是评判一个项目是否有价值的关键因素！

#### 二、TS继承的前置知识：JS中的继承

##### 1.原型链继承

原型链继承基本思想就是Son 类【子构造函数】的原型对象属性【 Son.prototype 】指向Parent类【父构造函数】的实例对象 new  Parent( )。即 ：

```javascript
function Parent(name,age){
	this.name = name
    this.age = age
}
function Son(favor,sex){
    this.favor = favor
    this.sex = sex
}
Son.prototype = new Parent("好好的",23)
let sonObj = new Son("篮球","男")
	
```

原型链继承实现的本质是改变Son构造函数的原型对象变量的指向【 Son.prototype的指向 】，Son.prototype= new  Parent ( )。那么 Son.prototype 可以访问  Parent 对象空间的属性和方法。所以顺着 【__proto__ 】属性 ，Son类也可以访问 Parent 类 的原型对象空间中的所有属性和方法。

原型链继承查找属性和方法的完整路线描述: 子对象首先在自己的对象空间中查找要访问的属性或方法，如果找到，就输出，如果没有找到，就沿着子对象中的 __proto__属性指向的原型对象空间中去查找有没有这个属性或方法，如果找到，就输出，如果没有找到，继续沿着原型对象空间中的 __proto__ 查找上一级原型对象空间中的属性或方法，直到找到Object.prototype原型对象属性指向的原型对象空间为止，如果再找不到，就输出null。

##### 2.原型链继承实现容易被遗忘的重要一步

上面原型链继承后，打印sonObj：

![1702104977274](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1702104977274.png)

上图中可以看出，Son的实例对象中，prototype指向的原型对象空间【new Parent()】缺少 **constructor** ，我们这里还需要为Son类的原型对象prototype增加**constructor** 属性，来指向Son的构造函数对象空间：

```javascript
Son.prototype.constructor = Son
```

![1702103935929](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1702103935929.png)

##### 3.原型链继承的不足

**不能通过子类构造函数向父类构造函数传递参数：**

```javascript
function Parent(name,age){
	this.name = name
    this.age = age
}
function Son(name, age, favor,sex){
    this.favor = favor
    this.sex = sex
    this.name = name // 这里的name、age的赋值在Parent构造函数中已经存在
    this.age = age
}
Son.prototype = new Parent("father", 40)
Son.prototype.constructor = Son
let sonObj = new Son("张三", "14", "篮球","男") // 这里我们指定sonObj的姓名和年龄
console.log(sonObj)
```

![1702106033058](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1702106033058.png)

虽然通过给子类构造函数中赋值name和age可以指定Son类的实例对象的name和age，但这并不符合我们使用继承这一特性的原始思想，我们希望父类构造函数中的name和age赋值操作可以被子类构造函数共用，而不是子类构造函数中再次赋值！下面将解决这一局限性。

##### 4.借用构造函数继承（冒充对象继承）解决原型链继承的局限性

借用构造函数继承思想就是在子类【Son构造函数】的内部借助 apply ( ) 和 call ( ) 方法调用并传递参数给父类【  Parent 构造函数】，在父类构造函数中为当前的子类对象变量【Son对象变量】增加属性【name、age】

```javascript
    function Parent (name, age) {
      this.name = name
      this.age = age
    }
    Parent.prototype.friends = ["xiaozhang", "xiaoli"]
    Parent.prototype.eat = function () {
    }
    function Son (name, age, favor, sex) {
      this.favor = favor
      this.sex = sex
      Parent.call(this, name, age)// 借用Parent构造函数
    }
    let sonObj = new Son("lisi", 34, "打篮球", "男");
    console.log("sonObj:", sonObj)
    console.log("sonObj.friends:", sonObj.friends); //undefined
```

**借用构造函数继承的不足：**这里的sonObj.friends没有值，借用构造函数实现了子类构造函数向父类构造函数传递参数，但没有继承父类原型的属性和方法，无法访问父类原型上的属性和方法。

##### 5.借用构造函数+原型链组合继承

```javascript
function Parent (name, age) {
  this.name = name
  this.age = age
  console.log("this.name:", this.name)
}
Parent.prototype.friends = ["xiaozhang", "xiaoli"]
Parent.prototype.eat = function () {
  console.log(this.name + " 吃饭");
}
function Son (name, age, favor, sex) {
  this.favor = favor
  this.sex = sex
  Parent.call(this, name, age) // TS继承中使用super
}
Son.prototype = new Parent("temp", 3);
Son.prototype.constructor = Son

let sonObj = new Son("lisi", 34, "打篮球", "男");

```

![1702111661862](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1702111661862.png)

**借用构造函数 + 原型链组合继承解决了二者各自的缺点**：

子类【Son 构造函数】的内部可以向父类【 Parent 构造函数】 传递参数；Son .prototype 和 new Son ( ) 出来的实例对象变量和实例都可以访问父类【 Parent  构造函数】 原型对象上的属性和方法。

**依然存在不足：调用了两次父类构造函数 【 Parent 构造函数】 new Parent()调用构造函数带来问题： **

1. 进入 Parent 构造函数为属性赋值，分配内存空间，浪费内存；

2. 赋值导致效率下降一些，关键是new Parent ()赋的值无意义，出现代码冗余，new Son出来的对象和这些值毫不相干，是通过子类 Son构造函数中的 apply 来向父类Parent 构造函数赋值。

##### 6.寄生组合继承模式

寄生组合继承模式 = 借用构造函数继承 + 寄生继承。

寄生组合继承既沿袭了借用构造函数+原型链继承两个优势，而且解决了借用构造函数+原型链继承调用了两次父类构造函数为属性赋值的不足。寄生组合继承模式保留了借用构造函数继承，寄生组合继承模式使用寄生继承代替了原型链继承。

什么是寄生继承呢？就是 Son.prototype 不再指向 new  Parent( ) 出来的对象空间，而用 Parent类 【父构造函数】的原型对象属性“克隆”了一个对象。再让Son.prototype指向这个新对象，很好的避免了借用构造函数 + 原型链继承调用了两次父类构造函数为属性赋值的不足。

```javascript
function Parent (name, age) {
  this.name = name;
  this.age = age;
}

Parent.prototype.doEat = function () {
  console.log(this.name + "吃饭...")
}

function Son (name, age, favor, sex) {

  Parent.call(this, name, age)
  this.favor = favor;
  this.sex = sex;
}
//寄生组合继承实现步骤
// 第一步: 创建一个寄生构造函数
function Middle () {
  this.sign = 1 // 这里的sign跟父类、子类无关，单纯标记，便于调试和理解
}

Middle.prototype = Parent.prototype // 注意这里的改变原型对象空间和下一步的实例化顺序不能颠倒
// 第二步:创建一个寄生新创建的构造函数的对象
let middle = new Middle();
// 第三步:Son子类的原型对象属性指向第二步的新创建的构造函数的对象
Son.prototype = middle
Son.prototype.constructor=Son

let son1 = new Son("王海", 18, "王者荣耀", "男");
let son2 = new Son("王红", 20, "LOL", "女");

console.log("Son1:", son1);
console.log("Son2:", son2);

```

为了让寄生组合继承模式更加通用，这里将其封装，便于后续用于更多使用继承的场景：

```javascript
function _extends (parent_, son_) {//继承
  // 第一步: 创建一个寄生构造函数
  function Middle () {
    this.sign = 1
    this.constructor = son_
  }
  Middle.prototype = parent_.prototype
  // 第二步:创建一个寄生新创建的构造函数的对象
  let middle = new Middle();//middle.__proto__=parent_.prototype
  return middle
}
function Parent (name, age) {
  this.name = name;
  this.age = age;
}

Parent.prototype.doEat = function () {
  console.log(this.name + "吃饭...")
}

function Son (name, age, favor, sex) {

  Parent.call(this, name, age)
  this.favor = favor;
  this.sex = sex;
}

let middle = _extends(Parent, Son);

Son.prototype = middle;

let son1 = new Son("王海", 18, "王者荣耀", "男");
let son2 = new Son("王红", 20, "LOL", "女");
console.log("Son1:", son1);
console.log("Son2:", son2);

```

**拓展：使用Object.create和Object.setPrototypeOf重写上面的_extentds函数**

```javascript
function _extends (parent_) {//继承
  // Object.create
  let middle = Object.create(parent_.prototype, {
    sign: {
      writable: true,
      value: 1
    }
  })
  return middle;
  // Object.setPrototypeOf
  let middle = { sign: 1 }
  return Object.setPrototypeOf(middle, parent_.prototype)
}
function Parent (name, age) {
  this.name = name;
  this.age = age;
}

Parent.prototype.doEat = function () {
  console.log(this.name + "吃饭...")
}

function Son (name, age, favor, sex) {

  Parent.call(this, name, age)
  this.favor = favor;
  this.sex = sex;
}

let middle = _extends(Parent);

Son.prototype = middle;

Son.prototype.constructor = Son;//需要额外增加子构造函数指向的原型对象空间中的constructor属性

let son1 = new Son("王海", 18, "王者荣耀", "男");
let son2 = new Son("王红", 20, "LOL", "女");
console.log("Son1:", son1);
console.log("Son2:", son2);

```

#### 三、TS中的继承

##### 1. 理解子类

**（1）什么是子类？**   

 TypeScript 支持继承类，即我们可以在创建类的时候继承一个已存在的类，这个已存在的类称为父类，继承它的类称为子类。 

有两个类，比如 A 类和 B 类，如果满足 A 类  is a kind of  B类，那么 A 类就是 B 类的子类
比如：A 类是顾客类，B 类是人类，因为顾客类 a kind of 人类成立【顾客类是人类的一种】，所以顾客类是人类的子类。

**（2） 子类如何继承父类的属性和方法？**

以顾客类为例子：顾客类继承了父类【人类】的非私有的属性和方法，也具备子类独有的属性和方法 。

顾客类继承父类【人类】的全部非私有的属性和方法外，还有哪些独有的属性和方法呢？
顾客类独有属性：顾客等级，顾客编号
顾客类独有方法：购买

  **(3)  初步理解为什么要用继承？**

 举例：宠物管理项目中的狗狗类，兔子类，小猫类都是宠物，尽管每个宠物都有独有属性和方法，比如狗狗类的品种，看家方法；兔子类的肤色属性等。但这些类都包含了 name, buymoney【购买价格】，healthstatus【健康状况】，friendshipstar【和主人的友谊星级数】这些属性，如果每一个类都写这些属性，那么就非常臃肿，可以把这些属性提取出来放到一个宠物类中，其他类都继承这个宠物类。当然继承还有更多好处，下面借助汽车租赁功能的实现来更深度的掌握继承。

 **(4)  示例：汽车租赁管理功能**

**需求1:**汽车租赁功能实现: 有小轿车,大巴,卡车三种类型的车,顾客可以租任意一种或多种不同类型的车,按照租用的天计算租金， 同时为了响应国家对各类车安全的管理, 对在租赁期内有过各种超载，超乘客数，酒后驾车等违规的车需额外支付一定的费用。

**需求2:**计算退回费用：最终退回顾客的费用为押金扣除使用天数，如押金不足需额外支付不足部分。

小轿车、大巴、卡车共同属性:  品牌 ( brand )  VechileNo ( 车牌号 )，days ( 租赁天数 )， total ( 支付的租赁总费用 )  deposit ( 押金 )。

小轿车、大巴、卡车共同方法: 计算租赁车的价格 ( calculateRent) ，支付押金的方法( payDesposit)，安全规则方法（safeShow)

父类：Vechile   交通工具。

子类： 小轿车型号type属性。

代码实现：

```typescript
// 父类：Vechile   交通工具。
class Vechile {
  static count: number = 3;
  public brand: string;// 品牌
  public vechileNo: string;// 车牌号
  public days: number;// 租赁天数
  public total: number = 0;// 支付的租赁总费用
  public deposit: number;// 押金
  constructor(brand_: string, vechileNo_: string, days_: number, deposit_: number) {
    this.brand = brand_;
    this.vechileNo = vechileNo_;
    this.days = days_;
    this.deposit = deposit_;
    console.log("constructor Vechile=>this.brand:", this.brand)
  }
  // 计算租赁车的价格 ( calculateRent)
  public calculateRent() {
    console.log("calculateRent来自Vechile=>this.brand:", this.brand)

    console.log(this.brand + " 车牌号为:" + this.vechileNo + "开始被租");
    return 0;
  }
  //支付押金的方法( payDesposit)
  payDesposit() {
    console.log(this.brand + " 车牌号为:" + this.vechileNo + " 支付了:" + this.deposit);
  }

  //  安全检测方法（safeShow)
  public safeShow() {
    console.log("车规则....");
    console.log(this.brand + " 车牌号为:" + this.vechileNo + " 违规了:");
  }
}

// 子类Car类 独有属性为type_ 
class Car extends Vechile {
  // public brand: string = "nobrand"
  public type: string;//车的型号
  constructor(brand_: string, vechileNo_: string, days_: number,
    deposit_: number, type_: string) {
    //  Vechile.call(this,brand_, vechileNo_, days_, total_, deposit_)
    super(brand_, vechileNo_, days_, deposit_);
    this.type = type_;
  }
  // 根据车的型号来获取租用一天该型号车的租金
  public getPriceByType() {
    let rentMoneyByDay: number = 0;//每天的租金
    if (this.type === "普拉多巡洋舰") {
      rentMoneyByDay = 800;
    } else if (this.type === "凯美瑞旗舰版") {
      rentMoneyByDay = 400;
    } else if (this.type === "威驰智行版") {
      rentMoneyByDay = 200;
    }
    return rentMoneyByDay;
  }
  // private 是私有的访问修饰符 只允许在本类中方法
  // protected 是受保护的访问修饰符【修饰符是用来控制方法或属性访问的范围】,可以被本类和子类中使用，不能在类的外部使用
  // public // 可以被本类和子类中使用，也可以在类的外部使用 默认是public
  public calculateRent() {//方法重写 [override]
    // this.safeShow();// 寄生组合继承模式 middle()
    super.calculateRent();  //=Vechile.prototype.calculateRent.call(this)
    console.log("Car:", Car.count)
    console.log("this.brand:", this.brand)
    return this.days * this.getPriceByType();
  }
  checkIsWeigui(isOverWeight: boolean) {
    if (isOverWeight) {
      this.total = this.total + 500;
    }
  }
}
let car = new Car("普拉多", "京3A556", 3, 100000, "凯美瑞旗舰版");
console.log(car.calculateRent());

class Bus extends Vechile {
  public seatNum: number // 座位数
  constructor(brand_: string, vechileNo_: string, days_: number,
    deposit_: number, seatNum_: number) {
    //  Vechile.call(this,brand_, vechileNo_, days_, total_, deposit_)
    super(brand_, vechileNo_, days_, deposit_);//使用父类的构造函数的好处
    this.seatNum = seatNum_;
    if (this.seatNum > 200) {
      throw new Error("座位数不能超过200");
    }
  }
  public getPriceBySeatNum() { //计算租赁价格
    let rentMoneyByDay: number = 0;//每天的租金
    if (this.seatNum <= 16) {
      rentMoneyByDay = 800;
    } else if (this.seatNum > 16) {
      rentMoneyByDay = 1600;
    }
    return rentMoneyByDay;
  }
  public calculateRent() {
    super.calculateRent();
    return this.days * this.getPriceBySeatNum();
  }
  checkIsOverNum(isOverWeight: boolean) {
    if (isOverWeight) {
      this.total = this.total + 2000;
    }
  }
}

class Truck extends Vechile {
  ton!: number // 座位数
  constructor(brand_: string, type_: string,
    days_: number, deposit_: number, ton_: number) {
    super(brand_, type_, days_, deposit_);
    this.ton = ton_;
    if (this.ton < 300 || this.ton > 2000) {
      throw new Error("吨数在300-2000吨之间");
    }
  }

  checkIsOverWeight(isOverWeight: boolean) {
    if (isOverWeight) {
      console.log("超载了");
      this.total = this.total + 2000;
    }
  }

  CalRentPrice() {//计算租赁价格
    let rentMoneyByDay: number = 0;//每天的租金
    if (this.ton <= 500) {//500吨
      rentMoneyByDay = 750;
    } else if (this.ton > 500) {
      rentMoneyByDay = 1350;
    }
    return rentMoneyByDay;
  }
  public calRent() {
    return this.CalRentPrice() * this.days;
  }
  public calDesposit() {
    return this.deposit;
  }
}

class Customer {
  public rentVechile() {

  }
}

/**
输出：
constructor Vechile=>this.brand: 普拉多
calculateRent来自Vechile=>this.brand: 普拉多
普拉多 车牌号为:京3A556开始被租
Car: 3
this.brand: 普拉多
1200
*/
```

##### 2.TS继承中的super和方法重写【override】

**（1）super： super 关键字是对父类的直接引用，该关键字可以引用父类的属性和方法。** 

super只能出现在子类【派生类】中，使用场景：

1.在子类的构造函数中使用 super (子类传递给父类构造函数的参数) 就表示用来调用父类构造函数  (传递给父类构造函数的参数)

2.在子类重写的方法中调用父类同名方法，super.重写的方法

注意：当子类和父类拥有同名属性时，super获取不到父类中的同名属性，一般要避免在子类中定义与父类的同名方法。

**（2）方法重写（override）：**

**条件**：一定发生在继承的子类中

**位置**： 子类中重写父类的方法

**应用场景**：当父类中方法的实现不能满足子类功能需要或不能完全满足子类功能需要时，就需要在子类中进行重写

**方法重写给继承带来的好处**: 让所有的子类共用父类中方法已经实现了一部分功能的代码【父类方法代码在各个子类中得到了复用】 

**定义规则**：

1.  和父类方法同名 
2.  参数和父类相同，如果是引用类型的参数，需要依据具体类型来定义。

   3.  父类方法的访问范围【访问修饰符】必须小于子类中方法重写的访问范围【访问修饰符】

      同时父类方法不能是private 

##### 3.TS继承底层JS源码

