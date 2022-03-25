## 闭包

闭包原理 作用 优点 缺点 如何回收内存消耗与内存泄漏问题

## 定时器

setTimeout()：`setTimeout`函数用来指定某个函数或某段代码，在多少毫秒之后执行。它返回一个整数，表示定时器的编号，以后可以用来取消这个定时器。

setInterval()：`setInterval`函数的用法与`setTimeout`完全一致，区别仅仅在于`setInterval`指定某个任务每隔一段时间就执行一次，也就是无限次的定时执行

## interface与type的区别

**不同点：**

- 扩展语法： interface使用extends，type使用‘&’
- 同名合并：interface 支持，type 不支持。
- 描述类型：对象、函数两者都适用，但是 type 可以用于基础类型、联合类型、元祖。
- 计算属性：type 支持计算属性，生成映射类型,；interface 不支持。

**相同点：**

- 两者都可以用来描述对象或函数的类型
- 两者都可以实现继承

总的来说，公共的用 interface 实现，不能用 interface 实现的再用 type 实现。主要是一个项目最好保持一致。

```typescript
// 1.语法不同
interface Point {
  x: number;
  y: number;
}
interface SetPoint {
  (x: number, y: number): void;
}
type Point = {
  x: number;
  y: number;
};
type SetPoint = (x: number, y: number) => void;

// 2.interface可以定义多次，并将被视为单个接口
//These two declarations become:
//interface Point { x: number; y: number; }
interface Point { x: number; }
interface Point { y: number; }
const point: Point = { x: 1, y: 2 };

// 3.extends方式不同
interface PartialPointX { x: number; }
interface Point extends PartialPointX { y: number; }

type PartialPointX = { x: number; };
type Point = PartialPointX & { y: number; };

type PartialPointX = { x: number; };
interface Point extends PartialPointX { y: number; }

interface PartialPointX { x: number; }
type Point = PartialPointX & { y: number; };

//4. type可以用于更多的类型：与接口不同，类型别名还可以用于其他类型，如基本类型（原始值）、联合类型、元组。
// primitive
type Name = string;
// object
type PartialPointX = { x: number; };
type PartialPointY = { y: number; };
// union
type PartialPoint = PartialPointX | PartialPointY;
// tuple
type Data = [number, string];
// dom
let div = document.createElement('div');
type B = typeof div;

//5.type 能使用 in 关键字生成映射类型，但 interface 不行。
type Keys = "firstname" | "surname"
type DudeType = {
  [key in Keys]: string
}
const test: DudeType = {
  firstname: "Pawel",
  surname: "Grzybek"
}
// 报错
//interface DudeType2 {
//  [key in keys]: string
//}
```

## ts中 infer 关键字

在编写时对未知类型的推断，在使用时按照给定泛型的匹配关系推断出正确类型

```typescript
type Flatten<T> = T extends Array<infer Item> ? Item : T
type strAry = Flatten<string[]>
```

## 变量提升

变量提升和 JavaScript 的编译过程密切相关：JavaScript 和其他语言一样，都要经历编译和执行阶段。在这个短暂的**编译阶段**，JS 引擎会搜集所有的变量声明，并且提前让声明生效。而剩下的语句需要等到执行阶段、等到执行到具体的某一句时才会生效，这就是变量提升背后的机制。

**作用域是指在程序中定义变量的区域，该位置决定了变量的生命周期。通俗理解，作用域就是变量与函数的可访问范围，即作用域控制着变量和函数的可见性和生命周期**

好处：提高性能，容错率更好

## hash模式与history模式

hash模式：利用了`window`可以监听`onhashchange`事件，也就是说你的url中的哈希值（#后面的值）如果有变化，前端是可以做到监听并做一些响应，即使前端并没有发起http请求他也能够找到对应页面的代码块进行按需加载。例如网易云：https://music.163.com/#/friend

history模式：使用`pushState`与`replaceState`方法将url替换并且不刷新页面，http并没有去请求服务器该路径下的资源，一旦刷新就会暴露这个实际不存在的“羊头”，显示404。解决方法：后端重定向

## 线程和进程

### 区别：

- 线程在进程下行进（单纯的车厢无法运行）
- 一个进程可以包含多个线程（一辆火车可以有多个车厢）
- 不同进程间数据很难共享（一辆火车上的乘客很难换到另外一辆火车，比如站点换乘）
- 同一进程下不同线程间数据很易共享（A车厢换到B车厢很容易）
- 进程要比线程消耗更多的计算机资源（采用多列火车相比多个车厢更耗资源）
- 进程间不会相互影响，一个线程挂掉将导致整个进程挂掉（一列火车不会影响到另外一列火车，但是如果一列火车上中间的一节车厢着火了，将影响到所有车厢）

### js如何实现多线程：

**Web Worker** 是 HTML5 提供的一个javascript**多线程解决方案**，可以将一些大计算量的代码交由web Worker运行而不冻结用户界面 web worker有两个好处：快速、不阻塞浏览器响应

web worker不能操作DOM，适合运算型操作：长文本格式化、语法高亮、图片处理、图片合成、大数组处理

## 前端如何进行seo优化

（1）合理的title、description、keywords

```html
<title>“网站名称-主关键词或一句含有主关键词的描述</title>

<meta name=”Description” Content=”你网页的简述”>

<meta name=”Keywords” Content=”关键词1,关键词2,关键词3,关键词4″>
```

（2）html语义化

（3）非装饰性图片必须加alt

（4）友情链接，好的友情链接可以快速的提高你的网站权重

（5）外链，高质量的外链，会给你的网站提高源源不断的权重提升

（6）向各大搜索引擎登陆入口提交尚未收录站点

（7）重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，保证重要内容一定会被抓取

（8）少用iframe：搜索引擎不会抓取iframe中的内容

（9）提高网站速度：网站速度是搜索引擎排序的一个重要指标

## 单页面spa的seo优化

### 不友好的原因

- 爬虫在爬取的过程中，不会去执行js，所以隐藏在js中的跳转也不会获取到
- vue通过js控制路由然后渲染出对应的页面，而搜索引擎蜘蛛是不会去执行页面的js的，导致搜索引擎蜘蛛只能收录index.html一个页面，在百度中就搜索不到相关的子页面的内容。
- 我们加载页面的时候,浏览器的渲染包含:html的解析、dom树的构建、cssom构建、javascript解析、布局、绘制,当解析到javascript的时候才回去触发vue的渲染,然后元素挂载到id为app的div上,这个时候我们才能看到我们页面的内容,所以即使vue渲染机制很快我们仍然能够看到一段时间的白屏情况,用户体验不好

### 引起的问题

- 收录的页面少了->被抓取的页面就少了->点击量之类的也就少了；
- 不能对对应的页面做TDK(title, keywords, description)不同的配置，每个页面的title和meta标签都是一样的，不利于网络爬虫的爬取

### 怎么做

- 页面预渲染，prerender-spa-plugin插件实现
- 服务端渲染，vue的ssr渲染
- 路由采用h5 history模式

## 获取用户信息：session token 与cookie 

JWT：token生成于服务端，存储在客户端，服务端不用存储，用户后面每次登录都携带首次都登录生成的token字符串用于验证，能做到这点，关键就是token使用的某种算法根据用户签名和其它一些信息生成的令牌信息是一致的，可以验证通过，对于用户量庞大的系统，或者分布式，避免了大量session对象的存储带来的内存消耗，和各服务器之间session的复制或者专门用于存储session的服务器宕机带来的问题

## js共享传递和按值传递

基本类型和引用类型在传递参数时都是**按值传递**的，在向参数传递基本类型的值时，**被传递的值会被复制给一个局部变量**（就是命名参数，arguments对象中的一个元素）；在向参数传递引用类型的值时，会**把这个值在内存中的地址复制给一个局部变量**，因此这个局部变量的变化会反应在函数的外部

- 按值传递：传内存拷贝
- 按引用传递：传内存指针
- 按共享传递：传引用的拷贝

## js设计模式

### 工厂模式

不关心内部操作流程， 只关注传入参数和产出结果

### 单例模式

单例模式就是保证一个类仅有一个实例，并提供一个访问它的全局访问点。其实这有一点像我们vuex当中的实现，也是一个全局的状态管理，并且提供一个接口访问。

```js
var Singleton = function (name) {
    this.name = name;
}

Singleton.prototype.getName = function () {
    console.log(this.name);
}

Singleton.getInstance = (function(){
        var instance = null;
        return function(name){
            if(!instance){
                instance = new Singleton(name);
            }
            return instance;
        }
    }
)()

var a = Singleton.getInstance('alan1');
var b = Singleton.getInstance('alan2');

console.log(a===b); //true
```

### 适配器模式

解决两个软件实体间的接口不兼容问题，使用之后就可以一起工作了。

```js
var googleMap = {
    show: function () {
        console.log('googleMap show!');
    }
}
var baiduMap = {
    show: function () {
        console.log('baiduMap show!');
    }
}

var renderMap = function (map) {
    if (map.show instanceof Function) {
        map.show()
    }
}
renderMap(googleMap);
renderMap(baiduMap);
//上面这段程序能够运行是因为百度地图和谷歌地图用的同一种show方法，但是我们在不知道对方使用的函数接口的时候，我们就不能这样用了（可能百度是使用了display方法来显示）。下面的baiduMapAdapter就是我们使用的适配器。

var baiduMap = {
    display: function () {
        console.log('baiduMap show!');
    }
}
var baiduMapAdapter = {
    show:function(){
        return baiduMap.display()
    }
}
renderMap(baiduMapAdapter);
```

### 代理模式

我们在事件代理的时候其实就是使用了代理模式，通过把监听事件全部交由父节点进行监听，这样你添加节点或者删除节点的时候就不用去改变监听的代码。

### 发布-订阅模式

类似vue的双向绑定。

### 策略模式

根据情况进行不一样的方案，比如你想去旅游，明确自己有多少钱然后选择旅游方式。

```js
var strategies = {
    "rich": function () {
        console.log("You can go with plane!");
    },
    "poor": function () {
        console.log("OH, You can go with your feet!");
    },
    "middle": function () {
        console.log("You can go with train!");
    }
}
var howShouldGo = function (money) {
    return strategies[money]();
}
console.log(howShouldGo("rich"));
```

### 迭代器模式

迭代器模式是指提供一种按顺序访问的方法。比如说我们经常使用的forEach方法，就是通过顺序访问的模式。我们可以自己去写一下forEach的方法。

```js
var myForEach = function (arr, callback) {
    for (var i = 0, l = arr.length; i < l; i++) {
        callback.call(arr[i], i, arr[i]) //把元素以及下标传递出去
    }
}f

myForEach([1, 2, 3], function (item, n) {
    console.log([item, n]);
})
//[ 0, 1 ]
//[ 1, 2 ]
//[ 2, 3 ]
```

## ajax

Ajax是一种用于创建快速动态网页的技术。通过在后台与服务器进行少量数据交换，Ajax可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。而传统的网页(不使用 Ajax)如果需要更新内容，必需重载整个网页面。

1. 创建XMLHttpRequest对象
2. 设置响应HTTP请求状态变化的回调函数
3. 使用open方法与服务器建立连接
4. 向服务器发送数据
5. 在回调函数中针对不同的响应状态进行处理

## 如何判断一个数是否为正整数

使用正则表达式：/(^[1-9]\d*$)/

## undefined与null

`null`

- 表示没有对象，即该处不应该有值，用法如下：
- 作为函数的参数，表示该函数的参数不是对象；
- 作为原型链的终点。

`undefined`

- 表示缺少值，就是此处应该有一个值，但是还没有定义，情况如下：
- 变量被声明了，但没有赋值时，就等于undefined；
- 调用函数时，应该提供的参数没有提供，该参数等于undefined；
- 对象没有赋值的属性，该属性的值为undefined；
- 函数没有返回值的时，默认返回undefined；

## ==与===

当进行双等号比较时候： 先检查两个操作数数据类型，如果相同， 则进行===比较， 如果不同， 则愿意为你进行一次类型转换， 转换成相同类型后再进行比较， 而===比较时， 如果类型不同，直接就是false

## ES5的继承和ES6的继承有什么区别？

ES5的继承是通过prototype或构造函数机制来实现。

ES5的继承实质上是先创建子类的实例对象，然后再将父类的方法添加到this上(Parent.apply(this))。

ES6的继承机制实质上是先创建父类的实例对象this(所以必须先调用父类的super()方法)，然后再用子类的构造函数修改this。具体为ES6通过class关键字定义类，里面有构造方法，类之间通过extends关键字实现继承。子类必须在constructor方法中调用super方法，否则新建实例报错。因为子类没有自己的this对象，而是继承了父类的this对象，然后对其调用。如果不调用super方法，子类得不到this对象。

注意：super关键字指代父类的实例，即父类的this对象。在子类构造函数中，调用super后，才可使用this关键字，否则报错。

## Cookie属性配置

- name/value：键值对，可以设置要保存的 Key/Value
- expires：过期时间，在设置的某个时间点后该 Cookie 就会失效。Cookie中的maxAge用来表示该属性，单位为秒。maxAge有3种值，分别为正数，负数和0。
- domain：指定了 Cookie 可以送达的主机名。假如没有指定，那么默认值为当前文档访问地址中的主机部分（但是不包含子域名）。但不能将一个cookie的域设置成服务器所在的域之外的域。是无效的。
- path：Path 指定了一个 URL 路径，这个路径必须出现在要请求的资源的路径中才可以发送 Cookie 首部。设置为"/"表示允许所有路径都可以使用Cookie（ Domain 和 Path 标识共同定义了 Cookie 的作用域：即 Cookie 应该发送给哪些 URL）
- Secure：标记为 Secure 的 Cookie 只应通过被HTTPS协议加密过的请求发送给服务端。使用 HTTPS 安全协议，可以保护 Cookie 在浏览器和 Web 服务器间的传输过程中不被窃取和篡改。
- HTTPOnly：设置 HTTPOnly 属性可以防止客户端脚本通过 document.cookie 等方式访问 Cookie，有助于避免 XSS 攻击。
- SameSite：SameSite 属性可以让 Cookie 在跨站请求时不会被发送，从而可以阻止跨站请求伪造攻击（CSRF）。
  SameSite 可以有下面三种值：
  1、Strict仅允许一方请求携带 Cookie，即浏览器将只发送相同站点请求的 Cookie，即当前网页 URL 与请求目标 URL 完全一致。
  2、Lax允许部分第三方请求携带 Cookie
  3、None无论是否跨站都会发送 Cookie
  造成现在无法获取cookie是因为之前默认是 None 的，Chrome80 后默认是 Lax。

## WeakMap

特性：

- `WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名
- `WeakMap`的键名所指向的对象，不计入垃圾回收机制

语法：

- `WeakMap`不可遍历（没有`keys()`、`values()`和`entries()`方法，且没有`size`属性
- `WeakMap`无法清空（不支持`clear`）。固只有四个方法可用：`get()`、`set()`、`has()`、`delete()`。

功能：`WeakMap`的专用场合就是，它的键所对应的对象，可能会在将来消失。`WeakMap`结构有助于防止内存泄漏，主要使用情况如下：

- DOM 节点作为键名
- 部署私有属性

## 正则表达式

```
附件参数：
g：代表可以进行全局匹配。
i：代表不区分大小写匹配。
m：代表可以进行多行匹配。
```

