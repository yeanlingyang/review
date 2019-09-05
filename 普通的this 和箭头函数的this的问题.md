ES6中新增了箭头函数这种语法,箭头函数以其简洁性和方便获取this的特性,俘获了大批粉丝儿

它也可能是面试中的宠儿, 我们关键要搞清楚 箭头函数和普通函数中的this

## 一针见血式总结:

普通函数中的this:

1.this总是代表它的直接调用者(js的this是执行上下文), 例如 obj.func ,那么func中的this就是obj

2.在默认情况(非严格模式下,未使用 'use strict'),没找到直接调用者,则this指的是 window (约定俗成)

3.在严格模式下,没有直接调用者的函数中的this是 undefined

4.使用call,apply,bind(ES5新增)绑定的,this指的是 绑定的对象

 

箭头函数中的this

箭头函数没有自己的this, 它的this是继承而来; 默认指向在定义它时所处的对象(宿主对象),而不是执行时的对象, 定义它的时候,可能环境是window; 箭头函数可以方便地让我们在 setTimeout ,setInterval中方便的使用this

下面通过一些例子来研究一下 this的一些使用场景

 

要整明白这些, 我们需要首先了解一下作用域链:

当在函数中使用一个变量的时候,首先在本函数内部查找该变量,如果找不到则找其父级函数,

最后直到window,全局变量默认挂载在window对象下

 

1.全局变量默认挂载在window对象下

```javascript
<script>
 var aa = 2;
 alert(window.aa);
 (function () {
   aa = 3;
 })();
 alert(window.aa);
</script>
```

我们仅仅声明了一个全局变量aa,但是打印出window.aa却和aa保持一致,为什么呢?

眼见为实, 我们使用console.dir(window) 打印 window对象看看

![img](https://images2018.cnblogs.com/blog/1434273/201807/1434273-20180709183137275-206269007.png)

我们可以看到在window属性中,看到 aa 属性了;此外,函数也适用于此情况,全局函数也会挂在在window对象下

我们常见的window的属性和方法有: alert, location,document,parseInt,setTimeout,setInterval等,window的属性默认可以省略window前缀!

2.在普通函数中,this指向它的直接调用者;如果找不到直接调用者,则是window

我们来看一些![img](file:///C:\Users\YANGYA~1\AppData\Local\Temp\SGPicFaceTpBq\17144\4F090050.png)

1. ```javascript
   <script>
    function test() {
      console.log(this);
    }
    test();
   </script>
   ```

结果是: window 

原因: test()是一个全局函数,也就是说是挂在window对象下的,所以test()等价于 window.test() ,所以此时的this是window

示例2:

```javascript
<script>
 var obj = {
   say: function () {
     setTimeout(function () {
       console.log(this)
     });
   }
 }
 obj.say();
</script>
```

结果是: window

匿名函数,定时器中的函数,由于没有默认的宿主对象,所以默认this指向window

问题: 如果想要在setTimeout/setInterval中使用这个对象的this引用呢?

用一个 变量提前把正确的 this引用保存 起来, 我们通常使用that = this, 或者 _this = this来保存我们需要的this指针!

```javascript
1. <script>
2. var obj = {
3. func: function() {},
4. say: function () {
5. var that = this;   //此时的this就是obj对象
6. setTimeout(function () {
7. console.log(this)
8. that.func()
9. });
10. }
11. }
12. obj.say();
13. </script>

```



我们也可以使用 func.bind(this) 给回调函数直接绑定宿主对象, bind绑定宿主对象后依然返回这个函数, 这是更优雅的做法

![img](https://images2018.cnblogs.com/blog/1434273/201807/1434273-20180709183303486-2093241534.png)

示例3(改变自360面试题):

 

```javascript
1. window.val = 1;
2. var obj = {
3. val: 2,
4. dbl: function () {
5. this.val *= 2;
6. val *= 2;
7. console.log(val);
8. console.log(this.val);
9. }
10. };
11. // 说出下面的输出结果
12. obj.dbl();
13. var func = obj.dbl;
14. func();

结果是:  2   4    8   8

```

结果是:  2   4    8   8 

<1> 12行代码调用

val变量在没有指定对象前缀,默认从函数中找,找不到则从window中找全局变量

即 val *=2 就是 window.val *= 2

this.val默认指的是 obj.val ;因为 dbl()第一次被obj直接调用

<2>14行代码调用

func() 没有任何前缀,类似于全局函数,即  window.func调用,所以

第二次调用的时候, this指的是window, val指的是window.val

第二次的结果受第一次的影响

3.在严格模式下的this

```javascript
1. <script>
2. function test() {
3. 'use strict';
4. console.log(this);
5. }
6. test();
7. </script>

```

结果是: undefined

4.箭头函数中的 this

```javascript
1. <script>
2. var obj = {
3. say: function () {
4. setTimeout(() => {
5. console.log(this)
6. });
7. }
8. }
9. obj.say(); // obj
10. </script>

```

此时的 this继承自obj, 指的是定义它的对象obj, 而不是 window!

示例(多层嵌套的箭头函数):

```javascript
1. <script>
2. var obj = {
3. say: function () {
4. var f1 = () => {
5. console.log(this); // obj
6. setTimeout(() => {
7. console.log(this); // obj
8. })
9. }
10. f1();
11. }
12. }
13. obj.say()
14. </script>

```

因为f1定义时所处的函数 中的 this是指的 obj, setTimeout中的箭头函数this继承自f1, 所以不管有多层嵌套,都是 obj

示例(复杂情况: 普通函数和箭头函数混杂嵌套)

```javascript
1. <script>
2. var obj = {
3. say: function () {
4. var f1 = function () {
5. console.log(this); // window, f1调用时,没有宿主对象,默认是window
6. setTimeout(() => {
7. console.log(this); // window
8. })
9. };
10. f1();
11. }
12. }
13. obj.say()
14. </script>

```

结果: 都是 window,因为 箭头函数在定义的时候它所处的环境相当于是window, 所以在箭头函数内部的this函数window

示例(严格模式下的混杂嵌套)

```javascript
1. <script>
2. var obj = {
3. say: function () {
4. 'use strict';
5. var f1 = function () {
6. console.log(this); // undefined
7. setTimeout(() => {
8. console.log(this); // undefined
9. })
10. };
11. f1();
12. }
13. }
14. obj.say()
15. </script>

```

结果都是undefined

说明: 严格模式下,没有宿主调用的函数中的this是undefined!!!所以箭头函数中的也是undefined!

总结:

使用箭头函数,可以让我们解决一些在匿名函数中 this指向不正确的问题; 但是要注意在和普通函数混合的时候,this的指向可能是window !

Y(^o^)Y, 掌握这么多已经足够面试绝大部分关于this的内容了,我们在开发中的应用也没问题了!