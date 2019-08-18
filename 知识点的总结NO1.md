## 什么是盒子模型

把所有的网页元素都看成一个盒子，它具有content，padding，border，margin 
四个属性，这就是盒子模型

盒子模型有两种形式：标准盒子模型，怪异盒子模型

标准模式，总宽度= width + margin(左右) + padding(左右) + border(左右)

怪异模式，总宽度= width + margin(左右)（即width已经包含了padding和border值）

## 如果页面不写doctype页面还会解析页面吗？

会，会按照浏览器各自模式进行解析（俗称：嗅探）

## 定位有几种方式？每种方式定位的基准是什么吗？那种方式会脱标？他们之间会有层级关系吗，谁的层级会高些？

1. position：static       静态定位 ，不脱标，不能配合方位名词进行移动
2. position：relative    相对定位 ，不脱标，可以配合方位名词进行移动，相对自己原来的位置移动
3. position：absolute   绝对定位   脱标  配合方位名词移动。相对最近的有定位的祖先元素移动，如果祖先元素都没有定位，相对于浏览器进行移动
4. position：fixed   固定定位  脱标 配合方位名词移动  只相对浏览器进行移动

标准流 < 浮动 < 定位（相对、绝对、固定）  定位的层级关系

相对、绝对、固定三者的层叠关系**相同**，HTML中写在下面元素的覆盖上面的，可以通过z-index进行层级控制

## 浮动会带来哪些影响，哪些方式可以清除浮动

浮动元素脱标，造成父盒子塌陷，所以需要清除浮动带来的影响

清除浮动的方式有5种，

1.设高法：不推荐，如果内容不是固定的会出现bug，比如超出盒子。

2.额外标签法  clear：both；在父元素**里面的最后**的添加一个**块级元素**，然后给添加的块级元素设置清除浮动的核心代码 clear:both;  不推荐：设置后，页面中会显示一些不必要的盒子。

3.单伪元素清除法

```
.clearfix::after {
/*伪元素必加的属性*/
content: '';
/*转换为块级元素*/
display: block;
/*清除浮动的核心代码*/
clear: both;
/*其实有上面三个属性已经可以生效了，但是可能在开发的时候，会有额外的几个属性！！*/
/*目的：在页面中看不到伪元素*/
height: 0;
line-height: 0;
visibility: hidden;
}
```

4.双伪元素清除法（增加了防塌陷功能）

```
.clearfix::before,
.clearfix::after {
content: "";
/* BFC + 转化为块级元素 */
display: table;
}
.clearfix::after {
clear: both;
}
/* 兼容 IE 67 目前需要兼容的浏览器版本最多到 IE8 */
.clearfix {
*zoom: 1;
}
```

5.overflow：hidden

## 页面之间传参的方式有哪些

1. **H5的数据存储：**sessionStorage和localStorage
2. **通过url来传参**，参数拼接在地址栏上 通过window.location.search()获取参数
3. **通过cookie也是可以**，但是由于他的缺点，我们现在开发极少用它

## **H5/C3新属性如何做兼容处理？**（IE8开始不兼容h5c3）

c3兼容问题

1. 增加各个版本的浏览器内核前缀 IE Trident  、Chrome  blink、Firefox Gecko、Safari webkit、 Opera blink
2. 优雅降级和渐进增强

**h5兼容问题使用Google的html5shiv包可以解决**

```js
<!--[if lt IE9]> 
<script src="http://html5shiv.googlecode.com/svn/trunk/html5.js </script>
<![endif]-->
```

##  优雅降级和渐进增强

**渐进增强** progressive enhancement：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。

**优雅降级** graceful degradation：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

**区别**：优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要。降级（功能衰减）意味着往回看；而渐进增强则意味着朝前看，同时保证其根基处于安全地带

## 浏览器兼容问题（了解）

1.**图片间隙**：

```
高版本:在div没有设置高度的情况下，图片会将盒子在自身高度的基础下，在撑大3-5px
低版本：在div设置了高度的情况下会将盒子撑大3-5px。
解决办法：1.给父元素加font-size：0
```

2.**双倍边距**：低版本中解析浮动元素时会错误的把浮动向边界加倍显示；

解决办法：浮动元素添加display：inline

3.当li中有a和span标签并给span设置浮动li在IE中有间距问题

解决办法：

1.zoom:1

2.overflow:hidden

3.verstical-align:top

4.img放在超链接a里面，在IE5的低版本浏览器中会有蓝色的边框

解决办法：

1.给父元素加font-size:0

2.给img加display：block

5.IE6元素有默认高度：

解决办法：

1.font-size:0

2.overflow:hidden

## 隐藏元素的办法

1. display：none    隐藏不占位
2. visibility：hidden  隐藏占位置
3. position：absolute；  top：-9999px；left：-9999px；不隐藏，消失在屏幕之外
4. opacity：0  透明化 占位置

## 前端性能优化

​     **减少HTTP请求数量**

- CSS Sprites（雪碧精灵图）

- 内联图片(图片base64)

- 最大化合并JS、CSS模块

- 利用浏览器缓存

    **减小HTTP请求大小**

- 压缩HTTP响应包(Accept-Encoding: gzip, deflate)

- 压缩HTML、CSS、JS模块

  ​     **DOM方面**

- 离线操作DOM

- 使用innerHTML进行大量的DHTML操作

- 使用事件代理

- 缓存布局信息

- 移除页面上不存在的事件处理程序

  ​       **JavaScript语言本身的优化**

- 使用局部变量代替全部变量，减少作用域链遍历标识符的时间

- 减少对象成员及数组项的查找次数

- 避免使用with语句和eval函数

  ​      **ajax优化**

- get或者post请求

- multipart XHR

- ajax缓存

  **其他方面的性能优化**

- 使用CDN加载静态资源

- CSS样式放在头部

- JS脚本放在底部

- 避免使用CSS表达式

- 外联JS、CSS

- 减少DNS查找

- 避免URL重定向

## 添加、删除、替换、插入到某个节点的方法？

```
appendChild(node) 添加到元素列表的最后面，如果在页面中存在，起着剪切的效果
//添加
removeChild(node)
//删除节点
insertBefore（插入节点，被插节点） 最后一个参数不指定，默认插在元素的后面，与appendChild()作用相同，指定了就插值这个节点前面
replaceChild（新节点,旧节点）// 替换
注意点：均由父元素调用
```

## css画出一个三角形

通过border的尺寸来设置： 宽高为0 ，留下要的边框边，其他边框设置为透明

```
width: 0;
height: 0;
border-width: 40px;
border-style: solid;
border-color: red transparent transparent transparent;
```

## css画出一个扇形

```
width: 0;
height: 0;
border-width: 40px;
border-style: solid;
border-color: red transparent transparent transparent;
border-radius:40px; //重点
```



## 浏览器缓存和离线缓存的区别是什么

1. 离线缓存是：H5提供了离线使用的功能，让应用程序可以获取本地的网站内容，提高网站的性能，增加用户的体验。这个是要我们手动的去操作。   

     备注：离线存储是通过manifest文件来管理的，需要服务器端的支持，不同的服务器开启支持的方式也是不同的

2. 浏览器缓存：浏览器的缓存他会对相同的内容进行缓存，以提高访问的速度。这个是浏览器自身的机制所导致的。

   **区别**：

  - 离线存储为整个web提供服务，浏览器只能缓存单个页面

  - 离线存储可以指定需要缓存的文件和需要在线浏览文件，浏览器没法指定

  - 离线存储可以动态通知用户进行更新

## **为什么会有缓存？如果不想缓存如何避免掉**

其根本都是为了提高网站的性能的，为了提高访问网站的速度

如果需要我们手动缓存，那我们就可以用H5的离线缓存来缓存我们需要的静态资源。如果是浏览器这个是自身的机制，他会根据你的url来判断，如果你url是相同的那他就会自己缓存下来。

如果你的后台资源不是一样的，不想缓存。我们需要**在url后面加一个时间戳**，那就不会缓存了，后台不会解析时间戳，但是浏览器不知道，就会每次都请求。

## **H5的wbsocket是什么？应用场景有哪些？和http以及https的区别是什么**

**WebSocke**是HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。应用场景有消息推送

**HTTPS**：是以安全为目标的HTTP通道，简单讲是HTTP的安全版，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。

**HTTP**：超文本传输协议是一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议。

**区别**：

HTTP相对于 HTTPS来说,速度较快且开销较小(没有 SSL/TSL) 对接,默认是80端口;

HTTP容易遭受域名劫持,而HTTPS相对来说就较为安全(加密),默认端口为443。

HTTP是明文跑在 TCP 上.而HTTPS跑在SSL/TLS应用层之下,TCP上的；

https加密方式：对称加密、非对称加密、CA(数字签名)

- 对称加密:

- - 双方都有同样的密钥,每次通讯都要生成一个唯一密钥,速度很快
  - 安全性相对比其他的2种加密方式较低。

- 非对称加密(一般用 RSA)

- - 安全性很高,对资源消耗很大(CPU),目前主流的加密算法(基本用于交换密钥或签名,而非所有通讯内容)

- CA(数字签名):

- - 这个是为了防止中间人窃取数据而诞生的
  - 用一些权威机构颁布的算法来签名,权威机构做中间人,通讯过程都会跟机构核对一遍



## em和rem的区别

1. em是相对自身的font-size，如果没有设置，找父元素的
2. rem是相对于根(html)的font-size大小来改变的

## **移动端页面如何来做适配？或者问：移动页面开发有哪些方式**

1. **流式布局**：固定和百分比配合，例如:两边固定宽度中间用百分比来适配

2. **弹性布局**(flex布局)：这个不需要配合什么东西，也是挺好用的技术

3. **Rem**布局：rem配合媒体查询，或者rem配合通过js进行的媒体查询来实现布局

   备注：c3 媒体查询  @media screen and (max-width: 300px) {...}

   ​	js实现行为和功能的改变媒体查询使用 window.matchMedia()  。只对首次加载或者重载有效

   ​	要想像c3一样可以随页面反应需要 addListener() 来监听页面改变

   ​	

## this的理解

this是js中的关键字，只存在于函数内部，严格模式下this指向undefined，原则上是谁调用指向谁，就是说一般情况下，this一开始是没有指向的，只有调用时才会有，this的指向有4种模式：

1. 函数调用： this指向window 例如  fn()
2. 方法调用：谁调用指向谁    obj.fn()
3. 构造函数调用：this指向实例
4. 格式上下文： call  apply bind 传入的第一个参数都是this的指向
   1. call的后面传入的参数是字符串形式传参
   2. apply 是以数组方式传参
   3. bind返回新的函数，并绑定死this的指向

特殊情况

1. 定时器和计时器this指向window
2. 箭头函数没有自己的this，内部this指向外部的this
3. 事件里的this指向事件源

## js中的new干了什么

```
(1) 创建一个新对象，并指定类型；
(2) 让构造函数内部的this指向实例 ；
(3) 执行构造函数 ；
(4) 返回这个新对象，也就是this
```

## window .onload和document.ready的区别？

1. window.onload需要等到页面的内容全部加载才执行里面的代码，document.ready只要HTML结构加载完成就执行，速度更快
2. onlodad页面只能出现一次，多次使用下面会覆盖上面，ready可以使用多次

## **什么是闭包？闭包产生的背景？有什么问题？怎么解决？举一个闭包的例子**

**什么是闭包：**一个函数内部有另一个函数，内部的函数能使用外部函数的变量和方法。

**闭包产生的背景：**

1. 作用域的问题，全局作用域无法访问局部作用域
2. 假如执行的函数还有用，避免执行完的函数被垃圾回收机制回收，使用闭包。
3. 全局污染的问题，变量和函数名的冲突。不管是当前页面冲突还是团队开发页面合并都会有

**产生的问题：闭包会导致内存泄漏**

解决办法： 尽量避免，如果不能避免，使用完后手动释放，让变量等于null 断掉指引

**举例：页面上li输出相对应的索引值，经典的面试题。**

```js
 for(var i = 0;i < lis.length;i++){
     lis[i].onclick = (function( num ){
         return function(){
          alert( num ) ;
		};
       })( i );
```

**1、继承有哪些方式？你的项目上有应用过继承吗？有自己封装过东西吗？**

**答：**继承的方式很多种，每种的叫法可能有些差别，所以大家在记得时候一是只需记几个常用的，二是你记得这几个方式要能举出一个例子来，大家课下自己补充一下例子，这里就不给你们列出来了。

1. 原型链继承
2. 构造函数继承，用call和apply来实现
3. 混合方式，混合了call方式、原型链方式。

关于项目上是否有用过这个问题，一般我们建议这样回答：因为公司开发更过的是考虑效率的，所以我们都是用的库、框架、插件。因为这些都是经过验证的能更好的应用到项目上而不会出现问题的而且开发效率高。所以我们项目上很少使用的，但是这些库或者插件的封装肯定是用到继承的，所以变向的是用到的了。

至于封装这个问题，你如果封装过就封装过如果没有就没有了。因为如果你说了自己封装过，面试官肯定会问你封装的什么东西的。当然这个你如果能说出来，肯定能给你加很多分数的。

## Js有哪些内置对象？你工作中常用那种？列举几个你常用的方法

JS中内置了十几个对象，例如：Object、Array、Boolean、Number 、String、Function、Arguments、Math、Date、RegExp、Error。

常用的是Array对象、Date对象、正则表达式对象、string对象、Global对象

**Array**
 Concat（）：表示把几个数组合并成一个数组。 
 Join（）：返回字符串值，其中包含了连接到一起的数组的所有元素，元素由指定的分隔符分隔开来。 
 Pop（）：移除数组最后一个元素。 
 Shift（）：移除数组中第一个元素。 
 Slice（start，end）：返回数组中的一段。 
 Push（）：往数组中新添加一个元素，返回最新长度。 
 Sort（）：对数组进行排序。 
 Reverse（）：反转数组的排序。 
 toLocaleString();返回当前系统时间 
 Array对象属性常用的只有一个： 
 Length：表示取得当前数组长度

**String对象**
 charAt() ：返回指定索引的位置的字符 
 concat（）：返回字符串值，表示两个或多个字符串的连接 
 match（）：使用正则表达式模式对字符串执行查找，并将包含查找结果最为结果返回 
 Replace（a，b）：字符b替换a 
 Search（stringObject）：指明是否存在相应的匹配。如果找到一个匹配，search 方法将返回一个整数值，指明这个匹配距离字符串开始的偏移位置。如果没有找到匹配，则返回 -1。 
 Slice（start，end）：返回字符段片段 
 Split（）：字符串拆分 
 Substr（start，length）：字符串截取 
 Substring（start，end）取得指定长度内的字符串 
 toUpperCase（）：返回一个字符串，该字符串中的所有字母都被转化为大写字母。 
 toLowerCase（）：返回一个字符串，该字符串中的所有字母都被转化为小写字母。

## Js中深拷贝和浅拷贝的区别？

浅拷贝拷贝的是引用，深拷贝拷贝的是一个完整的对象，拷贝出来的两个对象直接没有任何关联了

## 冒泡排序

自己去看之前的课程回顾下

## **数组去重**

核心思想是利用 for循环  进行判断

**方法一**：遍历数组，建立新数组，利用indexOf判断是否存在于新数组中，不存在则push到新数组，最后返回新数组

**方法二**：遍历数组，利用object对象保存数组值，判断数组值是否已经保存在object中，未保存则push到新数组并用object[arrayItem]=1的方式记录保存

**方法三**：数组下标判断法, 遍历数组，利用indexOf判断元素的值是否与当前索引相等，如相等则加入

**方法四**：数组先排序， 然后比较俩数组一头一尾进行去重

```
 arr = [1, 2, 3, 4, 1, 1, 2, 4, 4]
    // 输出 [1,2,4]
    Array.prototype.repeNum = function () {
      let new_arr = this.sort(); //先排序
      let res = [];
      for (let i = 0; i < new_arr.length; i++) {
        if (new_arr[i] == new_arr[i + 1] && //判断是否重复,是否已经放入容器
          new_arr[i] != new_arr[i - 1]) {
          res.push(new_arr[i]);
        }
      }
      return res
    }
```

**方法五**：es6语法 arr = [...new Set(arr)];

## 伪数组转换成真数组

伪数组与数组的区别是 伪数组实际上是一个对象，伪数组内有length属性，且不可以使用数组的方法

**第一种**遍历伪数组将值存入到新数组中

```
var aLi = document.querySelectorAll('li');
  var arr = [];
   for (var i = 0; i < aLi.length; i++) {
      arr[arr.length] = aLi[i] 或者  arr.push(weiArr[i]);
   }
```

**第二种**

```
var arr = Array.prototype.slice.call(aLi);
```

**第三种**:从一个类似数组或可迭代对象中创建一个新的，浅拷贝的数组实例。

```
Array.from(aLi)
```

## 事件包含哪些阶段

js里面的事件的三个阶段分别为：捕获、目标、冒泡阶段。

1. 捕获：事件由页面元素接收，逐级向下，到最具体元素的。
2. 冒泡：跟捕获相反，由最具体的元素接收，逐级向上，到页面元素。
3. 目标：最具体的元素。

## **什么是事件委托？事件委托的原理是什么？他有那些应用场景？**

事件委托又叫事件代理，简单理解就是自己的事情交给其他人帮忙做。在js中一般都是子元素的事件交给父元素来做。

事件委托是利用事件的冒泡原理来实现的，通过父元素监听子元素触发的事件。

**为什么要使用:**

绑定事件太多，浏览器占用内存变大，严重影响性能

动态创建的元素无法直接绑定事件，为了方便可以委托父元素注册事件。

## **如何阻止事件冒泡和事件的默认行为**

阻止事件冒泡

- 标准写法：Event.stopPropagation()
- IE兼容写法：window.event.cancelBubble= true

阻止浏览器默认行为

- 标准写法：Event.preventDefault()
- IE兼容写法：window.event.returnValue = false

## Js注册事件有几种方式？有什么区别吗？

1. 标准注册方式：addeventlistener()
2. IE的注册方式：attachEvent()
3. 通用模式：On+事件名

用"addeventlistener" 可以绑定多次同一个事件，且都会执行，而在DOM结构如果绑定两个 "onclick" 事件，只会执行第一个；在js中通过匿名函数的方式绑定的只会执行最后一个事件

## **Jq注册事件有几种方式？他们的区别是什么？举一个事件委托的例子**

jQuery中提供了四种事件监听方式，分别是bind、live、delegate、on，对应的解除监听的函数分别是unbind、die、undelegate、off。

1. bind()函数只能针对已经存在的元素进行事件的设置；但是live(),on(),delegate()均支持未来新添加元素的事件设置。
2. live()：jQuery1.7之后弃用。
3. delegate():jQuery1.7之后被on()取代 
4. on()： jQuery1.7之后引入，支持事件绑定的全部功能

$(“ul”).on(”click“,“lis”,function(e){})

## 回调地狱

什么是回调地狱(函数作为参数层层嵌套)

解决方式:

1. 拆解 function：将各步拆解为单个的 function
2. 通过 Promise 链式调用的方式
3. 通过ES8的异步函数 async / await
4. 尽量避免

## **说说Jsonp的原理？jsonp是用来解决什么问题的？为什么会有这个问题？Jsonp有什么缺点？**

1）ajax请求受同源策略影响，不允许进行跨域请求，而script标签src属性中的链接却可以访问跨域的js脚本，利用这个特性，服务端不再返回JSON格式的数据，而是返回一段调用某个函数的js代码，在src中进行了调用，这样实现了跨域。

2）Jsonp就是来解决跨域的问题的。

3）跨域是因为有同源策略的影响，同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。所以a.com下的js脚本采用ajax读取b.com里面的文件数据是会报错的。

4）Jsonp因为是利用src这个属性来进行跨域的，src只支持get请求。所以也导致了jsonp缺点出现，只支持get请求不支持post请求。

## **原型**

函数有原型，函数有一个属性叫prototype，函数的这个原型指向一个对象，这个对象叫原型对象。这个原型对象有一个constructor属性，指向这个函数本身。

## **原型链**

任何一个对象，都有原型对象，原型对象本身又是一个对象，所以原型对象也有自己的原型对象，这样一环扣一环就形成了一个链式结构，我们把这个链式结构称为：原型链

## **null和underfine的区别**

null是一个表示"无"的对象，转为数值时为0；undefined是一个表示"无"的原始值，转为数值时为NaN

**null****表示没有对象，即该处不应该有值。**典型用法是：

```
（1） 作为函数的参数，表示该函数的参数不是对象。
（2） 作为对象原型链的终点。 Object.getPrototypeOf(Object.prototype) // null 
 (3)  设置为null的变量或者对象会被内存收集器回收 例如闭包的的释放
```

**undefined****表示缺少值，就是此处应该有一个值，但是还没有定义。**典型用法是：

```
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。
```

## **数组扁平化概念**

数组扁平化是指将一个多维数组变为一维数组

[1, [2, 3, [4, 5]]] ------> [1, 2, 3, 4, 5]

**扁平化方法**

- arr.toString().split(',').map(处理每一项转换成数字)

- arr.join(',').split(',').map(处理每一项转换成数字)

- ```
  while (arr.some(item => Array.isArray(item))) {
       arr = [].concat(...arr);
     }
  ```

## 同步和异步

- 同步是主线程代码一步一步的执行，前面的执行完才会执行后面的代码
- 异步是不进入主线程，进入任务队列，等待主线程执行完后，才会把队列代码拿出来执行

## **页面传参有没有出现过乱码的情况？如果有你是怎么解决的？**

一般都是出现在url进行中文传参的时候会出现乱码的情况。

解决的方式：

1）可以使用H5的本地存储或者是cookie来进行传递。

2）在url传参的时候用encodeURL()进行编码一下中文参数传递到下个页面

​    然后用decodeURL()进行解码一下就可以了。

## **在前后端数据交互的时候，有处理过（参数）数据加密的情况吗**

这个数据加密的问题，一般金融或者支付公司会问这样的问题，这类公司对数据的保密性要求的很严的。这个样的问题就看你去面试什么的公司的，如果不会也可以说没有做过，如果你能记住一些方式最好能说一下。

```
引入Base64文件
例如：
```

```
var obj = {
     name:"lisi",
     age:18,
     getBase:function(){}
 };
 var str = JSON.stringify(obj);
 var base64 = new Base64();
 var baseStr = base64.encode(str);//加密
 var deBaseStr = base64.decode(baseStr);//解密
```

## base64是什么

Base64是网络上最常见的用于传输8Bit字节码的编码方式之一，Base64就是一种基于64个可打印字符来表示二进制数据的方法”

作用：对隐私数据加密。对小图片进行base64转换打包，省去不必要的资源请求。

## Json字符串和json对象怎么相互转换

JSON.parse()   **从一个字符串中解析出json对象**

JSON.stringify()  **从一个对象中解析出字符串**

## **http**的常用的状态码遇到过那些？分别代表什么意思？

1)、1xx(临时响应)表示临时响应并需要请求者继续执行操作的状态代码。

　　100 (继续) 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。

2)、2xx (成功)表示成功处理了请求的状态代码。

　　200 (成功) 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。

3)、3xx (重定向) 表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。

　　300 (多种选择) 针对请求，服务器可执行多种操作。 服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。

4)、4xx(请求错误) 这些状态代码表示请求可能出错，妨碍了服务器的处理。

　　400 (错误请求) 服务器不理解请求的语法。

404 (未找到) 服务器找不到请求的网页。

5)、5xx(服务器错误)这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。

500 (服务器内部错误) 服务器遇到错误，无法完成请求。

## Jq和zepto的区别是什么？他们一般的使用场景？

**同：**Zepto最初是为移动端开发的库，是jQuery的轻量级替代品，因为它的API和jQuery相似，而文件更小。Zepto最大的优势是它的文件大小，只有8k多，是目前功能完备的库中最小的一个，尽管不大，Zepto所提供的工具足以满足开发程序的需要。

**异：**针对移动端程序，Zepto有一些基本的触摸事件可以用来做触摸屏交互（tap事件、swipe事件），Zepto是不支持IE浏览器的。这个也是为什么zepto小的重要原因。

**场景：**Zepto更适合移动端开发，jq更适合pc端开发

## **$(this)和this的区别是什么**

$(this)是jquery对象，能调用jquery的方法，例如click(), keyup()等等；而this,则是[html元素](https://www.baidu.com/s?wd=html%E5%85%83%E7%B4%A0&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)对象，能调用元素属性，例如this.id,this.value等等。

## Jq对象和DOM对象是怎么相互转化的？

在jq中，只需要调用[index]和get(index)方法即可将jq对象转换为DOM对象。DOM对象只需调用jq的$()方法即可包装为jq对象

## **你了解jq的链式编程吗？知道如何实现的吗**

1）链式操作就是分步骤地对jQuery对象实现各种操作。例如：                    $("#Test").css('color','red').show(200).removeClass('style')

2）这样做的好处就是能节省代码量，提高代码的效率，代码看起来更优雅。

3）原理就是在每个方法的最后：return this来实现的

## **Jq.extend和Jq.fn.extend是用来做什么的？有什么区别？**

这问题主要是考察你对jq和js高级的一些理解，能考察出你的基础如何，面试官也是非常喜欢问的。Jq之所谓强大不仅仅是提高了很多方法，还提供了两个扩充插件的两个方法，就是这个两个方法。

1）jq.extend,它是为jQuery类添加类方法，可以理解为添加静态方法。调用不同：例如：$.ajax

2）jq.fn.extend,是对jQuery.prototype进得扩展，就是为jQuery类添加“成员函数”。jQuery类的实例可以使用这个“成员函数”，也可以叫实例方法。调用不同：例如：$(body).css()

（扩充理解：jQuery.fn.extend=jQuery.prototype.extend）

## Jq的each循环是如何跳出的

​    Return  false；可以跳出

## jq中ajax的有哪些参数

```
（1）type：HTTP 请求方法，默认为GET
（2）url：发送请求的地址
（3）username：响应 HTTP 访问认证请求的用户名
（4）xhr：返回一个 XMLHttpRequest 对象
（5）options：ajax 请求设置对所有的选项都是可选的
（6）async：判断请求是异步请求或是同步请求，如果为true，则为异步请求；为false则为同步请求（默认情况下为true）
（7）beforeSend(XHR):发生前可修改XMLHttpRequest 对象的函数，如果返回的是false则取消ajax请求。
（8）cache：为默认值true时缓存页面，为false时将不缓存此页面
（9）contentType：指发生信息至服务器时的内容编码，默认值为"application/x-www-form-urlencoded"
（10）context：设置 Ajax 相关回调函数的上下文，让回调函数内的this指向这个对象，如果不设定则this指向options
（11）data：指发送到服务器上的数据，自动转换为请求字符串格式。
（12）dataType：服务器返回的数据类型
（13）global：表示是否触发全局ajax事件，true时为出发，如果设置false将不会触发。
（14）jsonp：在请求中重写回调函数名字，用来代替GET或许POST请求中URL的参数“callback”
（15）jsonpCallback：请求指定一个回调函数名。代 替jQuery 自动生成的随机函数名。
（16）password：响应 HTTP 访问认证请求的密码
（17）success：请求成功后的回调函数
（18）timeout：设置请求超时的时间
```

## Jq的源码有看过吗？如果看过是怎么理解jq源码的？

这个如果能说出来是很加分的，如果不知道就算了，因为有很多问题还是不好理解的。但是更希望多少大家都能了解一些，如果面试官问到了可以聊上几句。有能力理解的同学，自己可以到网上去查一下好的博客，看别人写的源码分析。

1）[jQuery](http://lib.csdn.net/base/jquery)源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入window对象参数，可以使window对象作为局部变量使用，好处是当[jQuery](http://lib.csdn.net/base/jquery)中访问window对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。 window.jQuery = window.$ = jQuery

2） [jquery](http://lib.csdn.net/base/jquery)将一些原型属性和方法封装在了jquery.prototype中，为了缩短名称，又赋值给了jquery.fn，这是很形象的写法。

3）jquery实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率，这个也是所说的链式编程。

## art-template的使用步骤

+ 使用 引用简洁语法的引擎版本，例如： <script src="dist/template.js"></script>  
+ 表达式：{{ 与 }} 符号包裹起来的语句则为模板的逻辑表达式。
+ 输出表达式：对内容编码输出： {{content}} 、不编码输出： {{#content}} 编码可以防止数据中含有 HTML 字符串，避免引起 XSS 攻击。、
+ 可以使用条件表达式{{if xx}} 、 遍历表达式 {{each list as v i}} 。。。{{/each}}、
+ 模板数据相结合  var html = template（'test',data）
+ 渲染到dom中  document.getElementById('content').innerHTML = html; 

## prop和attr的区别

只要有 Boolean() 属性的,简单说就是具有true 和 false 两个属性的属性，如 checked, selected 或者 disabled 使用prop()，(其实这些都是表单类的), 其他的使用 attr()

## 单线程和多线程

多线程是指程序中包含多个执行流，即在一个程序中可以同时运行多个不同的线程来执行不同的任务，也就是说允许单个程序创建多个并行执行的线程来完成各自的任务。

单线程的也就是程序执行时，所跑的程序路径（处理的东西）是连续顺序下来的，必须前面的处理好，后面的才会执行到。

## Bootstrap有使用过吗？为什么使用？

bootstrap主要是做响应式开发，而且有很丰富的UI组件可以拿来用，通过class就可以操作样式，非常方便，容易上手

## Bootstrap栅格系统是什么？工作原理是什么？

Bootstrap内置了一套响应式、移动设备优先的流式栅格系统，随着屏幕设备或视口（viewport）尺寸的增加，系统会自动分为最多12列。

网格系统的实现原理非常简单，仅仅是通过定义容器大小，平分12份(也有平分成24份或32份，但12份是最常见的)，再调整内外边距，最后结合媒体查询，就制作出了强大的响应式网格系统。Bootstrap框架中的网格系统就是将容器平分成12份。

## Bootstrap中设置分页的class？

下面的这些题目都是具体的知识点，主要是考察你是否用过这个框架。

1）默认的分页：class="pagination"

2）默认的翻页：class="pager"

## **Bootstrap如何设置响应式表格**

后台管理系统里面做的表格比较多，所以这个问题问到的比较多。

1）增加class="table-responsive"

## **使用Bootstrap激活或禁用按钮要如何操作**

1）激活按钮：给按钮增加.active的class

2）禁用按钮：给按钮增加disabled="disabled"的属性

## **Bootstrap**中有关元素浮动及清除浮动的class？

**答：**

1）class="pull-left" 元素浮动到左边

2）class="pull-right" 元素浮动到右边

3）class="clearfix" 清除浮动

## **对于各类尺寸的设备，Bootstrap设置的class前缀分别是什么**

这个是做响应式布局的必须要知道的内容，所以大家要记住。

1. 超小设备手机（<768px）：.col-xs- 

2. 小型设备平板电脑（>=768px）：.col-sm-

3. 中型设备台式电脑（>=992px）：.col-md-

4. 大型设备台式电脑（>=1200px）：.col-lg-

   

## 响应式适应的终端设备有哪几个

基本上都能设置 一般来讲适用主流有下面几个

1. 超小设备手机（<768px）不是太建议用响应式来适应超小设备
2. 小型设备平板电脑（>=768px）
3. 中型设备台式电脑（>=992px）
4. 大型设备台式电脑（>=1200px）

## 响应式为什么不适用移动端的开发

第一：**响应式网站不能自由布局，做不到移动端的优化体验。**响应式网页不能自由布局，文字或图片元素只能删减，不能增加，图文只能更新，无法拖动位置，图片无法放大或者缩小，也就是说所有的元素都是被锁死的。

**第二：响应式设计对搜索关键词优化和排名不友好。**一套代码的响应式网站增添了搜索引擎的处理障碍

**第三：响应式网站移动端浪费带宽，加载耗时长。**图片过大，在移动端就要消耗更多的流量加载

**第四**：**响应式对于ie6,7,8浏览器的兼容效果极差**。

## 消息推送能

<https://88250.b3log.org/web-message-push>

消息推送是针对 Web 应用开发领域的技术，指服务端以主动方式将信息送达客户端。主要用于提升用户体验，避免用户刷新页面从服务端拉取数据。例如 Web 邮件中自动出现刚收到的邮件项，Web 即时通讯自动提示新到消息等应用场景。

消息推送的方式有2种：

1. web层推送消息(前端实现推送消息)
2. 服务层推送消息(后端实现)

web层实现消息推送的方式：

**一**.**套接字**：可以使用套接字接口进行全双工通讯。可以通过 Flash XMLSocket、Java Applet 技术实现。缺点是限制太大（实现方案与厂商技术绑定过紧）

**二**.**HTTP请求轮询**：基于 HTTP 协议进行“推送”实现，可由客户端发起 HTTP 请求轮询，服务端在请求后返回响应。根据轮询时间、请求处理方式，分为以下三种推。

- **简单轮询**：客户端一般以定时方式发起请求，服务端处理后返回响应，

- **长轮询**：客户端发起请求后服务端将该请求挂起（不返回响应），直到超时、异常或需要处理响应（推内容）才返回。客户端收到响应后再次请求（即轮询）服务端，并处理响应。

- **HTTP流**：客户端发起请求后服务器端处理请求，并通过 HTTP 流一直向客户端写入数据，直到超时或异常才返回响应。连接断开后客户端再次请求服务端，属于长轮询的一种。

  

**三**.**HTML 5 WebSocket**:     HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

<https://www.runoob.com/html/html5-websocket.html>

## **内置js函数有哪些**

1. alert函数：弹出对话框
2. confirm函数 弹出对话框
3. escape函数  将字符串编码，以便在所有计算机上都可读取该字符串
4. unescape函数   解码字符串
5. eval函数
6. isNaN(x)函数
7. parseFloat()函数
8. parseInt(str,x)函数
9. prompt()函数

## **构造函数可以返回那些值**

在传统语言中，构造函数不应该有返回值，实际执行的返回值就是此构造函数的实例化对象。

而在js中构造函数可以有返回值也可以没有。

1. 没有返回值则按照其他语言一样返回实例化对象。
2. 若有返回值则检查其返回值是否为引用类型。如果是非引用类型，如基本类型（string,number,boolean,null,undefined）则与无返回值相同，实际返回其实例化对象。
3. 若返回值是引用类型，则实际返回值为这个引用类型。

## node和浏览器有啥区别

一、全局环境下this的指向：在node中this指向global而在浏览器中this指向window

二、js引擎： 浏览器中不同的浏览器厂商提供了不同的浏览器内核。nodejs是基于GoogleV8引擎，V8引 擎执行Javascript的速度非常快，性能非常好

三、DOM操作：js可以在浏览器中进行DOM操作，node只是有运行js的环境。

四、I/O读写  node提供了比较方便的组件，浏览器需要考虑兼容，在本地打开一个图片过程很麻烦

五、模块加载  node中提供了CMD模块加载，也提供了npm，yarn包管理工具。至于浏览器。

## valueOf和toString的区别

**一、toString（）**

作用：toString（）方法返回一个表示改对象的字符串，如果是对象会返回，toString() 返回 “[object type]”,其中type是对象类型。

**二、valueOf( )**

**作用：valueOf返回指定对象的原始值，JS会利用 valueOf() 方法用来把对象转换成原始类型的值（数值、字符串和布尔值） 

## flex布局的属性

![](img\Snipaste_2019-07-29_23-09-47.png)

## normalize，rest和base的区别

**reset：重置，清零**      代表：  * {padding:0; margin:0;}

把所有标签全都扒光。`body`、`h1-h6`、`div`、`article`等一大推本来各不相同的标签，现在长的一模一样了，仅剩名字作为最后的尊严

**normalize：归一化，使正常化、标准化**

normalize做了几件事情：

- 保留浏览器默认样式中有用的部分
- 同时保证跨浏览器一致性（修复各浏览器中存在的不合W3C规范的问题）
- 在合适的地方设置默认值

normalize是*补丁*性质的，不像reset是破坏性的

**base**综合normalize和rest的一些样式，在添加一些其他的样式参产生的。

## ["1","2","3"].map(parseInt)?会得到什么

答案是：[1, NaN, NaN].

原因：主要是下面这3点

- map函数传递参数的定义
- parseInt函数针对于radix这个参数的理解
- 二进制当中没有"3"这个数码

```js
// ['1','2','3'].map(parseInt); 等于如下
 ['1','2','3'].map(function(item,index,array){     
     return parseInt(item,index); // 是不是一目了然
 });
  // parseInt("1",0); => 1
  // parseInt("2",1); => NaN 
  // parseInt("3",2); => NaN 
```

拓展：**parseInt(string, radix);**

string: 需要转化的字符，如果不是字符串会被转换，忽视空格符。

radix：数字2-36之前的整型。默认使用10，表示十进制。这个参数的意义是指把前面的字符看作是多少进制的数字，所谓的基数。

**分析**

```
parseInt('1', 0); // 1 (parseInt的处理方式，这个地方item没有以"0x"或者"0X"开始，8和10这个基数由实现环境来定，ES5规定使用10来作为基数，因此这个0相当于传递了10)
parseInt('2', 1); // NaN (因为parseInt的定义，超出了radix的界限)
parseInt('3', 2); // NaN (虽然没有超出界限，但是二进制里面没有3，因此返回NaN)
```

## 表单序列化

一、param() 方法创建**数组或对象的序列化**表示。

　　该序列化值可在进行 AJAX 请求时在 URL 查询字符串中使用。

语法：jQuery.param(*object*,*traditional*)
　　object要进行序列化的数组或对象
　　traditional规定是否使用传统的方式浅层进行序列化（参数序列化）。

二、serialize() 方法通过**序列化表单值**，创建 URL 编码文本字符串

　　您可以选择一个或多个表单元素，或者 form 元素本身。序列化的值可在生成 AJAX 请求时用于 URL 查询字符串中。

　　.serialize() 方法创建以标准 URL 编码表示的文本字符串。它的操作对象是代表表单元素集合的 jQuery 对象。

　　注意：1、只会将“**成功的控件**”序列化为字符串。（**即不能被禁用**）

　　　      2、如果不使用按钮来提交表单，则**不对提交按钮的值序列化**。

　　　　　3、如果要表单元素的值包含到序列字符串中，元素**必须使用 name 属性**。

三、serializeArray() 方法通过**序列化表单值来创建对象数组（名称和值）**

您可以选择一个或多个表单元素，或者 form 元素本身。语法：$(*selector*).serializeArray()。注意：此**方法返回的是 JSON 对象而非 JSON 字符串**

**注意点**元素不能被禁用（禁用的元素不会被包括在内），并且元素应当有含有 name 属性。提交按钮的值也不会被序列化。文件选择元素的数据也不会被序列化。

**总结：**

**1、param()，数组或对象序列化表示**

**2、serialize()，序列化表单（成功元素、必须有name属性）**

**3、serializeArray()，序列化表单来创建对象数组，返回JSON对象**

## 如何获取到用户用什么浏览器或使用pc还是移动端访问页面的

判断html 是在移动端应用（app）,还是移动端[浏览器](https://www.baidu.com/s?wd=%E6%B5%8F%E8%A7%88%E5%99%A8&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)中打开可以通过查看 UA来实现。

1. UA是SIP协议中的一个逻辑实体,SIP是一个信令协议,代理的含义为代替用户处理信令协议,简单说就是替用户收发信令信息。

2. UA简单是指用户的手机信息。通过UA，可以知道用户的手机类型，是小米的，还是索爱的。

3. Header 里面有 UA，可以根据  UA 来判断。

   

4. 约定App中的网页的UA都加入了某一标识，例如，从微信中打开的网页的UA都会含有Wechat/1.0.1之类的信息。

5. app 中打开可以在 Header 里面加入一些字段，这样就可以在页面中判断，有这些字段就是在 app 里面打开的，没有就不是。

6. 如果是app扫描打开，扫描传入的是网址，最后打开的还是在[浏览器](https://www.baidu.com/s?wd=%E6%B5%8F%E8%A7%88%E5%99%A8&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)打开，跟app是没有关联的，所以还是通过[浏览器](https://www.baidu.com/s?wd=%E6%B5%8F%E8%A7%88%E5%99%A8&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)打开的。

## node的优缺点

**优点：**1. 高并发（最重要的优点）

2. 适合I/O密集型应用

**缺点：**1. 不适合CPU密集型应用；CPU密集型应用给Node带来的挑战主要是：由于JavaScript单线程的原因，如果有长时间运行的计算（比如大循环），将会导致CPU时间片不能释放，使得后续I/O无法发起；

2. 只支持单核CPU，不能充分利用CPU

3. 可靠性低，一旦代码某个环节崩溃，整个系统都崩溃。原因：单进程，单线程

## 三大家族

<https://juejin.im/post/5c7ce2ef6fb9a049b82b3076>

## window.onload

onload 事件会在页面或图像加载完成后立即发生。onload 通常用于 <body> 元素，在页面完全载入后(包括图片、css文件等等。)执行脚本代码

在 HTML 中:

<body onload="*SomeJavaScriptCode*">

在 JavaScript 中:

window.onload=function(){*SomeJavaScriptCode*};



HTML+CSS部分：

## **1.列举HTML5和CSS3中的新特性**

h5

- 拖拽释放  

  ​       1. 给元素添加draggable="true可以进行拖拽，图片，链接默认开启的  

  ​	2. 事件： οndragstart 开始拖拽  、οndrag拖拽中、  οndragend拖拽结束、  οndragenter拖拽入目标元素、dragleave离开目标元素

- 自定义属性 data-id

- h5 新增的语义标签  header、nav、footer、aside、section、article

- 音频、视频（audio,video）

- 画布 canvas Api

- 地理 Geolocation APi   

  ```
  // 获取地理位置
  navigator.geolocation.getCurrentPosition (successCallback , errorCallback [ , options ] )
  ```

  ```
  //  监听位置
  watchPosition ( successCallback , errorCallback )
  ```

  ```
  // 清除监听
  clearWatch ( )
  ```

  

- 本地离线存储 localStorage 长期存储

- sessionStorage

- 表单控件 date time email url search。。。

- 表单属性 placeholder、autofocus、autocomplete、required

- 新的技术 

  ​	webwork    用于组件化和代码重用的J2EE Web框架

  ​	websocket 基于TCP的全双工通信协议

  ```
  // 初始化一个 WebSocket 对象
  var ws = new WebSocket('ws://localhost:9998/echo');  // 参数指定一个连接服务端的地址
  // 建立 web socket 连接成功触发事件
  ws.onopen = function() {
  // 使用 send() 方法发送数据
  ws.send('发送数据');
  alert('数据发送中...');
  };
  // 接收服务端数据时触发事件
  ws.onmessage = function(evt) {
  var received_msg = evt.data;
  alert('数据已接收...');
  };
  // 断开 web socket 连接成功触发事件
  ws.onclose = function() {
  alert('连接已关闭...');
  };
  ```

  C3新特性：

- 颜色 rgba

- 文字阴影(text-shadow)

- 边框 border-radius  边框圆角    

  - 阴影： box-shadow  

    ```
    语法：
    1.box-shadow:x y blur color;  
    2.box-shadow:[inset] x y blur [范围] color; 阴影可以叠加
    3.box-shadow:x y b c,x y b c,x y b c……; 边框图片 border-image
    ```

- 盒子模型 box-sizing

- 背景尺寸 background-size 背景图片  背景原点 background-origin 

- 只需要记住一和二，其他了解

  ```
  1.线性渐变 linear-gradient(渐变角度/渐变开始的位置，red 颜色开始渐变的位置，green 颜色开始渐变的位置);
  
  2.径向渐变 radial-gradient(渐变角度/渐变开始的位置，red 颜色开始渐变的位置，green 颜色开始渐变的位置);
  
  3.重复渐变：repeating-linear/gradial-gradient(渐变角度/渐变开始的位置，写一个循环的渐变);
  
  4.跳变linear-gradient(red
  25%,green 25%,green 50%,yellow 50%,yellow 75%,deepskyblue 75%); 
  ```

  

- 过渡 transition

- 自定义动画 animate @keyfrom

  动画八大属性  

  ![](img\Snipaste_2019-07-27_21-56-30.png)

- 媒体查询  @media screen add (width:980px){..}

- 2d转换 transform   属性tanslate平移  rotate旋转 skew斜切 scale缩放

- 3d转换

- 弹性布局 

- 文字缩略

  ```
  text-overfow:ellipsis; —->参数还可以是clip 
  wite-space:nowarp; 
  overflow:hidden
  ```

  

## **2.如何让一个元素垂直水平居中**

**第一种方法:**

<!--把元素变成定位元素-->

```
position:absolute;
<!--设置元素的定位位置，距离上、左都为50%-->
left:50%;
top:50%;
<!--设置元素的左外边距、上外边距为宽高的负1/2-->
margin-left:-100px;
margin-top:-200px;
}
```

*兼容性好;缺点:必须知道元素的宽高

\-------------

**第二种方法：**

```
div.box{
weight:200px;
height:400px;
<!--把元素变成定位元素-->
position:absolute;
<!--设置元素的定位位置，距离上、左都为50%-->
left:50%;
top:50%;
<!--设置元素的相对于自身的偏移度为负50%(也就是元素自身尺寸的一半)-->
transform:translate(-50%,-50%);
}
这是css3里的样式;缺点:兼容性不好，只支持IE9+的浏览器-
```

**第三种方法**

```
div.box{
width:200px;
height:400px;
<!--把元素变成定位元素-->
position:absolute;
<!--设置元素的定位位置，距离上、下、左、右都为0-->
left:0;
right:0;
top:0;
bottom:0;
<!--设置元素的margin样式值为 auto-->
margin:auto;
}*
兼容性较好，缺点:不支持IE7以下的浏览器

```

**第四种方法**

```
.box{
 display: flex;
 justify-content: center; //水平居中
 align-items: center;	  // 垂直居中
}

```

**第五种方法**

```
.parent {
display: table;
}
.son {
display: table-cell;
vertical-align: middle;
}

```



### 3.iframe 的优缺点

**iframe也称作嵌入式框架，嵌入式框架和框架网页类似，它可以把一个网页的框架和内容嵌入在现有的网页中**

**iframe的缺点**
1、页面样式调试麻烦，出现多个滚动条；
2、浏览器的后退按钮失效；

3、过多会增加服务器的HTTP请求；

4、小型的移动设备无法完全显示框架；
5、产生多个页面，不易管理；
6、不容易打印；
7、代码复杂，无法被一些搜索引擎解读。

**iframe的优点**：
1.iframe能够原封不动的把嵌入的网页展现出来。

2.如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷。

3.网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用iframe来嵌套，可以增加代码的可重用。

4.如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由iframe来解决。

5.重载页面时不需要重载整个页面，只需要重载页面中的一个框架页(减少了数据的传输，增加了网页下载速度)

# JS/ES6部分：

## 1.javascript中共有哪几种基本数据类型？

数字类型 number，字符串 string，布尔类型  Boolean，undefined 、 null 、 symbol（es6新加）

## 2.get和post请求的区别

get: 请求参数拼接在地址栏中，没有请求体，传输的数据大小有限制，速度快，数据会暴露，不安全

post：请求在请求体中，请求在请求体中，可传输数据量大，所以缓慢，相对安全。

## 3.请描述JS里面作用域、闭包以及匿名函数的含义和作用

作用域：**含义**：作用域分为全局作用域和局部作用域。函数以外的区域就叫全局作用域，函数内部的区域叫做局部作用域。**作用**：函数内部可以访问外部变量，外部无法访问内部变量。

闭包：**含义**：函数内部嵌套一个函数，内部函数可以访问外部函数的变量，叫做闭包。**作用**：保护变量私有化，避免变量被修改。同时避免全局变量污染。缺点：闭包会导致内存泄漏

匿名函数：没有名称的函数，例如 (fn())() ,匿名函数可以自调用，也叫沙箱模式，一般大型项目开发，或者项目模块相分离使用，这样避免了模块之间变量的相互影响。从而避免了变量污染。正因为如此所以外部也无法访问这个模块内的变量。如果需要内部变量还需要把变量暴露出去

## 4.ajax的作用是什么？请写出ajax的基本结构（或者是ajax的步骤）

Ajax的**原理简单来说通过XmlHttpRequest对象来向服务器发异步请求**，从服务器获得数据，然后用javascript来操作DOM而更新页面

Ajax实现了客户端与服务器进行数据交流过程。使用技术的好处是：不用页面刷新，并且在等待页面传输数据的同时可以进行其他操作。

```
(1)创建`XMLHttpRequest`对象,也就是创建一个异步调用对象；
(2)创建一个新的`HTTP`请求,并指定该`HTTP`请求的方式、`URL`及验证信息
(3)设置响应`HTTP`请求状态变化的函数；
(4)发送`HTTP`请求；
(5)获取异步调用返回的数据
(6)使用JavaScript和DOM实现局部刷新。

```

## ajax的优点和缺点

**优点**：

```
1.减轻服务器的负担，提升站点性能
2.无刷新更新页面，减少用户实际等待和心理等待的时间
3.更好的用户体验
4.ajax是基于标注化并被广泛支持的技术，并且不需要插件和下载小程序（需要在js浏览器上执行）
5.界面与应用分离（数据与呈现相分离）
6.异步与服务器通信,不打断用户的操作

```

**缺点**：

- ajax干掉了back和history功能，就是后退键无法使用
- 安全不好，暴露了数据交互细节
- 对搜索引擎支持比较弱
- ajax不是很好的支持移动设备
- 违背URL与资源定位的初衷
- 破坏程序的异常处理机制
- 客户端肥大，太多客户段代码造成开发上的成本

## 5.let、const、var的区别和作用？

- let和const都是块级作用域，如果是函数，那么一个{}就是一个块，声明的变量名不能重复。
- let声明的变量是可以修改的，与var用法类似，区别是没有变量的提升。
- const声明的变量又叫常量，后面无法更改的，在声明时必须同时赋值。
- var 变量 可以变化得量，会有作用域，有变量提升，可以重复声明，全局变量会挂载到window上。

## 6.Class(ES6)继承和Prototype继承的区别是什么？

ES5继承：prototype

1.通过原型链实现继承。子类的prototype为父类对象的一个实例，因此子类的原型对象包含指向父类的原型对象的指针，父类的实例属性成为子类原型属性

2.ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)）

ES6继承：

1.子类没有自己的this对象，因此必须在construct中通过super继承父类的this对象，而后对此this对象进行加工。super关键字在构造函数中表示父类的构造函数，用来新建父类的this对象。

2.ES6 的继承机制完全不同，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。

3.super可作为函数和对象使用。当作为函数使用时，只可在子类的构造函数中使用，表示父类的构造函数，但是super中的this指向的是子类的实例，因此在子类中super()表示的是Parent.prototype.constructor.call(this)。当作为对象使用时，super表示父类原型对象，即Parent.prototype

## 7.localStorage、sessionStroage、cookie的区别

1. cookie  早期使用的 内存只有4k ，每次请求会自动携带cookie数据,可以有周期的，例如7天会员；
2. sessionStorage  大小5M   特点：会话级别(关闭浏览器就自动销毁)   不能跨窗口共享
3. localStorage 大小5M 甚至更大，特点：永久存储只要用户不主动清除缓存  可以多个窗口共享

## 8.写一个函数，判断一个字符串是不是回文字符串("回文串"从左往右和从右往左读取出来的结果是一致的，例如"level"就是回文串)

```
var str1 = "level"
  function isFn(str) { 
      console.log(str.split('').reverse().join('') === str)
    }
  isFn(str1)

```

## 9.eval()的作用和使用场景有哪里？

eval的使用：

- 执行字符串代码
- 将json字符串转换成JS对象

注意：eval函数的功能非常的强大，但是实际使用的情况并不多。

- eval形式的代码难以阅读
- 字符串内的变量解析后会变成全局变量
- eval形式的代码无法打断点，因为本质还是还是一个字符串
- 在浏览器端执行任意的 JavaScript会带来潜在的安全风险，恶意的JavaScript代码可能会破坏应用

## 10.谈谈垃圾回收机制及内存管理

### js的垃圾回收机制

- 内存：计算机中所有程序的运行都是在`内存`中进行的，因此内存的性能对计算机的影响非常大，运行程序需要消耗内存，当程序结束时，内存会得到释放。
- javascript分配内存：当我们定义变量，javascript需要分配内存存储数据。无论是值类型或者是引用类型，都需要存储在内存中。
- 垃圾回收：当代码执行结束，分配的内存已经不需要了，这时候需要将内存进行回收，在javascript语言中，`垃圾回收机器`会帮我们回收`不再需要使用`的内存。

垃圾回收的2种方式：

计算清除法：查看内存是否有被变量引用，如果没有就回收。这种方式有bug，内部循环引用时，内存不会释放。

标记清除法：浏览器定期的从全局对象window(也叫根)开始，找所以从根开始引用的对象，然后找这些对象引用的对象。从根开始，垃圾回收器回找到说有可以获得的对象和不能获得得对象。

## 11.描述一次完整的http请求过程（即从浏览器地址栏输入URL地址后到页面加载完成之间的流程）

- 浏览器解析url地址
- DNS域名解析
- 发起http请求
- 服务器处理请求
- 浏览器解析服务器响应
- 根据结构样式和js进行渲染页面

12.请写出下面的输出结果

```javascript
function fun(n,o){
  console.log(o)
  return {
    fun:function(m){
      return fun(m,n)
    }
  }
}
var a = fun(0); a.fun(1); a.fun(2); a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1); c.fun(2); c.fun(3)

setTimeout(function(){
  console.log(1)
},0)
new Promise(function executor(resolve){
  console.log(2)
  for(var i = 0; i < 10000; i++){
    i==999&& resolve()
  }
  console.log(3)
}).then(function(){
  console.log(4)
});
console.log(5)

```

## 12.内存泄漏的几种方式,解决办法

1.全局变量     // 少用

2.闭包   //用完断指引 改为null

3 定时器未清理  //定时器不用时，清除定时器

4.没有清理的dom元素引用   =>简单来说就是当元素已经从dom树中移除，但仍然有元素在引用它



## 1.写出至少4种vue中的指令和它的用法？其中v-el的作用？

v-for 循环创建元素，并使用数据 ,需要配合 :key使用  如  <li v-for='item in  list' ：key='item.id'></li>

v-bind   {{}}语法不能直接作用于标签属性中。v-bind就是为了动态绑定属性 例如 v-bind:title="list.msg" 一般用于在数据中动态匹配的时候使用。  

v-on 用于给元素注册事件，v-on：事件名='事件函数' 可以简写为@事件名="事件函数"   可以添加事件修饰符

v-model  用于表单元素双向数据绑定。在data中必须存在这个数据

答：v-if：判断是否隐藏；v-for：数据循环出来；v-bind:class：绑定一个属性；v-model：实现双向绑定

​	v-el 作用：通过`v-el`我们可以获取到`DOM`对象。

​	v-ref     作用：通过`v-ref`获取到整个组件（`component`）的对象。

## 2.vuex有哪几种属性？它的使用场景里有哪些？

VueX 是一个专门为 Vue.js 应用设计的状态管理架构，统一管理和维护各个vue组件的可变化状态(你可以理解成 vue 组件里的某些 data )。

Vue有五个核心概念，state, getters, mutations, actions, modules。

state => 基本数据 
 getters => 从基本数据派生的数据 
 mutations => 提交更改数据的方法，同步！ 
 actions => 像一个装饰器，包裹mutations，使之可以异步。 
 modules => 模块化Vuex

单页应用中，组件之间的状态。应用实例：音乐播放、登录状态、加入购物车等等

## 3.vue-router是什么，它有哪些组件？

是[Vue.js](http://vuejs.org/)的官方路由器

功能：

- 嵌套路由/视图印射
- 模块化，基于组件的路由配置
- 导航控制

组件：   <router-link></router-link>

​		<router-view></router-view>

## 5.什么是双向数据绑定以及单向数据流，它们的不同点是什么？

双向数据绑定是当数据变化时，视图也跟着改变，当视图发生改变时，数据也跟着改变

单向数据流：父组件的数据向子组件传递。如果子组件要修改父组件的数据，必须把数据传给父组件，让父组件自己修改。

不同点：双向数据绑定是允许视图和数据双向更改的，一般作用于单个组件之。但是单向数据流是子组件和父组件的数据交互方式

## 7.vue.cli项目中src目录每个文件夹和文件的用法

assets文件夹是用来存放一些静态资源目录

components 文件件用来存放组件的

main.js是入口文件

route.js是用来处理路由相关的代码。

App.vue是根组件，一般用来设置所有组件的视图出口或者配置所有组件公共的样式

## 项目中有没有用过Eslint

ESLint 是一个ECMAScript/JavaScript语法规则和代码风格的检查工具，它的目标是保证代码的一致性和避免错误

## **堆和栈**

栈：基本的数据类型放在栈中，大小固定，自动分配内存空间，可以直接访问

堆：动态分配内存，大小不定，复杂类型存放在堆中，变量存的是指针(内存地址)，这个指针指向堆内存地址

## 函数节流与防抖

**函数防抖**，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。简单的说，当一个动作连续触发，则只执行最后一次。

**函数节流**：限制一个函数在一定时间内只能执行一次。

## async与promise的区别：

1. 在函数前有一个关键字async,await关键字只能在使用`async`定义的函数中使用。任何一个`async`函数都会隐式返回一个`promise`，并且promise resolve 的值就是 return 返回的值 
2. 不能在函数开头使用`await`
3. Async 函数的实现最简洁，最符合语义，几乎没有语义不相关的代码。
4. Promise 的写法比回调函数的写法大大改进，但是一眼看上去，代码完全都是 Promise 的 API（`then`、`catch`等等），操作本身的语义反而不容易看出来。
5. async配合promise语法让promise从异步变成了同步。await后的代码必须执行完了，才会进行下一步的操作。

## js中==和===的区别是什么？

==        只比较值，不比较类型

===　值和类型都进行比较　

===比较运算符的转换规则：

1. NaN不等于任何值,包括NaN本身
2. null不等于任何值，除了null和undefined
3. undefined不等于任何值，除了undefined和null
4. 如果操作符两边有数字或者布尔值，那么两边都转换成数字进行比较。 注意点：true:1  false:0  "":0  [ ]:0   {}:NaN
5. 如果以上都没再看是否有字符串，那么两边都转换成字符串比较。 注意： [ ]转字符串时 “”    对象时 object Object
6. 如果两边都是复杂类型，那么直接比地址（引用）。

## `<keep-alive></keep-alive>`的作用是什么

`<keep-alive></keep-alive>` 包裹动态组件时，会缓存不活动的组件实例,主要用于保留组件状态或避免重新渲染。 大白话: 比如有一个列表和一个详情，那么用户就会经常执行打开详情=>返回列表=>打开详情…这样的话列表和详情都是一个频率很高的页面，那么就可以对列表组件使用`<keep-alive></keep-alive>`进行缓存，这样用户每次返回列表的时候，都能从缓存中快速渲染，而不是重新渲染

## Vue 改变数组触发视图更新（数组更新检测）

> *以下方法调用会改变原始数组：push(), pop(), shift(), unshift(), splice(), sort(), reverse(),Vue.set( target, key, value )*

```
调用方法：Vue.set( target, key, value )
target：要更改的数据源(可以是对象或者数组)
key：要更改的具体数据
value ：重新赋的值

```

#### 如何优化SPA应用的首屏加载速度慢的问题？

> 1.将公用的JS库通过script标签外部引入，减小 app.bundel 的大小，让浏览器并行下载资源文件，提高下载速度
> *2.在配置 路由时，页面和组件使用懒加载的方式引入，进一步缩小 app.bundel 的体积，在调用某个组件时再加载对应的js文件；*
> 3.加一个首屏loading图，提升用户体验；

#### 页面刷新vuex被清空解决办法？

> 1. localStorage 存储到本地再回去
> 2. 重新获取接口获取数据
> 3. 使用vuex-along插件

#### vue如何兼容ie的问题。

> babel-polyfill插件

#### axios的特点有哪些？

> 1、axios是一个基于promise的HTTP库，支持promise的所有API；
> 2、它可以拦截请求和响应；
> 3、它可以转换请求数据和响应数据，并对响应回来的内容自动转换为json类型的数据；
> 4、它安全性更高，客户端支持防御XSRF；

## ajax的优缺点

#### $nextTick`是什么？

> vue实现响应式并不是数据发生变化后dom立即变化，而是按照一定的策略来进行dom更新。
> `$nextTick` 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM

#### 计算属性和watch的区别

> 在我们运用vue的时候一定少不了用计算属性computed和watch
> computed计算属性是用来声明式的描述一个值依赖了其它的值。当你在模板里把数据绑定到一个计算属性上时，Vue 会在其依赖的任何值导致该计算属性改变时更新 DOM。这个功能非常强大，它可以让你的代码更加声明式、数据驱动并且易于维护。
> watch监听的是你定义的变量,当你定义的变量的值发生变化时，调用对应的方法。就好在div写一个表达式name，data里写入num和lastname,firstname,在watch里当num的值发生变化时，就会调用num的方法，方法里面的形参对应的是num的新值和旧值，而计算属性computed,计算的是Name依赖的值,它不能计算在data中已经定义过的变量。

## vuex actions异步修改状态

actions和Mutations功能基本一样，不同点是，actions是异步的改变state状态，而Mutations是同步改变状态。

## 单页面应用应用优缺点

### 优势

- 传统的多页面应用程序，每次请求服务器返回的都是一整个完整的页面
- 单页面应用程序只有第一次会加载完整的页面
- 以后每次请求仅仅获取必要的数据，减少了请求体积，加快页面响应速度，降低了对服务器的压力
- SPA更好的用户体验，运行更加流畅

### 缺点

1. 开发成本高 (需要学习路由)
2. **不利于 SEO** 搜索引擎优化

## watch属性

handler：监听数组或对象的属性时用到的方法

deep：**深度监听，为了发现对象内部值的变化**，可以在选项参数中指定 deep:true 。**注意监听数组的变动不需要这么做。**

immediate: 在选项参数中指定 immediate: true 将**立即以表达式的当前值触发回调**，说白了，立即执行

tips: 只要bet中的属性发生变化（可被监测到的），便会执行handler函数；如果想监测具体的属性变化，如pokerHistory变化时，才执行handler函数，则可以利用计算属性computed做中间层

## 怎么理解MVVM？它和MVC有什么异同？（两个问题，逐一回答）

### MVVM

MVVM，一种更好的UI模式解决方案

- M：model数据模型(ajax获取到的数据)
- V：view视图（页面）
- VM：ViewModel 视图模型

### MVC vs MVVM

- MVC模式，将应用程序划分为三大部分，实现了职责分离，需要自己实现controller的代码，**需要操作DOM，单向通讯，view和Model都必须经过Controller来承上启下**

- MVVM通过`数据双向绑定`让数据自动地**双向同步**，**不需要操作DOM**

  - V（修改视图） -> M（数据自动同步）

  - M（修改数据） -> V（视图自动同步

    

**拓展：MVVM的优点**

**MVVM优点：**

MVVM模式和MVC模式类似，**主要目的是分离视图（View）和模型（Model）**，有几大优点：

1. **低耦合**，视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的”View”上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
2. **可重用性**，可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。
3. **独立开发**，开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xml代码。
4. **可测试**，界面向来是比较难于测试的，而现在测试可以针对ViewModel来写。

## MVVM设计思想适用于哪些场景？

单页面应用开发，表单元素的数据双向绑定

## vue的全家桶有哪些

vue的核心思想是：组件化和数据驱动

1. Vue-cli是快速构建这个单页应用的脚手架。
2. V-router路由，组件之间的切换。
3. Veux数据管理。
4. axios是一个http请求包，vue官网推荐使用axios进行http调用。

## vue的项目搭建

- node环境安装 
- 安装vue-cil脚手架工具
- 创建一个基于webpack模板的新项目
- 安装依赖
- 启动项目

## v-model是什么 ？ 怎么使用？ vue 标签怎么绑定事件？

v-model是vue中的指令，是用来给表单元素进行双向数据绑定的

**使用方式**：在对应的标签上添加v-model指令，例如 

```
<input type="text" v-model:title="msg">
```

  **注意点**

1. data属性中要有msg这个属性。
2. 会忽略表单元素中原有的value值
3. 如果使用v-model动态设置默认被选中，就不需要selected或者checked等属性了

给标签绑定事件要使用v-on指令 实例代码如下

```
<button v-on:click="add">按钮</button>
```

v-on指令可以直接缩写为@click='add'。

**注意点**： 必须在vm实例中的methods中提供add这个方法，然后再这个方法中进行事件处理。v-on指令还可以添加修饰符操作

## **Vue 的双向数据绑定原理是什么**？

数据劫持，`Object.defineProperty`中的`get`和`set`方法

- `getter`和`setter`：访问器
- 作用：指定`读取或设置`对象属性值的时候，执行的操作
- 访问数据时getter会返回数据，修改数据时setter会接收新值

## active-class是哪个组件属性？

​    active-class 属于vue-router的样式方法，当routerlink标签被点击时将会应用这个样式

使用有两种方法

1.routerLink标签内使用

```
<router-link to='/' active-class="active" exact>首页</router-link>
```

2.在路由js文件,配置active-class

```
<script>
     const router = new VueRouter({
	   routes,
	   linkActiveClass: 'active'
	   linkExactActiveClass: 'active'
      });
</script>
```



## 自定义指令的方法有哪些？

自定义指令方式有2种全局与局部自定义指令

全局的定义指令 Vue.directive('指令名',{指令的钩子函数})

局部定义指令：在vue实例中加入

```
directives: {
  focus: {
    inserted: function (el) {
      el.focus()
    }
  }
}
```

## 自定义指令钩子函数，以全局定义指令为例

```
Vue.directive('focus', {
    // 只会调用一次，当指令绑定到当前元素上时调用
    bind (el,binding) {
    },
    // 当前元素被插入到父节点的时候调用(渲染时)
    inserted (el,binding) {
        el.focus()
    },
    // 当指令对应的数据发生改变的时候
    update (el,binding) {
	},
	// 所有的DOM都更新之后
	componentUpdated (el,binding) {
	},
	// 指令与元素解绑的时候
	unbind (el,binding) {
	}
})
```

## 详细说明Vue的生命周期（不仅限于把钩子函数写出来）

Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、卸载等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。钩子函数 - beforeCreate()

- 说明：在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用
- 注意：此时，无法获取 data中的数据、methods中的方法

钩子函数 - **created()**

- 注意：这是一个常用的生命周期，可以调用methods中的方法、改变data中的数据

钩子函数 - beforeMounted()

- 说明：在挂载开始之前被调用，此时无法进行DOM操作

钩子函数 - **mounted()**

- 说明：此时，vue实例已经挂载到页面中，可以获取到el中的DOM元素，进行DOM操作

钩子函数 - beforeUpdated()

- 说明：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

钩子函数 - updated()

- 说明：组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。

钩子函数 - beforeDestroy()

- 说明：实例销毁之前调用。在这一步，实例仍然完全可用。
- 使用场景：实例销毁之前，执行清理任务，比如：清除定时器等

钩子函数 - destroyed()

- 说明：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

  

## **vue第一次加载的时候触发哪些钩子函数**

beforeCreate()

- 说明：在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用
- 注意：此时，无法获取 data中的数据、methods中的方法

钩子函数 - **created()**

- 注意：这是一个常用的生命周期，可以调用methods中的方法、改变data中的数据

钩子函数 - beforeMounted()

- 说明：在挂载开始之前被调用，此时无法进行DOM操作

钩子函数 - **mounted()**

- 说明：此时，vue实例已经挂载到页面中，可以获取到el中的DOM元素，进行DOM操作

## axios 是什么？怎么使用？

​	Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 nodejs 中。

```js
axios({
	//method:请求方式
	//url：请求地址
	//data：设置请求得数据（post/put请求等数据）
	//params:设置请求得参数（get和delect请求数据）
})then(function(res){  //处理成功得结果用then
	//consolelog(res)  可以获取一个对象
	//consolelog(res.data)  res对象里得data内有属性
})catch(function(err){ 
	consolelog(err) //打印错误信息
})
注意点：在vue中成功和失败处理的函数最好都用箭头函数
```

## axios或者ajax的跨域的2种方式

跨域行为是浏览器行为，是浏览器阻止了ajax行为。服务器与服务器之间是不存在跨域的问题的

反向代理：配置代理服务器请求另一个服务器数据，请求数据返回代理服务器，代理服务器返回给我们的客服端

- `nginx 反向代理`: 主要就是用了`nginx.conf`内的`proxy_pass http://xxx.xxx.xxx`,会把所有请求代理到那个域名。 备注：nginx 音译：NGX

  ```js
  反向代理配置:
  server { 
      #监听端口 
      listen 80; 
      #服务器名称，也就是客户端访问的域名地址 
      server_name  a.xxx.com; 
      #nginx日志输出文件 
      access_log  logs/nginx.access.log  main; 
      #nginx错误日志输出文件 
      error_log  logs/nginx.error.log; 
      root   html; 
      index  index.html index.htm index.php; 
      location / { 
          #被代理服务器的地址 
          proxy_pass  http://localhost:8081; 
          #对发送给客户端的URL进行修改的操作 
          proxy_redirect     off; 
          proxy_set_header   Host             $host; 
          proxy_set_header   X-Real-IP        $remote_addr; 
          proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for; 
          proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504; 
          proxy_max_temp_file_size 0; 
     } 
  } 
  
  // 注意点
  // server_name：代表客户端向服务器发起请求时输入的域名
  // proxy_pass：代表源服务器的访问地址，也就是真正处理请求的服务器(localhost+端口号)。
  ```

- cors：需要后端来配置。后端需要设置。

1.  Access-Control-Allow-Origin：xxx  //处理session问题，指定允许访问的域名
2. Access-Control-Allow-Methods: POST, GET, OPTIONS // 允许那些行为方法
3. Access-Control-Allow-Headers: X-PINGOTHER, Content-Type  // 允许的头部字段
4. Access-Control-Max-Age: 86400  // 有效期



## 一次完整的http请求

1. 域名解析
2. 建立连接(三次握手，没把握不要说，避免挖坑)
3. 发送请求
4. 服务器响应，浏览器获取html代码
5. 浏览器解析html代码，并请求html代码中的资源
6. 浏览器对页面进行渲染并呈现给客户
7. 断开请求（四次分手，没把握不要讲）

## 在vue.cli中怎样使用自定义的组件？

- 在components目录创建组件，组件内的script一定要export default
- 在需要使用的页面中导入 import 组件名称 from 路径
- 注入vue的子组件的component属性上面
- 在template视图view中使用

## 对Vue中的template编译是怎么理解的？

**首先，通过compile编译器把template编译成AST语法树**(abstract syntax tree 即源代码的抽象语法结构的树状表现形式)，compile是createCompiler的返回值，createCompiler是用以创建编译器的。另外compile还负责合并option；

然后，AST会经过generate(将AST语法树转化成render function字符串的过程)得到render函数，render的返回值是虚拟DOM节点，里面有（标签名、子节点、文本等等）；

## Vue-loader是什么？有什么作用？

vue-loader是解析 .vue 文件的一个加载器，把 template/js/style转换成 js 模块；

用途：js可以写es6、style样式可以scss或less；template可以加jade等；

## 怎么定义vue-router动态路由？

**动态路由**的创建，主要是使用 path 属性过程中，使用动态路径参数，以冒号开头，如：path: /user/:id

```js
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    // 可以匹配: /article/1  /article/2  /article/xxx
    { path: '/article/:id', component: Article }，
    // 如果在动态的路由参数中使用了?,表示该路由参数可选  
    // 可以匹配 /article   /article/1   /article/2
    { path: '/article/:id?', component: Article }，
  ]
})
```

## Vue-router有几种导航钩子？（说明有几种钩子，再逐一解释对应钩子的作用和使用场景）

1. #### 1 、全局导航钩子

   beforeEach
    beforeResolve
    afterEach

   #### 2 、某个路由独享的导航钩子

   beforeEnter

   #### 3 、路由组件上的导航钩子

   beforeRouteEnter
    beforeRouteUpdate (2.2 新增)
    beforeRouteLeave

   各种钩子的详细使用说明<https://www.jianshu.com/p/06d08ea39e31>

## 父子组件的生命周期顺序是怎么样的？

一、加载渲染过程

```repl
父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

```

二、子组件更新过程

```repl
父beforeUpdate->子beforeUpdate->子updated->父updated

```

三、父组件更新过程

```repl
父beforeUpdate->父updated

```

四、销毁过程

```
父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

```

## Vuex 是什么？ 怎么使用 ？ 那些场景使用它？

vuex是vue的状态管理工具，状态即数据。 状态管理就是管理vue中的数据

场景有：单页应用中，组件之间的状态、组件之间涉及大量的组件通讯

## vue-router的实现原理是怎么样的？

原理核心就是 更新视图但不重新请求页面。

vue-router实现单页面路由跳转，提供了三种方式：hash模式、history模式、abstract模式，根据mode参数来决定采用哪一种方式。

## vue如何实现父子组件通信，以及非父子组件通信？

​      **父传子**

1. ```
   在父组件的模版中，给子组件增加一个自定义的属性。
     <son :car="car"></son>
     //前面的:car是自定义属性名，是子组件要接收的属性名
     //后面的car是父组件的数据
   子组件通过props属性进行接收
     //接收父组件传递过来的值
     props: ['car']
   
   ```

```js
    **子传父**
 父组件给子组件注册事件
<son @fn='getData'></son>

```

- 子组件触发自定义事件，并且把要传递的数据作为参数进行传递

```js
//$emit可以出发当前实例的事件
this.$emit('getData', this.car);

```

- 父组件获取值

```js
methods: {
    //1. 父组件中定义了一个方法，用于获取数据
    getData (skill) {
        console.log("父组件中提供的方法", skill);
        this.skill = skill;
    }
}
//$emit 绑定一个自定义事件event，当这个这个语句被执行到的时候，就会将参数arg传递给父组件，父组件通过@event监听并接收参数

```

**非父子**

​	首先创建事件总线 

```
const bus = new Vue()

```

- 组件A给bus注册事件

```js
 //rose在组件创建的时候，给bus注册了一个事件
created () {
    bus.$on("get", (msg)=>{
        console.log("这是rose注册的事件", msg);
        this.msg = msg;
    });
}

```

- 组件B触发bus的事件

```js
<button @click="send">表白</button>

methods: {
    send() {
        bus.$emit("get", this.msg);
    }
}

```

- 组件A通过事件处理程序可以获取到传递的值

```js
bus.$on("get", (msg)=>{
    console.log("这是rose注册的事件", msg);
    this.msg = msg;
});

```

**注意点：1. 必须是同一辆公交车  2. 注册的事件和触发的事件必须保持一致**

## Vue的路由有几种模式？分别有什么特点的？

vue-router有两种模式，hash模式和history模式

hash模式：vue-router默认的模式，使用url的hash来模拟一个完整的url，当url改变时，页面不会重新加载

history模式：充分利用history.pushState API来完成url跳转而不用重新加载页面，需要后台的配置支持

## vue-router 有哪几种导航守卫（钩子）?

一.全局守卫

1. router.beforeEach((to,from,next)=>{})

   ​	回调函数中的参数，to：进入到哪个路由去，from：从哪个路由离开，next：函数，决定是否展示你要看到的路由页面

   2.全局后置钩子router.afterEach((to,from)=>{})

   ​	只有两个参数，to：进入到哪个路由去，from：从哪个路由离开

二.组件内的守卫

​	     1.beforeRouteEnter:(to,from,next)=>{}

2. 离开这个组件时，beforeRouteLeave:(to,from,next)=>{}

三.路由独享的守卫

​	 beforeEnter:(to,from,next)=>{}，用法与全局守卫一致。只是将其写进其中一个路由对象中，只在这个路由下起作用

## Vue路由里常用的钩子函数有哪些？（例举出不少于4个）

- 全局钩子

全局钩子有三个，分别是beforeEach、beforeResolve和afterEach，在路由实例对象注册使用；

- 路由级钩子

路由级钩子有beforeEnter，在路由配置项中项定义；

- 组件级钩子

组件级钩子有beforeRouteEnter、beforeRouteUpdate和beforeRouteLeave，

## vue过滤器的作用，Vue如何自定义一个过滤器

过滤器的作用：是一个通过输入数据，能够及时对数据进行处理并返回一个数据结果

**方法**

在组件的选项中定义本地的过滤器：

```js
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}

```

或者在创建 Vue 实例之前全局定义过滤器：

```js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})

```

当全局过滤器和局部过滤器重名时，会采用局部过滤器。

## 在Vue中$route和$router有什么区别

$router: 整个路由对象

$route: 指的是当前路由相关的信息

## Vue中常用的修饰符有哪些？别分有什么含义？

### 事件修饰符

语法   @事件名.修饰符=“事件函数”   可以有多个事件修饰符

- `stop`       阻止冒泡，调用 eventstopPropagation()
- `prevent`    阻止默认行为，调用 eventpreventDefault()

按键修饰符

- enter 回车键
- esc 退出键
- tab tab键

## 什么是vue的计算属性

computed是vue实例的计算属性。

优点

- 1计算属性可以把逻辑的操作放在vue中，不是放到模板做，方便维护
- 2计算属性的性能非常高，基于缓存实现，计算一次后，后面再次使用，可以直接用，不用计算
- 3计算属性的值依赖的属性发送改变，计算属性会重新执行一次。

## Vue中如何进行深度监听

当监听的数据是复杂数据类型时才需要进行深度监听，这时要使用watch的完整形态。在监听的属性对象中加入 deep：true

## v-if和v-show的区别

v-show和v-if都是控制元素的显示和影藏的

v-show：通过元素的display:none来控制显示影藏,除了一开始生成是性能消耗大，后面显示影藏消耗不大，适合频繁的切换

v-if：通过元素的删除和创建来控制元素的显示和隐藏，正因为如此，所以消耗性能一直都很大，适合不频繁的切换。

## 模块化和组件化的区别是什么？Vue是组件化还是模块化开发

- 模块化：（如果记性不好，只需要记住第**一**条和第**三**条）

  ​	(1)  js文件按照功能分离，是从代码逻辑的角度进行划分的，方便代码分层开发，保证每个能能模块的职能单一；

   	(2)模块化开发遵循三个规范：AMD、CMD、COMMONJS,网页端有AMD，CMD开发规范，

  ​      （3）任何的模块化开 发都离不开四个词：模块定义、接口暴露、模块引入、模块调用

- 组件化：是从UI界面的角度进行划分的，前端的组件化，方便UI组件的复用

  vue是组件化开发

## Es6增加了哪些东西？常用的有哪些

- 变量let 、const
- 字符串和变量的拼接 `` 反引号
- 解构赋值
- 箭头函数
- 数组的处理方法 filter map forEach find。。。。

## Promise有什么用处？怎么使用？

Promise是一个方案，用来解决多层回调嵌套的解决方案，可以把一个多层嵌套的同步、异步都有回调的方法，给拉直为一串.then()组成的调用链。

## Vue中keep-alive的作

## 用是什么？

1. <keep-alive>是Vue的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染DOM。
2. <keep-alive> 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。
3. 当组件在 <keep-alive> 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。

## Vue中如何获取dom元素进行操作？

在使用Vue的时候，有时候我们希望直接用Js操纵DOM来实现某些效果，具体实现只用利用Vue提供的ref属性以及this.$refs即可实现。

## Vue中 $set()什么时候用？

简单的说就是$set()给data对象新增属性，并触发视图更新

## Vue是如何自定义组件的？

```js
// 我们可以用Vue.component来自定义组件。
Vue.component('input-price', {     
template: '<input type='text'>'
})；

```

## vue如何实现单页面应用

1. 引入对应的vue-router.js
2. 指定一个盛放代码片段的容器：<router-view></router-view>）3
3. 创建业务所需要的各个组件
4. 配置路由词典，每一个路由地址的配置对象（要加载哪个页面...）

## Vue的首屏加载有哪些优化的方式？

1. 在使用ui库时,尽量使用按需加载方式。
2. JS文件按需加载。
3. 路由懒加载，路由懒加载也就是 把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件。

## vue传参的2种方式

1. params传参：缺点:F5强制刷新，数据会被清空
2. query  路径传参。参数不会被强制刷新

## Vuex怎么实现数据共享

用State负责存储整个应用的状态数据，一般需要在使用的时候在跟节点注入store对象，后期就可以使用this.$store.state直接获取状态。一般是在main.js文件中引入store的文件，从而使用。这个store可以理解为一个容器。

## webpack是用来做什么的，和gulp有什么区别

webpack主要是处理依赖管理,合并代码,配合各种插件实现各种功能

1. 根据模板生成html,并且自动处理上面的css/JS引用路径。
2. 自动处理img里图片路径，css样式中背景图的路径。。。
3. 开启本地服务器，变写代码边自动更新页面内容。
4. 编译js、es6、sass、lesst等等并添加md5、sourcemap等辅助。
5. 异步加载内容，比如弹出框，不需要时不加载到dom。
6. 配合vue.js、react等框架开发等。

**gulp**强调的是前端开发的工作流程，通过配置一系列的task，可以进行文件压缩合并、雪碧图、启动server、版本控制等

**webpack**是一个前端模块化方案，更侧重模块打包，我们可以把开发中的所有资源（图片、js文件、css文件等）都看成模块，通过loader（加载器）和plugins（插件）对资源进行处理，打包成符合生产环境部署的前端资源。

## **虚拟DOM**

虚拟DOM( VDOM )是利用了js的Object对象模型来模拟真实DOM,它的结构是一个树形结构，操作虚拟DOM比真实DOM更高效

## diff算法

diff算法的思维来自于后端,用来比较两个或多个文件,返回值是文件的不同点,diff算法进行的是同级比较

diff算法的比较思维，会出现以下四种情况:

1. 此节点是否被移除  *--->*  *添加新的节点*
2. 属性是否被更改 *--->*  *旧属性改为新属性*
3. 文本内容被改变  *--->*  *旧内容改为新内容*
4. 结构完全不同  *--->*  *节点将被整个移除替换*
5. 有唯一key属性，只是根据key进行diff算法

## **自己封装过什么组件，描述一下**（建议百度，准备一2个）

## V-modle可以用什么替代

v-model其实是v-bind和v-**on**的语法糖

 `<input v-model="something">`其实是

 `<input v-bind:value="something" v-on:input="something = $event.target.value">`的语法糖

 v-**on**，它其实就是一个事件绑定器。我们仔细阅读一下

<input v-bind:value="something" v-on:input="something = $event.target.value">`，

 发现它由两部分组成：v-bind:**value**和v-**on**:**input**，必须是**value**属性和**input**事件，否则也不会等价于v-model，而且**input**事件里面，正好是something等于当前输入值。

## **Vue不能检测哪些变动的数组，官方是怎么解决的？你是怎么解决的？**

1.当你利用索引设置一个项时, 例如: **vm**.items[indexOfItem] = newValue;

  **解决方法**：使用   Vue.**set**  || this.$**set** 或splice(indexOfItem, 1, newValue);

 2.当你修改数组的长度时,例如: **vm**.items.length = newLength;

  **解决方法**：使用splice(newLength)

## vuex的使用流程

Vue组件接收交互行为，调用dispatch方法触发action相关处理，若页面状态需要改变，则调用commit方法提交mutation修改state，通过getters获取到state新值，重新渲染Vue Components，界面随之更新。

## Vue 组件中data 为什么必须是函数？

为了数据独立.

解释如下：对象为引用类型，当重用组件时，由于数据对象都指向同一个data对象，当在一个组件中修改data时，其他重用的组件中的data会同时被修改；而使用返回对象的函数，由于每次返回的都是一个新对象（Object的实例），引用地址不同，则不会出现这个问题。

## v-if和v-for的优先级

v-for和v-if不应该一起使用，必要情况下应该替换成computed属性。

原因：v-for比v-if优先，如果每一次都需要遍历整个数组，将会影响速度，尤其是当之需要渲染很小一部分的时候。

## 常见的实现数据绑定的做法有哪些？

发布者-订阅者模式（backbone.js）
脏值检查（angular.js）
数据劫持（vue.js）

**发布者-订阅者模式:** 一般通过sub, pub的方式实现数据和视图的绑定监听，更新数据方式通常做法是 `vm.set('property', value)`

**脏值检查:** **angular.js 是通过脏值检测的方式比对数据是否有变更**，最简单的方式就是通过 setInterval() 定时轮询检测数据变动，当然Google不会这么low，angular只有在指定的事件触发时进入脏值检测，大致如下：

- DOM事件，譬如用户输入文本，点击按钮等。( ng-click )
- XHR响应事件 ( $http )
- 浏览器Location变更事件 ( $location )
- Timer事件( $timeout , $interval )
- 执行 $digest() 或 $apply()

**数据劫持:** vue.js 则是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

## 不用 vuex会带来什么问题

一、可维护性会下降，你要想修改数据，你得维护三个地方

二、可读性会下降，因为一个组件里的数据，你根本就看不出来是从哪来的

三、增加耦合，大量的上传派发，会让耦合性大大的增加，本来Vue用Component就是为了减少耦合，现在这么用，和组件化的初衷相背。

## 解构赋值

es6允许按照一定的模式，从数组或对象中提取值，给变量进行赋值，称为解构赋值。说白了，就是批量的声明变量

## v-on可以同时绑定多个方法么

可以，vue2.x中下面为例

`<button v-on=``"{mouseenter: onEnter,mouseleave: onLeave}"``>鼠标进来1</button>`

也可分开写  `

```js
<form @keyup.delete="onKeyup" @submit.prevent="onSubmit"><form>

```

## vue中key值得作用

key用来给每个节点做一个唯一标识，目的解决就地更新策略，在项目中key值一般都是以id做唯一值得。

低层是在虚拟DOM进行diff算法时，会根据key属性做比较，这样效率高，同时还解决了就地复用问题。

## 在网页中的应该使用奇数还是偶数的字体？

偶数：在使用移动端布局，和百分比布局的时候，换算单位和数值更加的方便精准。

## Vuex中actions和mutations的区别

mutations 内处理的是同步函数，原因：任何回调函数中进行的状态改变都是无法追踪的,  devtools会对mutations的每一条提交做记录,记录上一次提交之前和提交之后的状态,在上面的例子中无法实现捕捉状态,因为在执行mutations时,内部回调函数还没有执行

为了解决mutations只有同步的问题,提出了actions(异步),专门用来解决mutations只有同步无异步的问题

## Vue-cli打包命令是什么？打包后的路径问题

命令行输入：npm  run  build

第一个问题，文件引用路径。我们直接运行打包后的文件夹中的index.html文件，会看到网页一片空白，f12调试，全是css，js路径引用错误的问题。

解决：到config文件夹中打开index.js文件。

文件里面有两个assetsPublicPath属性，更改第一个，也就是更改build里面的assetsPublicPath属性：

![img](https://images2017.cnblogs.com/blog/1202246/201711/1202246-20171121094900196-325897431.png)

assetsPublicPath属性作用是指定编译发布的根目录，**‘/’指的是项目的根目录 ，’./’指的是当前目录。**

**改好之后重新打包项目，运行index.html文件，我们可以看到没有报错了。但是router-view里面的内容却出不来了。**

 

**第二个问题：router-view中的内容显示不出来。**原因是路由是history模式，直接把这个注释掉即可。

## 第三个问题  就是背景图片引用资源错误

此时通过img标签引入的图片显示正常，是因为img为html标签，他的路径是由index.html开始访问的，他走static/img/'图片名'是能正确访问到图片的

但是app.css访问static/img/'图片名'是访问错误的，因为在css目录下并没有static目录。所以此时需要先回退两层到根节点处才可以正确获取到图片。

具体办法是：

打开build/utils.js，在图中相应位置加入红框内容，其中值可能会有不同，若不同，自己配置成相应的即可

## ajax请求应该写在组件的created中还是vuex的actions中

一、如果请求来的数据是不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入vuex 的state里。
二、如果被其他地方复用，这个很大几率上是需要的，如果需要，请将请求放入action里，方便复用，并包装成promise返回，在调用处用async await处理返回的数据。如果不要复用这个请求，那么直接写在vue文件里很方便。

## vuex的State特性是？

答：
一、Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于与一般Vue对象里面的data
二、state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
三、它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中

## vuex的Getter特性是？

答：
一、getters 可以对State进行计算操作，它就是Store的计算属性
二、 虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
三、 如果一个状态只在一个组件内使用，是可以不用getters

## vuex的Mutation特性是？

答：
一、Action 类似于 mutation，不同在于：
二、Action 提交的是 mutation，而不是直接变更状态。
三、Action 可以包含任意异步操作

## active-class是哪个组件的属性？嵌套路由怎么定义

router-link的组件，嵌套路由在路由配置里面再配置个child，并在内部配置对应的路由

## axios用post请求问题会有什么问题

使用axios发送post有肯发现会变成options，传递的数据也找不到，因为发送的数据默认是字符串格式，所以需要将其转换成URL格式，以&进行拼接。还有一点要注意，如果URL是远程接口时，会报404错误，需要实例化一个新的axios并设置请求头。****

解决办法：

- 使用Qs.stringify()将对象序列化成URL形式，qs是axios自带的，无需下载，直接引入即可
- 设置请求头里的content-type’: ‘application/x-www-form-urlencoded



## 简述readonly 与 disabled 的区别

都可以让表单元素变成不可用状态

1. **disabled**属性可以作用于所有的表单元素，让表单元素无法被提交。
2. **readonly**属性只对`<input type="text">、<input type="number">、<textarea>和<input type="password">等可以输入的表单元素`有效。

## 查找字符串的最长字串

​         备注：只要思想不滑坡，方法总比困难多！！！百度去吧！！！少年

## JS实现异步有几种方式

**一、回调函数**：回调函数是异步编程最基本的方法，其优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度[耦合](http://en.wikipedia.org/wiki/Coupling_(computer_programming))（Coupling），流程会很混乱，而且每个任务只能指定一个回调函数。

**二、事件监听**：采用事件驱动模式。任务的执行不取决代码的顺序，而取决于某一个事件是否发生。监听函数有：on，bind，listen，addEventListener，observe**

​	**优点**：比较容易理解，可以绑定多个事件，每一个事件可以指定多个回调函数，而且可以**去耦合，有利于实现模块化。**

​          **缺点**：整个程序都要变成事件驱动型，运行流程会变得不清晰

三、**发布/订阅**

我们假定，存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"（publish）一个信号，其他任务可以向信号中心"订阅"（subscribe）这个信号，从而知道什么时候自己可以开始执行。这就叫做["发布/订阅模式"](http://en.wikipedia.org/wiki/Publish-subscribe_pattern)（publish-subscribe pattern），又称["观察者模式"](http://en.wikipedia.org/wiki/Observer_pattern)（observer pattern）。

四、**promise对象（promise 模式）**

（1）promise对象是commonJS工作组提出的一种规范，一种模式，目的是为了异步编程提供统一接口。

（2）**promise是一种模式，promise可以帮忙管理异步方式返回的代码。**他讲代码进行封装并添加一个类似于事件处理的管理层。我们可以使用promise来注册代码，这些代码会在在promise成功或者失败后运行。

（3）promise完成之后，对应的代码也会执行。我们可以注册任意数量的函数再成功或者失败后运行，也可以在任何时候注册事件处理程序。

（4）promise有两种状态：1、等待（pending）；2、完成（settled）。

promise会一直处于等待状态，直到它所包装的异步调用返回/超时/结束。

（5）这时候promise状态变成完成。完成状态分成两类：1、解决（resolved）；2、拒绝（rejected）。

（6）promise解决（resolved）：意味着顺利结束。promise拒绝（rejected）意味着没有顺利结束。

**优点**：回调函数写成了链式写法，程序的流程可以看得很清楚，而且有一整套的配套方法，可以实现很多强大的功能

**缺点：**编写和理解都相对比较难。

**五、优雅的async/await**

1. async/await更加语义化，async 是“异步”的简写，async function 用于申明一个 function 是异步的； await，可以认为是async wait的简写， 用于等待一个异步方法执行完成；
2. async/await是一个用同步思维解决异步问题的方案（等结果出来之后，代码才会继续往下执行）
3. 可以通过多层 async function 的同步写法代替传统的callback嵌套

## 阐述一下异步加载的过程

异步加载即非阻塞加载，浏览器在加载JS的同时，还会继续进行后续页面的处理。

过程很长，建议找一个记住步骤：<https://www.jianshu.com/p/3aa3a3e27417>

## jQuery的事件委托on、live、delegate之间有什么区别

1. live 把事件委托交给了document（根节点），document 向下去寻找符合条件的元素（）， 不用等待document加载结束也可以生效。
2. delegate可指定事件委托对象，相比于live性能更优，直接锁定指定选择器；
3. on事件委托对象选填，如果不填，即给对象自身注册事件，填了作用和delegate一致。
4. band只能给调用它的时候已经存在的元素绑定事件，不能给未来新增的元素绑定事件，存在局限性。
5. 在jQuery 1.7中 .bind()、.live()和.delegate() 统一用on代替，解决混乱问题。

## JavaScript实现二分法查找

二分法查找要求线性表必须采用顺序存储结构，而且表中元素按关键字有序排列分为：递归法和非递归法，数据量大的时候推荐非递归法,普通数据用递归法

<https://blog.csdn.net/Ireallyyourfather/article/details/82809390>

## Vue-router跳转和location.href有什么区别

1、使用location.href='/url'来跳转，简单方便，但是刷新了页面。

2、使用history.pushState('/url')，无刷新页面，静态跳转。

3、引进router，然后使用router.push('/url')来跳转，使用了diff算法，实现了按需加载，减少了dom的消耗。

## delete和Vue.delete删除数组的区别

1. **delete**只是被删除的元素变成了 empty/undefined 其他的元素的**键值还是不变**。
2. **splice**直接删除了数组 **改变了数组的键值**。
3. **Vue.delete**直接删除了数组 **改变了数组的键值**。

## <router-link>在IE与火狐上点击失效（路由不跳转）问题

兼容问题：改成编程式导航

## 键盘挡住输入框内容

在input获得焦点时，触发焦点事件，让键盘弹出的同时，让form表单整体向上方移动，避免让表单被键盘盖住。

## 超大文件如何上传

借助js的Blob对象FormData对象可以实现大文件分片上传的功能，这两种办法都需要前后端代码配合，前端代码核心是把文件切片分段发送。

## vue事件中如何使用event对象

如果事件后面不传参，可以在调用方法时直接传参，如果事件后面带有其他参数，要使用$event与参数同时发送，在调方法时，对应位置就是event对象

## 项目中如何开启服务

项目的开启一般需要在node环境中开启，在项目依赖的文件package.json中有一个scripts代码带配置。内部配有开启服务指令

## Vue.set()方法

向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新属性，因为 Vue 无法探测普通的新增属性

语法：Vue.set( target, propertyName/index, value )

**参数**：

- `{Object | Array} target`
- `{string | number} propertyName/index`
- `{any} value`

## 项目批量加载本地图片

## 移动端click点击清除300ms延迟

原因：移动浏览器上支持的**双击缩放**操作，以及IOS Safari 上的**双击滚动**操作，是导致300ms的点击延迟主要原因。

解决办法：

**禁用缩放**  <meta name="viewport" content="initial-scale=1,maximum-scale=1"></code></pre>

**更改默认视口宽度**： <pre><code><meta name="viewport" content="width=device-width"></code></pre>，好处是仍然支持缩放。

 **指针事件和css touch-action:**新属性，可能存在浏览器兼容问题，如仅为解决点击延迟问题儿引入一整套指针事件有点过了

 **tap事件**：能较好解决点击延迟，并且对其他移动端触摸事件也有较好支持，但存在点透问题，不知最新版是否解决

 **fastclick:**当前较好的专门解决点击延迟的库，脚本尺寸相对较大。

1. 安装 npm I fastclick
2. 引入 import FastClick from 'fastclick'
3. 初始化FastClick实例,在页面的DOM加载完成后FastClick.attach(document.body)

## 浏览器中的回流和重绘是什么

重绘：当元素的外观或外观可见性（visibility）发生变化时会触发重绘
回流：render树中的部分或全部因为元素的规模尺寸、布局、隐藏等改变，需要重新计算render树。

## Vuex和单纯的全局对象有何不同

1)缺少时间旅行功能
2）vuex专做态管理，由一个统一的方法去修改数据，全部变量是可以任意修改的
3）做日志搜集，埋点的时候，有vuex更方便
4）全部变量多了会造成命名污染，vuex不会，同时解决了父组件与孙组件，以及兄弟组件之间通信的问题

## xss攻击

xss攻击条件：

1. 需要向web页面注入恶意代码；
2. 这些恶意代码能够被浏览器成功的执行。

攻击方式：

1. xss反射型攻击    例如：钓鱼网站：引诱用户点击恶意链接来攻击。
2. xss存储型攻击：恶意代码被保存到目标网站的服务器中，比如博客,论坛。系统管理员查看投诉信息时，窃取了客户的资料
3. 如果用到了cookie,需要设置http-only,避免cookie被更改。

解决办法：

1. 通过xss过滤工具，在表单或者url提交前对参数进行过滤     
2. 严格控制输出，检测用户输入得内容是否含有非法符号。例如<>（尖括号）、”（引号）、 ‘（单引号）、%（百分比符号）、;（分号）、()（括号）、&（& 符号）、+（加号）

## `CSRF`跨域请求伪造

**什么是CSRF,危害是什么**。

CSRF（Cross-site request forgery）跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（[XSS](https://baike.baidu.com/item/XSS)），但它与XSS非常不同，XSS利用站点内的信任用户，而CSRF则通过伪装成受信任用户的请求来利用受信任的网站。与[XSS](https://baike.baidu.com/item/XSS)攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比[XSS](https://baike.baidu.com/item/XSS)更具危险性

解决方式：

 - 验证 HTTP Referer 字段；
 - 在请求地址中添加 token 并验证；
 - 在 HTTP 头中自定义属性并验证

## 不使用object.defineProperty如何实现vue的双向数据绑定

​	 第一种：proxy  vue3.0开始使用

​	 第二种：脏数据检查机制，通过轮询实现。

## ObjectdefineProperty和Proxy

Object.defineProperty() 的问题主要有三个：

- 不能监听数组的变化
- 必须遍历对象的每个属性
- 必须深层遍历嵌套的对象

**3.0后使用proxy**，其实是解决object.defineProperty遗留的问题

Proxy 对象用于定义基本操作的自定义行为（如属性查找，赋值，枚举，函数调用等）

- 针对对象:针对整个对象,而不是对象的某个属性

- 支持数组:不需要对数组的方法进行重载，省去了众多 hack（hack：意思是对兼容的处理）

- 嵌套支持: get 里面递归调用 Proxy 并返回

  ```js
  const obj1 = new Proxy(obj, {   
    get(target, key) {     //target相对于劫持的对象 本例为obj
      return target[key]
    },
    set(target, key, value) {
      target[key] = value
    }
  })
  ```

  

## 如何检测一个数据具体的类型

简单数据类型：String、number、Boolean、Undefined、Null、Symbol

复杂数据类型：Object、Array、Function等

检测的方法有 6种，推荐记忆第一、二和**第六种**、

注意：第六种是最牛的。一定要牢记！！！！

**一、基本类型检测推荐用 typeof**   

- 注意点：检测其他简单数据类型都可以得到正确的结果，但是console.log(typeof null)得到的是object

- typeof检测复杂数据类型除了函数和对象能返回正确结果，其他的都返回object

  ```js
  console.log(typeof /\d/);//object
  console.log(typeof {});//object
  console.log(typeof []);//object
  console.log(typeof (null));//object
  console.log(typeof 123);//number
  console.log(typeof true);//boolean
  console.log(typeof function () {});//function
  console.log(typeof (undefined));//undefined
  ```

**二、复杂数据类型推荐用 instanceof**

缺点：只能进行类型的判断，并不能指明类型

例子：

```js
1 function fn() {}
2 let f = new fn;
3 console.log(f instanceof fn);//true
4 console.log(f instanceof Object);//true
5 let arr = [1,2,3,4];
6 console.log(arr instanceof Array);//true
```

​	**三**、constructor 也可以检测类型，但是并不可靠，因为容易被修改(原型替换)。所以开发时原型替换，最好把constructor也手动更改下

```js
1 function fn() {}
2 let f = new fn;
3 console.log(f.constructor.name);//fn
4 console.log(fn.constructor);//Function（）{}
5 console.log(Function.constructor);//Function（）{}
```

​	**四、 hasOwnproperty 检测当前属性是否为对象的使用属性**

注意点：在面试中很有能被问是=>在Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

​	语法: obj.hasOwnporperty("属性名（K值）")

```js
let obj = {
name:"xx"
};
console.log(obj.hasOwnProperty('name'));//true
console.log(obj.hasOwnProperty('xxxx'));//false
```

​	**五、isArray 判断是否为数组**

```js
console.log(Array.isArray([]));//true
console.log(Array.isArray(new Array()));//true
```

​	**六**.**Object.portotype.toString** （最好的）	

​	语法：Object.prototype.toString.call([value]) 简化后  {}.toString.call([value])

```js
Object.prototype.toString.call(12)//[object Number]
Object.prototype.toString.call(true)//[object Boolean]
```

- *"[object Number]"*
- *"[object String]"*
- *"[object Object]"*
- *"[object Function]"*
- *"[object Undefined]"*
- *"[object Null]"*
- *"[object RegExp]"*
- *"[object Boolean]"*

使用toString检测数据类型，不管你是啥值，我们都可以正常检测出需要的结果（这个方法检测是`万能`的）

## 如何判断字符串哪个字符串出现的次数最多

charAt()

语法： str.charAt(index)    作用：找到字符串中对应下标的字符。如果下标位置不存在字符，返回一个空字符串

解决方式：声明一个空的对象，遍历每个字符，

1. 不存在： 把这个字符存到对象中，让它的值为1  =>  json[str.charAt(i)] =1;
2. 存在： 找到对应的key，然后让他的值+1   =>  json[str.charAt(i)]++;

​      最终形成一个对象：key都是唯一的。值是重复的统计。代码如下

// 要求找到重复最多的字符串，并统计出现的次数

```js
    var str = 'abasdasdqeqweqad'
    var json = {}; //定义JSON格式的变量，备后用
    //以下代码遍历str,将其中的字符和该字符出现的次数存放在json中
    for (var i = 0; i < str.length; i++) {
      //判断json中是否存在当前str.charAr(i)的值
      if (!json[str.charAt(i)]) {
        //如果不存在，则将其存放在json中，并且赋值为1，相当于出现的次数为1
        json[str.charAt(i)] = 1;
      } else {
        //如果存在，则这个字符的值加1，相当于次数加1
        json[str.charAt(i)]++;
      }
    }
    //定义变量char存储出现次数最多的字符，number为该字符出现的次数
    var char = '';
    var num = 0;
    //遍历json,找到值最大的字符，值相当于次数
    for (var key in json) {
      //判断当前json中的键值（相当于当前键所在字符的次数）是否大于num
      if (json[key] > num) {
        //如果大于num,就将键（字符）存放在char中，键值存放在num中
        char = key;
        num = json[key];
      }
    }
    //输出结果
    console.log("出现次数最多的字符是" + char +
      ",次数为：" + num + "。")
```

## 页面的参数传递的时候，出现了中文乱码的情况

解决：

在url传参的时候用encodeURL()进行编码一下中文参数传递到下个页面，然后用decodeURL()进行解码一下就可以了

## 音频视频禁止播放

原因有2种：

1.现在大多浏览器都不支持自动播放，影响用户体验。

2.需要在video标签中添加muted属性。

3.ie的浏览器对MP4的编码方式要求比较严格。视频必须是H.246音频必须是：必须是: AAC      **测试后必需是：mpeg**

## ajax缓存如何处理	

```
解决这个问题最有效的办法是禁止页面缓存，有以下几种处理方法：

1、在ajax发送请求前加上 xmlHttpRequest.setRequestHeader(“Cache-Control”,”no-cache”);

2、在服务端加 header(“Cache-Control: no-cache, must-revalidate”);

3、在ajax发送请求前加上 xmlHttpRequest.setRequestHeader(“If-Modified-Since”,”0″);

4、在 Ajax 的 URL 参数后加上 "?fresh=" + Math.random(); //当然这里参数 fresh 可以任意取了

5、第五种方法和第四种类似，在 URL 参数后加上 "?timestamp=" + new Date().getTime();

6、用POST替代GET：不推荐

7、jQuery提供一个防止ajax使用缓存的方法：

<script type="text/javascript" language="javascript"> 
     $.ajaxSetup ({ 
           cache: false //close AJAX cache 
      }); 
</script>

8、修改load 加载的url地址，如在url 多加个时间参数就可以：

function loadEventInfoPage(eventId){
    $.ajaxSetup ({ 
       cache: true // AJAX cache  下面加上时间后load的页面中的js、css图片等都会重新加载，   
         //加上这句action会重新加载，但是js、css、图片等会走缓存 
    }); 
    $("#showEventInfo").load(ctx + "/custEvents/viewEvent.action",  {"complaint.Id":eventId, "tt":(new Date()).getTime()},function(){}) 
}

9、设置html的缓存

<META HTTP-EQUIV="Pragma" CONTENT="no-cache">    
<META HTTP-EQUIV="Cache-Control" CONTENT="no-cache">    
<META HTTP-EQUIV="Expires" CONTENT="0">
```

## 语义化标签的好处

标签语义化的好处：

+ HTML结构清晰

+ 代码可读性较好

+ 无障碍阅读

+ 搜索引擎可以根据标签的语言确定上下文和权重问题

+ 移动设备能够更完美的展现网页（对css支持较弱的设备）

+ 便于团队维护和开发

## 函数声明的方式（5种）

声明式函数 function render(){...}

声明匿名函数 ：var render = function(){...}

函数表达式：var render= function render(){...}

构造函数Function     f  = new Function(){....}

 箭头函数：  f = (x,y)=>x+y



## 如何配置反向代理

webpack反向代理 

下载个插件webpack-dev-server

在项目webpack.json配置的文件中配置一个proxy：{}选项。

在内部配置正则匹配代理服务下的 /api地址 ,然后配置内部代理到的接口地址；例如

```js
 proxy: {    
            '/api': {   //代理服务下的地址
                target: 'http://localhost:3000',//代理地址的端口
                changeOrigin: true
            }
        }
// changeOrigin作用：代理服务器会在请求头中加入相应Host首部，然后目标服务器就可以根据这个首部来区别要访问的站点了。假如你在本地80端口起了apache服务器，服务器配了两个虚拟站点a.com b.com,设置代理之后并且changeOrigin为true 。此时就可以正确方法问道虚拟主机下的文档内容。否则访问a b站点等同于访问localhost。webpack dev sever用的是node-http-proxy
```

## vue中的插槽

插槽的作用：作为承载分发内容的出口

1.匿名插槽 也叫默认插槽。作用就是占位置，有内容输入时显示当前内容

2.具名插槽：同一个组件内需要多个插槽的时候，需要为给插槽添加name属性来区分，这叫具名插槽。在向具名插槽提供内容的时候，我们可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称，现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 `v-slot`的 `<template>` 中的内容都会被视为默认插槽的内容。

3.作用域插槽：携带有子组件数据的插槽，目的是让插槽内容能够访问子组件的数据。

## 动态添加数据

## vue-router在什么阶段挂载上去的

vue-router是vue的路由：是在created钩子函数时间段内挂载上去的。

## axios能在IE的浏览器中使用吗

不能，原因因为axios本质上是封装了ES6语法的promise,而promise在IE上并不兼容。

解决办法：解决办法：安装babel-polyfill

具体步骤：

 1.安装：npm i --save-dev babel-polyfill 

2.在main.js中   import 'babel-polyfill' 

3.在build/vue-loader.conf.js中：

```
module.exports = {
  entry: { app: ['babel-polyfill', './src/main.js'] },
  ......
}
```

## vue-router的几种实例方法以及参数传递

**第一种：get方法**

传递值

```
<router-link :to="{path:'/test',query: {name: id}}">跳转</router-link>
```

接收值

```
this.$route.query.name
```

第二种：post方法

传递值

```
<router-link :to="{path:'/test',push: {name: id}}">跳转</router-link>
```

接收值

```
this.$route.params.name  （在页面刷新的时候就会消失）
```

第三种：路由方法

传递值

```
//?问号的意思是该参数不是必传项
//多个参数用/:id连接
//path: '/Driver/CarManager/CarSetting/:car_id/:page_type',
{
    path: 'test/:name?',
    name: 'test',
    component: 'test.vue',
    props: true,
},
```

接收值

```
props:{name:{required:false,default:''}} （页面刷新不消失，可以在路由配置中设置参数）
```

 如果在链接上设置 replace 属性，当点击时，会调用 router.replace() 而不是 router.push()，于是浏览器不会留下 history 记录。

```
  <router-link :to="{ path: '/abc'}" replace></router-link>
```

## position和transition的谁的层级高

## css3的:nth-child和:nth-of-type的区别是什么？

1、:nth-child() 选择器
:nth-child(n) 选择器匹配属于其父元素的第 N 个子元素，不论元素的类型，n 可以是数字、关键词或公式。

2、:nth-of-type(n)

:nth-of-type(n) 选择器匹配属于父元素的特定类型的第 N 个子元素的每个元素，n 可以是数字、关键词或公式。

## ajax请求应该写在组件的created中还是vuex的actions中

actions中。

## vue-router如何响应路由参数的变化？

想对路由参数的变化作出响应的话，你可以简单地 **watch (监测变化) $route 对象**：

```
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

或者设置路由拦截

```
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
  }
}
```

注意是：

（1）从同一个组件跳转到同一个组件。

（2）生命周期钩子created和mounted都不会调用。

## axios每次发送请求会发送两次请求

CORS跨域分为 简单跨域请求和复杂跨域请求
简单跨域请求是不会发送options请求的
复杂跨域请求会发送一个预检请求options
复杂跨域请求要满足以下：
1、请求方法不是GET/HEAD/POST
2、POST请求的Content-Type并非application/x-www-form-urlencoded, multipart/form-data, 或text/plain
3、请求设置了自定义的header字段

如果不想发送option请求可以改为简单请求
比如你的Content-Type可能是application/json格式
将其改为application/x-www-form-urlencoded

## vue组件之间的双向绑定

1. 在子组件的 data 中创建一个props属性的副本
2. watch props 中的属性 目的是为了同步父组件对 props 的修改
3. watch 属性副本，emit一个函数通知到组件外，目的是为了同步子组件对属性的修改。这时利用了.sync，所以可以使用 this.$emit("update:name", newVal); 这种形式。

```
   var componentInput = {
        template : "<div>子组件<input v-model='curName'></div>",
        data : function(){
            return {
                curName : this.name
            }
        },
        props : ['name'],
        watch : {
            'name' : function(newVal, oldVal){
                this.curName = newVal;
            },
            curName : function(newVal, oldVal){
                this.$emit("update:name", newVal);
            }
        }
    }
```

## vue阻止修改现有的属性

调用Object.freeze('冻结对象')方法冻结datas对象，注意一定在vue实例化之前调用。

## ajax和axios的区别

**ajax**

- 本身是针对MVC的编程,不符合现在前端MVVM的浪潮
- 基于原生的XHR开发，XHR本身的架构不清晰，已经有了fetch的替代方案
- JQuery整个项目太大，单纯使用ajax却要引入整个JQuery非常的不合理（采取个性化打包的方案又不能享受CDN服务）

**aixos**

- 从 node.js 创建 http 请求
- 支持 Promise API
- 客户端支持防止CSRF=>让你的每个请求都带一个从cookie中拿到的key, 根据浏览器同源策略，假冒的网站是拿不到你cookie中得key的,后台可以轻松分辨用户
- **提供了一些并发请求的接口**（重要，方便了很多的操作）

## 如何初始化vue项目

```js
// 全局安装 vue命令
yarn global add @vue/cli
// 初始化项目
vue create 项目名称
```

**选择预设**： babel 、路由、路由模式、css预处理语言、ESLint、校验规则、文件生成模式

**启动项目**：yarn serve

## Webpack的四个核心点

入口(entry)、出口(output)、loader、插件-plugins

**入口(entry)**：每个 HTML 页面都有一个入口起点。单页应用(SPA)：一个入口起点，多页应用(MPA)：多个入口起点。入口起点告诉 webpack 从哪里开始，并根据依赖关系图确定需要打包的内容

```
module.exports = {
entry:  './src/index.js'
};
```

**输出(output)**：将所有的资源(assets)归拢在一起后，还需要告诉 webpack 在哪里打包应用程序。webpack 的 `output` 属性描述了如何处理归拢在一起的代码(bundled code)。`output`选项可以控制webpack如何向硬盘写入编译文件。注意，即使可以存在多个`entry`起点，但只指定一个`output`配置。

```
//webpack.config.js
module.exports = {
entry: './src/index.js', //入口
output: {
path: './home/proj/public/assets', //输出文件的名称
filename: 'bundle.js' // 输出目录path的绝对路径
}
};
```

**loader**：webpack loader 在文件被添加到依赖图中时，将文件源代码转换为模块。loader可以识别对应的loader进行对应转换。

```
  module: {
rules: [
{ 
test: /\.txt$/,  //文件只要是以txt结尾就用raw-loader处理一下
use: 'raw-loader' 
}
]
}
```

**插件-plugins**：插件是 wepback 的支柱功能。插件目的在于解决 loader无法实现的**其他事**

## Promise怎么理解的(请搜索：Promise对象)

## token如何加密的

tocken 其实相当于一个字符串，根据某个页面的url作为键名，通过base64和一些规则形成的字符串作为键值存放到session中，请求的时候带上这个值在后端进行验证，后端获取页面的url当做键值， 穿过来的值当键值，符合session验证则通过验证。

## vue给动画配置了6个类名来控制动画的变化自由（动画有哪些阶段）

1. v-enter：定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。
2. v-enter-active：定义过渡的状态。在元素整个过渡过程中作用，在元素被插入时生效，在 transition/animation 完成之后移除。这个类可以被用来定义过渡的过程时间，延迟和曲线函数。
3. v-enter-to: 2.1.8版及以上 定义进入过渡的结束状态。在元素被插入一帧后生效 (于此同时 v-enter 被删除)，在 transition/animation 完成之后移除。
4. v-leave: 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
5. v-leave-active：定义过渡的状态。在元素整个过渡过程中作用，在离开过渡被触发后立即生效，在 transition/animation 完成之后移除。这个类可以被用来定义过渡的过程时间，延迟和曲线函数。
6. v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。在离开过渡被触发一帧后生效 (于此同时 v-leave 被删除)，在 transition/animation 完成之后移除。

## 在vuex中使用异步修改，为什么要异步修改?

## 如何获取DOM更新后的数据

在updated钩子函数中可以获取DOM更新后的数据

## DOM和BOM的区别

BOM是浏览器对象模型，用来获取或设置浏览器的属性、行为，例如：新建窗口、获取[屏幕分辨率](https://www.baidu.com/s?wd=%E5%B1%8F%E5%B9%95%E5%88%86%E8%BE%A8%E7%8E%87&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)、浏览器版本号等。
DOM是[文档对象模型，用来获取或设置文档中标签的属性，例如获取或者设置input表单的value值。

## json和xml的区别

**xml**扩展标记语言 (Extensible Markup Language, XML) ，用于标记电子文件使其具有结构性的标记语言，可以用来标记数据、定义数据类型，是一种允许用户对自己的标记语言进行定义的源语言。

**JSON**(JavaScript Object Notation)一种轻量级的数据交换格式，具有良好的可读和便于快速编写的特性。可在不同平台之间进行数据交换。JSON采用兼容性很高的、完全独立于语言文本格式，同时也具备类似于C语言的习惯(包括C, C++, C#, Java, JavaScript, Perl, Python等)体系的行为

(1).可读性方面。
JSON和XML的数据可读性基本相同，JSON和XML的可读性可谓不相上下，一边是建议的语法，一边是规范的标签形式，XML可读性较好些。
(2).可扩展性方面。
XML天生有很好的扩展性，JSON当然也有，没有什么是XML能扩展，JSON不能的。
(3).编码难度方面。
XML有丰富的编码工具，比如Dom4j、JDom等，JSON也有json.org提供的工具，但是JSON的编码明显比XML容易许多，即使不借助工具也能写出JSON的代码，可是要写好XML就不太容易了。
(4).解码难度方面。
XML的解析得考虑子节点父节点，让人头昏眼花，而JSON的解析难度几乎为0。这一点XML输的真是没话说。
(5).流行度方面。
XML已经被业界广泛的使用，而JSON才刚刚开始，但是在Ajax这个特定的领域，未来的发展一定是XML让位于JSON。到时Ajax应该变成Ajaj(Asynchronous Javascript and JSON)了。
(6).解析手段方面。
JSON和XML同样拥有丰富的解析手段。
(7).数据体积方面。
JSON相对于XML来讲，数据的体积小，传递的速度更快些。
(8).数据交互方面。
JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互。
(9).数据描述方面。
JSON对数据的描述性比XML较差。
(10).传输速度方面。
JSON的速度要远远快于XML。

## static的作用

## 浏览器中的回流和重绘是什么,如何减少重绘和回流

重绘：当元素的外观或外观可见性（visibility）发生变化时会触发重绘
回流：render树中的部分或全部因为元素的规模尺寸、布局、隐藏等改变，需要重新计算render树。

 减少回流、重绘其实就是需要减少对render tree的操作，并减少对一些style信息的请求，尽量利用好浏览器的优化策略
   （1） 避免操作DOM，创建一个documentFragment或div，在它上面应用所有DOM操作，最后再把它添加到window.document。也可以在一个display:none的元素上进行操作，最终把它显示出来。因为display:none上的DOM操作不会引发回流和重绘。
   （2） 让要操作的元素进行"离线处理"，处理完后一起更新，这里所谓的"离线处理"即让元素不存在于render tree中。如读取offsetLeft等属性。
   （3） 尽可能在DOM树的末端改变class ，尽可能在DOM树的里面改变class，可以限制回流的范围，使其影响尽可能少的节点。
   （4） 避免设置多层内联样式，因为每一个都会造成回流，样式合并在一个外部类，这样当该元素的class属性被操作时，只会产生一个reflow。
   （5） 将需要多次回流的元素position属性设为absolute或fixed，这样该元素就会脱离文档流，它的变化不会影响其他元素变化。比如动画效果应用到position属性为absolute或fixed的元素上。
    （6） 牺牲平滑度换取速度，动画元素每次移动3像素可能在非常快的机器上看起来平滑度低了，但它不会导致CPU在较慢的机器和移动设备中抖动
    （7） 避免使用table布局，在布局完全建立之前，table需要很多关口，table是可以影响之前已经进入的DOM元素的显示的元素。即使一些小的变化和会导致table中所有其他节点回流

（8） 避免使用css的JavaScript表达式，该规则较过时，但是个好主意。因为每次都需要重新计算文档，或部分文档、回流

## 去哪网用的什么vue的版本

 Vue 全家桶：**vue2.5** + vuex + vue-router + webpack

 ES6

 网络请求：axios

## ajax如何缓存

## 移动端用什么内核

​	1.Trident:因为在早期IE占有大量的市场份额，所以以前有很多网页是根据这个Trident的标准来编写的，但是实际上这个内核对真正的网页标准支持不是很好，同时存在许多安全Bug。
　　2.Gecko:优点就是功能强大、丰富，可以支持很多复杂网页效果和浏览器扩展接口，缺点是消耗很多的资源，比如内存。
　　3.Webkit:优点就是Webkit拥有清晰的源码结构、极快的渲染速度，缺点是对网页代码的兼容性较低，会使一些编写不标准的网页无法正确显示。
　　4.Presto：Presto内核被称为公认的浏览网页速度最快的内核，同时也是处理JS脚本最兼容的内核，能在Windows、Mac及Linux操作系统下完美运行。

## 为什么使用2倍图

在Retina屏幕下，图片很容易出现模糊，原因就是它将原来的一个css像素点放大成了多个设备像素点。为了保证图片不出现模糊。我们需要对图片进行处理，来消除模糊现象。这就是传统意义上的二倍图，三倍图的产生。

## vue.prototype使用

你可能会在很多组件里用到数据/实用工具，但是不想[污染全局作用域。这种情况下，你可以通过在原型上定义它们使其在每个 Vue 的实例中可用。

```js
Vue.prototype.$appName = 'My App'
```

## js里字符串去重的办法

**方法一 for循环**

```js
function quchong1(str){
	var newStr="";
	var flag;
	for(var i=0;i<str.length;i++){
		flag=1;
		for(var j=0;j<newStr.length;j++){
			if(str[i]==newStr[j]){
				flag=0;
				break;
			}
		}
		if(flag)  newStr+=str[i];
	}
	return newStr; 
}
```

方法二：indexOf(无兼容问题)

```js
function quchong2(str){
	var newStr="";
	for(var i=0;i<str.length;i++){
		if(newStr.indexOf(str[i])==-1){
			newStr+=str[i];
		}
	}
	return newStr;
}
```

方法三：search()方法

```js
function quchong3(str){
	var newStr="";
	for(var i=0;i<str.length;i++){
		if(newStr.search(str[i])==-1)	
		newStr+=str[i];	
		}
	return newStr;
}
```

方法四：对象属性

```js
function quchong4(str){
	var obj={};
	var newStr="";
	for(var i=0;i<str.length;i++){
		if(!obj[str[i]]){
			newStr+=str[i];
			obj[str[i]]=1;
		}
	}
	return newStr;
}
```

## JS 实现一个闭包函数,每次调用都自增1;

```
var add = (function() {   
   // 声明一变量,由于下面 return所以变量只会声明一次  
   var count = 0;  
   return function() {    
    return console.log(count++);  
  }; 
})(); 
add(); // 0 
add(); // 1 
add(); // 2 
```

### 一个数组中 par中存放了多个人员的信息,每个人员的信息由 `name` 和 `age` 构成(`{name:'张三',age:15}`).请用 JS 实现年龄从小到大的排序;

```
var par = [{age:5,name:'张三'},{age:3,name:'李四'},{age:15,name:'王五'},{age:1,name:'随便'}]

var parSort = par.sort(function(a,b){
    return a.age - b.age;
})
```

