####  1.TS 类型断言定义

把两种能有重叠关系的数据类型进行相互转换的一种 TS 语法，把其中的一种数据类型转换成另外一种数据类型。类型断言和类型转换产生的效果一样，但语法格式不同。

#### 2.TS 类型断言语法格式

A 数据类型的变量  as  B 数据类型 。

A 数据类型和 B 数据类型必须具有重叠关系。

#### 3.TS 类型断言使用规则和场景

##### （1）如果  A，B 如果是类并且有继承关系

【 extends 关系】无论 A，B 谁是父类或子类， A 的对象变量可以断言成 B 类型，B 的对象变量可以断言成A类型 。但注意一般在绝大多数场景下都是把父类的对象变量断言成子类。

```javascript
class People {
  public myusername!: string;
  public myage!: number;
  public address!: string
  public phone!: string
  constructor() {
  }
  eat() {

  }
  step() {
    console.log("People=>step");
  }
}

class Stu extends People {
  public username!: string
  public age!: number;
  public address!: string
  constructor(username: string, age: number, address: string, public phone: string) {
    super();
    this.address = address;
  }
  study() {
  }
}

let people = new People()

//let result = people as Stu;// 类型断言 正确
let result = <Stu>people;// 类型转换 正确
result.study();

let stu = new Stu("wangwu", 23, "北京", "123")
let result2 = stu as People;// 正确

```

##### （2）如果   A，B 如果是类，但没有继承关系

两个类中的任意一个类的所有的 public 实例属性【不包括静态属性】加上所有的 public 实例方法和另一个类的所有 public 实例属性加上所有的 public 实例方法完全相同或是另外一个类的子集，则这两个类可以相互断言，否则这两个类就不能相互断言。

```javascript
// 类型断言中的不能相互重叠问题:
//   两个类之间断言的规则:
//   两个类中任意一个的属性和方法是另一个类的属性和方法完全相同或子集，则这两个类可以相互断言
//   否则这两个类就不能相互断言

class People {
  constructor(public username: string, public age: number,
    public address: string) {

  }
}

class Stu {
  public username!: string
  public age!: number;
  public address!: string
  public phone!:string;
  constructor(username: string, age: number, address: string) {
    this.address = address;
  }
}

let people = new People("wangwu", 23, "beijing")
let stuedConvert = people as Stu;
let stu = new Stu("wangwu", 23, "北京")
let peopledConvert = stu as People;
```

##### （3）如果 A 是类，B 是接口，并且 A 类实现了 B 接口【implements】，则A 的对象变量可以断言成 B 接口类型，同样 B 接口类型的对象变量也可以断言成A类型 。

```javascript
interface People {
  username: string, age: number, address: string, phone: string
}

class Stu implements People {
  public username!: string
  public age!: number;
  public address!: string
  public phone!: string
  public kk() {

  }
  constructor(username: string, age: number, address: string) {
    this.address = address;
  }
}

let people: People = { username: "wangwu", age: 23, address: "11", phone: "111" }
let result = people as Stu;//正确
let result2 = <Stu>people;// 类型转换 正确

let stu = new Stu("wangwu", 23, "北京")
stu as People;// 正确

```

##### （4）如果 A 是类，B 是接口，并且 A 类没有实现了 B 接口，则断言关系和第2项的规则完全相同。

```javascript
interface People {
  username: string, age: number, address: string, phone: string
}

class Stu {
  public username!: string
  public age!: number;
  public address!: string
  public phone!: string
  public kk() {

  }
  constructor(username: string, age: number, address: string) {
    this.address = address;
  }
}

let people: People = { username: "wangwu", age: 23, address: "11", phone: "111" }
let result = people as Stu;//正确

let stu = new Stu("wangwu", 23, "北京")
stu as People;// 正确

```

##### （5）如果 A 是类，B 是 type 定义的数据类型

type 定义的数据类型是引用数据类型，例如 Array, 对象，不能是基本数据类型，例如 string，number,boolean，并且有 A 类实现了 B type 定义的数据类型【 implements】，则 A 的对象变量可以断言成 B type 定义的对象数据类型，同样 B type 定义的对象数据类型的对象变量也可以断言成 A 类型 。

```javascript
type People = {
  username: string, age: number, address: string, phone: string
}

class Stu implements People {
  public username!: string
  public age!: number;
  public address!: string
  public phone!: string
  constructor(username: string, age: number, address: string) {
    this.address = address;
  }
}


let people: People = { username: "wangwu", age: 23, address: "11", phone: "111" }
let result = people as Stu;//正确

let stu = new Stu("wangwu", 23, "北京")
stu as People;// 正确

```

##### （6）如果 A 是类，B 是 type 定义的数据类型，并且 A 类没有实现 B type定义的数据类型，则断言关系和第2项的规则完全相同。

```javascript
type People = {
  username: string, age: number, address: string, phone: string
}
class Stu {
  public username!: string
  public age!: number;
  public address!: string
  constructor(username: string, age: number, address: string) {
    this.address = address;
  }
}

let people: People = { username: "wangwu", age: 23, address: "11", phone: "111" }
let result = people as Stu;//正确

let stu = new Stu("wangwu", 23, "北京")
stu as People;// 正确

```

##### （7）如果 A 是一个函数上参数变量的联合类型，例如 string |number，那么在函数内部可以断言成 string 或number 类型。

```javascript
function selfMutiply(one: string | number) {
    one as number +3;
}
```

##### （8）多个类组成的联合类型断言

例如：let vechile: Car | Bus | Trunck。 vechile 可以断言成其中任意一种数据类型。 例如 vechile as Car， vechile as Bus ， vechile as Trunck 。

```typescript
class Vechile {
  static count: number = 3;
  public brand: string;
  public vechileNo: string;
  public days: number;
  public total: number = 0;
  public deposit: number;
  constructor(brand_: string, vechileNo_: string, days_: number, deposit_: number) {
    this.brand = brand_;
    this.vechileNo = vechileNo_;
    this.days = days_;
    this.deposit = deposit_;
  }
  public calculateRent() {
  }
  payDesposit() {
  }

  public safeShow() {
  }
}

class Car extends Vechile {
  public type: string;
  constructor(brand_: string, vechileNo_: string, days_: number,
    deposit_: number, type_: string) {
    super(brand_, vechileNo_, days_, deposit_);
    this.type = type_;
  }
  public getPriceByType() {
  }

  public calculateRent() {
  }
  public checkIsWeigui(isOverWeight: boolean) {
  }
}

class Bus extends Vechile {
  public seatNum: number
  constructor(brand_: string, vechileNo_: string, days_: number,
    deposit_: number, seatNum_: number) {
    super(brand_, vechileNo_, days_, deposit_);
    this.seatNum = seatNum_;
  }

  public getPriceBySeatNum() {
  }

  public checkIsOverNum(isOverWeight: boolean) {
  }
}

class Truck extends Vechile {
  ton!: number
  constructor(brand_: string, type_: string,
    days_: number, deposit_: number, ton_: number) {
    super(brand_, type_, days_, deposit_);
    this.ton = ton_;
  }

  checkIsOverWeight(isOverWeight: boolean) {
  }
  CalRentPrice() {
  }
  public calRent() {
  }

  public calDesposit() {
  }
}

class Customer {
  rentVechile(vechile:Bus | Truck | Car) {
     // 下面进行断言、转换
    //<Bus>vechile <==等同==> vechile as Bus
    //vechile as unknown
    //vechile.calculateRent()
    //(vechile as Bus).checkIsOverNum(false)
  }
}
```

##### （9）**任何数据类型都可以转换成 any 或 unknown 类型**，any 或 unknown 类型也可以转换成任何其他数据类型。

```javascript
function add(one: string | number, two: string | number) {
  return one as any + two as any
}

console.log(add(3, 5))
console.log(add("3", 5))
```

#### 4.类型断言存在的意义和应用场景：

**场景1**：顾客在执行汽车租赁项目租赁价格计算方法中调用每一个类的独有方法时使用

**场景2：**对象中的 Symbol 数据类型取值问题

**场景3：** 加法计算，巧用 as any。

```javascript
let symid = Symbol("objid")
let obj = { [symid]: 101, username: "wangwu", age: 23 }
let username = obj["username"]
//let objid=obj[symid]//类型“symbol”不能作为索引类型使用
// 解决:
let objid = obj[symid as any]
//let objid2 = obj[symid as unknown]//类型“unknown”不能作为索引类型使用
//let symidunknown = symid as unknown// 可以转换成unknown,正确

```

**脚踏实地行，海阔天空飞**