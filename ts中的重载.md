### 深度掌握TypeScript中的重载【函数重载、方法重载】

#### 1. 函数重载，方法重载的重要性

著名前端流行框架底层都用到函数重载，例如：Vue3 底层源码就多处使用到带泛型的函数重载。很多前端面试更是拿函数重载作为考核求职者 TS 技能是否扎实的标准之一，如果你不掌握函数重载，等于你的 TS 技能有缺失，技能不过关。

函数重载或方法重载适用于完成项目种某种相同功能但细节又不同的应用场景，我们举一个生活中的例子让同学们先有个印象，比如：吃饭是一个函数，表示一个吃饭功能，但西方人用叉子，中国人用筷子，这就是细节不同，那如果我们可以用函数重载来解决。

不管现阶段你公司的项目中是否用到了函数重载和方法重载【如果没有用，多半是公司不少人用的并不熟练才不用的缘故】。

函数重载或方法重载有以下几个优势：

##### 优势1： 结构分明

让 代码可读性，可维护性提升许多，而且代码更漂亮。

##### 优势2： 各司其职，自动提示方法和属性

每个重载签名函数完成各自功能，输出取值时不用强制转换就能出现自动提示，从而提高开发效率

##### **优势3： **更利于功能扩展

#### 2.函数重载的定义

TS 的函数重载比较特殊，和很多其他后端语言的方法重载相比，多了不少规则。学习函数重载，先要了解什么是函数签名，定义如下：

**函数签名** [ function signature ]：函数签名=函数名称+函数参数+函数参数类型+返回值类型四者合成。在 TS 函数重载中，包含了实现签名和重载签名，实现签名是一种函数签名，重载签名也是一种函数签名。

关于函数重载的定义，我们先来看一个很多其他资料提供的不完整且模糊的TS函数重载定义：

**不完整模糊的 TS 函数重载定义**：一组具有相同名字，不同参数列表的和返回值无关的函数 。

**完整的函数重载定义**：包含了以下规则的一组函数就是TS函数重载 :

**规则1：**由一个实现签名+ 一个或多个重载签名合成。

**规则2：** 但外部调用函数重载定义的函数时，只能调用重载签名，不能调用实现签名，这看似矛盾的规则，其实 是TS 的规定：**实现签名下的函数体是给重载签名编写的，实现签名只是在定义时起到了统领所有重载签名的作用，在执行调用时就看不到实现签名了。**

**规则3：**调用重载函数时，会根据传递的参数来判断你调用的是哪一个函数 

**规则4:**  只有一个函数体，只有实现签名配备了函数体，所有的重载签名都只有签名，没有配备函数体。

**规则5:  关于参数类型规则完整总结如下：**

实现签名参数个数可以少于重载签名的参数个数，但实现签名如果准备包含重载签名的某个位置的参数 ，那实现签名就必须兼容所有重载签名该位置的参数类型【联合类型或 any 或 unknown 类型的一种】。

**规则6： 关于重载签名和实现签名的返回值类型规则完整总结如下：**

必须给重载签名提供返回值类型，TS 无法默认推导。

提供给重载签名的返回值类型不一定为其执行时的真实返回值类型，可以为重载签名提供真实返回值类型，也可以提供  void 或 unknown 或 any 类型，如果重载签名的返回值类型是 void 或 unknown 或 any 类型，那么将由实现签名来决定重载签名执行时的真实返回值类型。 当然为了调用时能有自动提示+可读性更好+避免可能出现了类型强制转换，强烈建议为重载签名提供真实返回值类型。

不管重载签名返回值类型是何种类型，实现签名都可以返回 any 类型 或 unknown类型，当然一般我们两者都不选择，让 TS 默认为实现签名自动推导返回值类型。

#### 3.方法重载

**方法：**方法是一种特定场景下的函数，由对象变量【实例变量】直接调用的函数都是方法。

比如：

1. 函数内部用 this 定义的函数是方法； 

2. TS  类中定义的函数是方法【 TS  类中定义的方法就是编译后  JS 底层 prototype 的一个函数】； 
3. 接口内部定义的函数是方法【注意：不是接口函数】；
4. type  内部定义的函数是方法【注意：不是 type 函数】。

**方法签名：**和函数签名一样，方法签名 = 方法名称 + 方法参数 + 方法参数类型 + 返回值类型四者合成。

#### 4.构造器 【构造函数】重载

##### （1）再次强化this

this 其实是一个对象变量，当 new 出来一个对象时，构造器会隐式返回 this  给 new 对象等号左边的对象变量，this 和等号左边的对象变量都指向当前正创建的对象。

以后，哪一个对象调用 TS 类的方法，那么这个方法中的 this 都指向当前正使用的对象【 this 和当前的对象变量中都保存着当前对象的首地址】

##### （2）TS构造器没有返回值

尽管TS类构造器会隐式返回 this，如果我们非要返回一个值，TS 类构造器只允许返回 this，但构造器不需要返回值也能通过编译，更没有返回值类型之说，从这个意义上，TS 构造器可以**说成是没有返回值这一说**的构造函数。【**注意：TS 构造器和 JS 构造函数关于返回值的说法不完全相同**】

##### （3）构造器不是方法

我们说对象调用的才是方法，但是 TS 构造器是在对象空间地址赋值给对象变量之前被调用，而不是用来被对象变量调用的，所以构造器( constructor )可以说成构造函数，但不能被看成是一个方法。

##### （4）构造器【构造函数】重载的意义

构造器重载和函数重载使基本相同，主要区别是：TS 类构造器重载签名和实现签名都不需要管理返回值，TS 构造器是在对象创建出来之后，但是还没有赋值给对象变量之前被执行，一般用来给对象属性赋值。

我们知道在 TS 类中只能定义一个构造器，但实际应用时，TS 类在创建对象时经常需要用到有多个构造器的场景，比如：我们计算一个正方形面积，创建正方形对象，可以给构造器传递宽和高，也可以给构造器传递一个包含了宽和高的形状参数对象，这样需要用构造器重载来解决。而面试中也多次出现过关于TS构造器重载的考察，主要考察求职者对重载+构造器的综合运用能力。

#### 5.函数重载实战

场景：聊天应用中，发送的消息分为很多类型，例如：图片、文字、语音等等，现在需要封装一个消息查找函数，根据传入的参数从数组中查找数据，如果入参为数字， 就认为消息 id，然后从从后端数据源中找对应 id 的数据并返回，否则当成类型，返回这一类型的全部消息。

代码1，**不使用函数重载**的特性：

```typescript
type MessageType = "image" | "audio" | string;//消息类型
//type xtype=string
//boolean true false
type Message = {
  id: number;
  type: MessageType;
  sendmessage: string;
};
//let msgobj:Message={id:23,type:"df",sendmessage:"abc"}
//let obj={username:"wangwu",age:23}
let messages: Message[] = [
  //let messages: Array<Message> = [
  {
    id: 1, type: 'image', sendmessage: "你好啊,今晚咱们一起去三里屯吧",
  },
  {
    id: 2, type: 'audio', sendmessage: "朝辞白帝彩云间，千里江陵一日还"
  },
  {
    id: 3, type: 'audio', sendmessage: "你好！张无忌"
  },
  {
    id: 4, type: 'image', sendmessage: "刘老根苦练舞台绝技！"
  },
  {
    id: 5, type: 'image', sendmessage: "今晚王牌对王牌节目咋样?"
  }]

//不用函数重载来实现
// 1.函数结构不分明,可读性，可维护性变差
function getMessage(value: number | MessageType):
  Message | undefined | Array<Message> {
  if (typeof value === "number") {
    return messages.find((msg) => { return value === msg.id })
  } else {
    //return messages.filter((msg) => { return value === msg.type })
    return messages.filter((msg) => value === msg.type)
  }
}
// 自定义守卫
console.log(getMessage("audio"));
// TS没有办法运行之前根据传递的值来推导方法最终返回的数据的数据类型
// 只可以根据方法定义的类型展现
//let msg=getMessage(1) 
//console.log(msg.sendMessage)//错误 类型“Message | Message[]”上不存在属性“sendMessage”。
//  类型“Message”上不存在属性“sendMessage”，联合类型使用方法，必须是交集（公共方法）！
let msg = (<Message>getMessage(1)).sendmessage // 类型转换成Message或类型断言
console.log("msg:", msg)// msg: 你好啊,今晚咱们一起去三里屯吧

export { }
```

代码2，使用**函数重载**实现：

```typescript
type MessageType = "image" | "audio" | string;//消息类型

type Message = {
  id: number;
  type: MessageType;
  sendmessage: string;
};

let messages: Message[] = [
  //let messages: Array<Message> = [
  {
    id: 1, type: 'image', sendmessage: "你好啊,今晚咱们一起去三里屯吧",
  },
  {
    id: 2, type: 'audio', sendmessage: "朝辞白帝彩云间，千里江陵一日还"
  },
  {
    id: 3, type: 'audio', sendmessage: "你好！张无忌"
  },
  {
    id: 4, type: 'image', sendmessage: "刘老根苦练舞台绝技！"
  },
  {
    id: 5, type: 'image', sendmessage: "今晚王牌对王牌节目咋样?"
  }]

// function getMessage(value: number | MessageType):
//   Message | undefined | Array<Message> {
//   if (typeof value === "number") {
//     return messages.find((msg) => { return value === msg.id })
//   } else {
//     //return messages.filter((msg) => { return value === msg.type })
//     return messages.filter((msg) => value === msg.type)
//   }
// }
// 重载签名可以有多个
function getMessage(value: number, myname: string): Message//第一个根据数字id来查询单个消息的 重载签名
function getMessage(value: MessageType, readRecordCount: number): Message[]//第二个根据消息类型来查询消息数组的 重载签名

// 实现签名函数，只有实现签名才有函数体，实现签名只有一个
function getMessage(value: any, value2: any = 1) {
  //console.log(myname)
  if (typeof value === "number") {
    return messages.find((msg) => { return 6 === msg.id })//undefined
  } else {
    //return messages.filter((msg) => { return value === msg.type })
    return messages.filter((msg) => value === msg.type).splice(0, value2)
  }
}
getMessage(1, "df") // 这里调用的是第一个重载签名
// let x: number = 3;
// let y: any = x;

// let z: any = 3;
// let k: number = z;

// function go(value:number,readRecordCount:number=1){

// }
// go(1);


//console.log(getMessage(6).sendmessage); 根据传参，判断为第一个重载签名，所以这里的sendmessage可以正常调用，不需要类型转换
getMessage("image", 2).forEach((msg) => {
  console.log(msg);
})

export { }
```

#### 6.方法重载实战

场景： Java 简易版 ArrayList 中的经典应用实现【 ArrayList 可弥补 Set 取值不方便短板，比 Set 删除功能更方便】

代码：

```typescript
//  1.对现有的数组进行封装，让数组增删改变得更加好用
//  2.提供get方法 remove方法 显示方法【add方法】
// 其中需求中的remove方法有两个，我们用方法重载来实现

class ArrayList {
  //第一步：定义一个引用属性【数组】
  constructor(public element: Array<object>) {

  }
  // 第二步：根据索引来查询数组中指定元素
  get(index: number) {
    return this.element[index]
  }

  // 第三步: 显示方法
  show() {
    this.element.forEach((ele) => {
      console.log(ele);
    })
  }

  remove(value: number): number
  remove(value: object): object
  //remove(value: number | object): number | object {
  remove(value: any): any {
    this.element = this.element.filter((ele, index) => {
      //如果是根据数字【元素索引】去删除元素，remove方法返回的是一个数字
      if (typeof value === "number") {
        return value !== index
      } else {
        // 如果是根据对象去删除元素，remove方法返回的是一个对象
        return value !== ele
      }
    })
    return value;
  }

}

let stuOne = { stuname: "wnagwu", age: 23 }
let stuTwo = { stuname: "lisi", age: 39 }
let stuThree = { stuname: "liuqi", age: 31 }

let arrayList = new ArrayList([stuOne, stuTwo, stuThree]);
arrayList.show();

console.log("删除第一个学生");
// let value = arrayList.remove(0)
// console.log("删除的元素为第:", value, "学生")
// arrayList.show();
let value = arrayList.remove(stuTwo)
console.log("删除的学生对象为:", value)
arrayList.show();
// 如果是根据数字【元素索引】去删除元素，remove方法返回的是一个数字
// 如果是根据对象去删除元素，remove方法返回的是一个对象
//let value=arr.remove(1)

```

#### 7.构造器重载实战

场景：计算正方形面积

```typescript
// 计算正方形面积
// 计算创建正方形对象，可以给构造器传递宽和高
// 也可以给构造器传递一个包含了宽和高的形状参数对象，这样需要用构造器重载
type type_ChartParam = { // 各个图形求面积参数
  width?: number,
  height?: number,
  radius?: number
}

class Square {
  public width: number;
  public height: number;

  constructor(width_: number, height_: number) // 重载签名
  constructor(value_: type_ChartParam) // 重载签名
  // constructor(value_: number | type_ChartParam) { // 实现签名
  constructor(paramObjOrWidth_: any, height_: number = 0) { // 实现签名
    if (typeof paramObjOrWidth_ === 'object') {
      this.width = paramObjOrWidth_.width;
      this.height = paramObjOrWidth_.height;
    } else {
      this.width = paramObjOrWidth_;
      this.height = height_;
    }

  }

  public getArea(): number {
    return this.height * this.width
  }
}

let square1 = new Square(40, 50);
let chartParamObj: type_ChartParam = {width: 50, height: 90}
let square2 = new Square(chartParamObj);
console.log(square1.getArea()); // => 2000
console.log(square2.getArea()); // => 4500

```



**脚踏实地行，海阔天空飞！**





