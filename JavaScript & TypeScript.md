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

