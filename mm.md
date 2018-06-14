### 前端面试题总结

1. 小程序的生命周期

生命周期是指一个小程序从创建到销毁的一系列过程.在小程序中 ，通过App()来注册一个小程序 ，通过Page()来注册一个页面.

```
app():
onLaunch 生命周期函数--监听小程序初始化 当小程序初始化完成时，会触发 onLaunch（全局只触发一次）
onShow 生命周期函数--监听小程序显示 当小程序启动，或从后台进入前台显示，会触发 onShow
onHide 生命周期函数--监听小程序隐藏 当小程序从前台进入后台，会触onHide
onError 错误监听函数 当小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息

其他 Any 开发者可以添加任意的函数或数据到 Object 参数中，用 this 可以访问
ps:onLaunch,onShow会返回一个对象，包含三个参数，path:打开小程序的路径，query:打开小程序页面url携带的参数，scene:场景值。
page():
onLoad 生命周期函数--监听页面加载

onReady 生命周期函数--监听页面初次渲染完成

onShow 生命周期函数--监听页面显示

onHide 生命周期函数--监听页面隐藏

onUnload 生命周期函数--监听页面卸载

ps:小程序为我们提供了全局数据管理 ，在page页面中通过getApp()方法获取app.js实例
注意：
App() 必须在 app.js 中注册，且不能注册多个。
不要在定义于 App() 内的函数中调用 getApp() ，使用 this 就可以拿到 app 实例。
不要在 onLaunch 的时候调用 getCurrentPages()，此时 page 还没有生成。
通过 getApp() 获取实例之后，不要私自调用生命周期函数。
```



2. vue的双向绑定原理

```
vue数据双向绑定是通过数据劫持结合发布者-订阅者模式的方式来实现的，

数据劫持（通过object.defineproperty()定义访问器属性）改变数据存取的默认行为。有三个参数，对象，属性，描述符对象。

DocumentFragment（文档片段）可以看作节点容器，它可以包含多个子节点，当我们将它插入到 DOM 中时，只有它的子节点会插入目标节点，所以把它看作一组节点的容器。使用 DocumentFragment 处理节点，速度和性能远远优于直接操作 DOM。Vue 进行编译时，就是将挂载目标的所有子节点劫持（真的是劫持，通过 append 方法，DOM 中的节点会被自动删除）到 DocumentFragment 中，经过一番处理后，再将 DocumentFragment 整体返回插入挂载目标。

  订阅发布模式（又称观察者模式）定义了一种一对多的关系，让多个观察者同时监听某一个主题对象，这个主题对象的状态发生改变时就会通知所有观察者对象。
  发布者发出通知 => 主题对象收到通知并推送给订阅者 => 订阅者执行相应操作
```



3. 数组去重

```
JS实现数组去重方法总结(六种方法)
1.es6的set()，自动去重
2.
双层循环，外层循环元素，内层循环时比较值
如果有相同的值则跳过，不相同则push进数组
3.利用splice直接在原数组进行操作
双层循环，外层循环元素，内层循环时比较值
值相同时，则删去这个值
4.利用对象的属性不能相同的特点进行去重
Array.prototype.distinct = function (){
 var arr = this,
  i,
  obj = {},
  result = [],
  len = arr.length;
 for(i = 0; i< arr.length; i++){
  if(!obj[arr[i]]){ //如果能查找到，证明数组元素重复了
   obj[arr[i]] = 1;
   result.push(arr[i]);
  }
 }
 return result;
};
var a = [1,2,3,4,5,6,5,3,2,4,56,4,1,2,1,1,1,1,1,1,];
var b = a.distinct();
console.log(b.toString()); //1,2,3,4,5,6,56
5.数组递归去重
运用递归的思想
先排序，然后从最后开始比较，遇到相同，则删除
6.利用indexOf以及forEach
```



4. var a=[1,2,3];

​       var b=a;

​             a=[0,2,3];

​     console.log(b);？//输出？

​      var a=[1,2,3];

​      var b=a;

​            a.push(3);

​            console.log(b);？//输出？

```
答：1. var a=[1,2,3];
       var b=a;
       a=[0,2,3];//开辟了一个新的内存，所以b还是指向原来的内存的对象
       console.log(b);//[1,2,3]
   2.var a=[1,2,3];
     var b=a;
     a.push(3);
     console.log(b);//[1,2,3,3]
  浅拷贝，引用类型值，引用类型的值是保存在内存中的对象，javas不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间，当从一个变量向另一个变量复制引用类型的值时，同样也会将存储在变量对象中的值复制一份放到新变量分配的空间。不同的是，这个值的副本实际是一个指针，而这个指针指向存储在堆中的一个对象。复制操作结束后，两个变量实际上将引用同一个对象。因此改变其中一个变量就会影响另一个变量。
  

```

4.1. var a=[1,2,3];
​       var b=a;
​       a.push(3);
​       console.log(b);//[1,2,3,3]
  怎么解决让b=[1,2,3],原来的a,换句话是怎样实现深拷贝？


    var a = [1, 2, 3];
    function getType(obj) {
      //tostring会返回对应不同的标签的构造函数
      var toString = Object.prototype.toString;
      var map = {
        "[object Boolean]": "boolean",
        "[object Number]": "number",
        "[object String]": "string",
        "[object Function]": "function",
        "[object Array]": "array",
        "[object Date]": "date",
        "[object RegExp]": "regExp",
        "[object Undefined]": "undefined",
        "[object Null]": "null",
        "[object Object]": "object"
      };
      if (obj instanceof Element) {
        return "element";
      }
      return map[toString.call(obj)];
    }
    function deepClone(data) {
      var type = getType(data);
      var obj;
      if (type === "array") {
        obj = [];
      } else if (type === "object") {
        obj = {};
      } else {
        //不再具有下一层次
        return data;
      }
      if (type === "array") {
        for (var i = 0, len = data.length; i < len; i++) {
          obj.push(deepClone(data[i]));
        }
      } else if (type === "object") {
        for (var key in data) {
          obj[key] = deepClone(data[key]);
        }
      }
      return obj;
    }
    var b = deepClone(a);
    a.push(3);
    console.log(b);

5. 变量提升

   ```
   JavaScript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。

   JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。

   JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。

   变量提升即将变量声明提升到它所在作用域的最顶部。

   函数提升：函数声明提升到最顶部。优先于变量提升。

   作用域：JavaScript作为编程语言，最基本的功能之一就是能够存储变量当中的值。而一套设计良好的用来存储变量，并且之后可以方便地找到这些变量的规则，被称为作用域。负责收集并维护由所有声明的标识符（变量）组成的一系列查询，并实施一套非常严格的规则，确认当前执行的代码对这些标识符的访问权限。
   ```

6.  class 

   ```
   1.继承：extend
      super:子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承了父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。
   所以在constructor方法内this不能放在super前。
      可继承原生的构造函数。
      子类的_proto_属性表示构造函数的继承，总是指向父类。
      子类prototype属性的_proto_属性表示方法的继承总是指向prototype属性。
    2.特殊情况
      子类继承object类，不存在任何继承，继承null.
    3.Object.getPrototypeOf()可用于从子类上获取父类。
    4. 子类实例的_proto_属性的_proto_属性指向父类实例的_proto_属性。子类的原型的原型是父类的原型。因此，通过子类实例的 _proto_属性的_proto_属性可以修改父类实例的行为。,
    
   ```

7.  set/map

   ```
   set()自动去重

   map():它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。
   ```


7.  vue组件间传值与小程序组件间传值

```
vue组件间传值：
1.父子之间传值通讯：proper
2.子传父：父组件可以使用 v-on监听子组件上 $emit 的变化。这可以允许你很方便的添加事件显性。
this.$emit(event,...args);
/*
* event: 要触发的事件
* args: 将要传给父组件的参数
*/ 
Usage： 
子组件内容：

<template>
<div @click="iclick"></div>
</template>
methods:{
    iclick(){
        let data = {
            a:'data'
        };
        this.$emit('ievent',data,'lalala');
    }
}
父组件内容：

<i-template @ievent = "ievent"></i-template>
methods:{
    ievent(...data){
        console.log('allData:',data);// data为包含传过来所有数据的数组，第一个元素是对象，第二个元素是字符串
    }
}
3.非父子：简单情况下我们可以通过使用一个空的Vue实例作为中央事件总线，管理组件间的通信：
// 将在各处使用该事件中心
// 组件通过它来通信
var eventHub = new Vue()
然后在组件中，可以使用 $emit , $on, $off 分别来分发、监听、取消监听事件：
4.vuex:专用的状态管理层
小程序组件间传值：
```

8. 跨域方式

```
1.跨域资源共享（CORS）
基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。
<script type="text/javascript">
    var xhr = new XMLHttpRequest();
    xhr.open("￼GET", "/trigkit4",true);
    xhr.send();
</script>
以上的trigkit4是相对路径，如果我们要使用CORS，相关Ajax代码可能如下所示：

<script type="text/javascript">
    var xhr = new XMLHttpRequest();
    xhr.open("￼GET", "http://segmentfault.com/u/trigkit4/",true);
    xhr.send();
</script>
代码与之前的区别就在于相对路径换成了其他域的绝对路径，也就是你要跨域访问的接口地址。

服务器端对于CORS的支持，主要就是通过设置Access-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。
 CORS支持所有类型的HTTP请求。
 使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。
 JSONP主要被老的浏览器支持，它们往往不支持CORS，而绝大多数现代浏览器都已经支持了CORS）。
2.jsonp
用js动态生成script标签,通过script标签引入一个js文件，这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。请求完毕后可以通过调用callback的方式回传结果。所以jsonp是需要服务器端的页面进行相应的配合的。它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况.
3.通过修改document.domain来跨子域
4.使用window.name来进行跨域
5.使用HTML5中新引进的window.postMessage方法来跨域传送数据
6.hash + iframe
在文章最开始提到过 iframe 标签也是不受同源策略限制的标签之一，hash + iframe 的跨域核心思想就是，在 A 源中通过动态改变 iframe 标签的 src 的哈希值，在 B 源中通过 window.onhashchange 来捕获到相应的哈希值。思路不难直接上代码：
7.WebSockets
WebSockets 属于 HTML5 的协议，它的目的是在一个持久连接上建立全双工通信。由于 WebSockets 采用了自定义协议，所以优点是客户端和服务端发送数据量少，缺点是要额外的服务器。
8.反向代理
9.vue-cli proxy代理

```

9. 箭头函数与普通函数的区别，箭头函数作用，里面的this指向哪里

```
1.比普通函数代码量更少，更简洁明了。
2.箭头函数的this指向的对象就是定义时所在的对象，不是使用时所在的对象。普通函数的this指向的对象是基于函数的执行环境绑定的，在全局函数中，this等于window.而当函数被作为某个对象的方法调用时，this等于那个对象。匿名函数的执行环境具有全局性，因此其this对象通常指向window.
3.普通函数this对象的指向是可变的，但是箭头函数中是固定的。因此，普通函数的apply，call不能改变他的指向。
4.箭头函数不可以当作构造函数，不可以使用new命令。
5.不可以使用argumengt对象，该对象在函数体内不存在。如果要用，可以用rest参数代替。
6.不可使用yield命令，因此箭头函数不能用作generrator函数。
```

10. 把一个数组乱序

```
var a = [1, 2, 3, 4, 5, 6, 7, 8, 9];

var random = function(array) {

    return array.sort(function(){return Math.random() > 0.5});

};

console.log(random(a)); 

```

11. sort()参数有哪些怎么用

```
arrayObject.sort(sortby)
参数	描述
sortby	可选。规定排序顺序。必须是函数。
```

12. 深拷贝和浅拷贝

​       深拷贝：对于对象来说是复制一个对象到另一个新开辟的内存中， 两者的     修改值互不影响。

​       浅拷贝：对于对象来说，只是复制了对象的引用，指针还是指向同一个堆里面存储的对象。

13. static 函数 

    ```
    class的静态方法，加上static 表示该方法不会被实例继承，而是直接通过类调用。父类的静态方法可以被子类继承。可以从super调用。 
    ```

14. ​

15. 单例模式？

    ```
    传统单例模式

    　　保证一个类仅有一个实例，并提供一个访问它的全局访问点。

    实现单例核心思想

    　　无非是用一个变量来标志当前是否已经为某个类创建过对象，如果是，则在下一次获取该类的实例时，直接返回之前创建的对象，接下来我们用JavaScript来强行实现这个思路。
    ```

16. class 中的instance？？？

17. ctx.drawImage(图片，图片剪裁起始坐标x,图片剪裁起始坐标y,被剪裁的图片的宽度，被剪裁的图片的高度，放置在画布的坐标x，放置在画布的坐标y,放置的图片的宽，放置的图片的高)

18. 定义一个变量时不要跟类取一样的名字。会报错，eg:

    ```
        let b=new b();//error

           let B=new b();//对的

          微信浏览器只认识小写的后缀名

    ```

    ​

19. promise

20. vue 适配

    ```
    在项目根目录的index.html 头部加入手机端适配的meta的代码
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    flex+rem或者安装插件lib-flexible+px2rem-loader
    ```

21. 原型/原型链

    ```
    我们创建的每个函数都有一个prototype（原型）属性，这个属性是一个指针，指向一个对象，就是原型对象，而这个对象里面的属性和方法可以让所有的对象实例共享。prototype就是通过调用构造函数而创建的那个对象实例的原型对象。 
    ```

22. vue生命周期

    ![img](https://img-blog.csdn.net/20180104113902663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ1MTE1Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

    ![这里写图片描述](https://img-blog.csdn.net/20180104113943794?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ1MTE1Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

23. vue组件中的updated，和watch那个会在组件更新的时候先触发

    ```
    updated钩子它的官方文档解释就是：组件更新之后，那么顾名思义，当前组件的数据发送变化的时候（任何一个当前组件data中的数据）就会触发updated。

    watch呢它主要用于监听一个数据，基本用在一个数据影响多个数据的情况下，这里不得不提到computed计算属性了，它正好和watch相反，是一个数据受多个数据影响；watch监听数据如果不设置“deep”深度监听的话，是无法监听到对象内部的数据变化。

    在触发的先后上，watch会先触发，因为watch触发是dom还未更新，updated实在组件更新之后，所以在dom更新后才会触发。

    所以显而易见，updated一般用在比如用户信息的任何一个信息发生改变，就更新其他地方的信息等等，而watch则是当某个数据改变会触发某些事件或某些改变等等，那么我们用它们的时候就要根据具体需求而定咯。
    ```

24. vue路由的实现原理

    https://segmentfault.com/a/1190000011967786

25. 问了一下es6 import模块导入问题例如：

    ```
     import {Button,Select} from 'element-ui'

     有前文可知import会先转换为commonjs即

              var a=require('element-ui');

              var b=a.Button;

              var c=a.Select;有此可知element-ui全部被引进了；

              所以babel-plugin-component就做了一件事将  import  {Button,Select} from 'element-ui'  转换成了

              import Button from "element-ui"; ui/lib/button

              import Select  from "element-ui"; ui/lib/select

    ```

26. Js 存储问题

    ```
    javaScript有三种数据存储方式，分别是：

    sessionStorage

    localStorage

    cookier

    相同点：都保存在浏览器端，同源的

    不同点：

    ①传递方式不同

    cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。

    sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。

    ②数据大小不同

    cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。
    存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。

    sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

    ③数据有效期不同

    sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；

    localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；

    cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

    ④作用域不同

    sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；

    localStorage 在所有同源窗口中都是共享的；

    cookie也是在所有同源窗口中都是共享的。

    Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。
    Web Storage 的 api 接口使用更方便。
    ```

27. Gulp和Webpack前端自动化工具

28. 流式布局，自适应布局，响应式布局，弹性布局

    ```
    流式布局：页面元素的宽度按照屏幕分辨率进行适配调整，但整体布局不变。网页中主要的划分区域的尺寸用百分数。搭配min，max使用。
    自适应布局：分别为不同的屏幕分辨率定义布局，即创建多个静态布局，每个静态布局对应一个屏幕分辨率范围。改变屏幕分辨率可以切换不同的静态局部。使用@media媒体查询给不同尺寸和介质的设备切换不同的样式。
    弹性布局：使用rem/em单位进行相对布局。
    响应式布局：流式布局+媒体查询+弹性布局。
    ```

29. cookie session区别

30. 数据库的一些数据查询问题

31. 两个变量在不借助第三方变量的帮助下实现两个值交换，和一些物体碰撞算法。

32. 两个排序算法  

33. javascrip原型链的用法和作用，对闭包的理解，以用闭包的作用

34. 堆数据结构

35. promise

36. css盒子模型

37. http请求整个流程

38. 从地址栏输入地址到展现发生什么

39. web安全

40. react和vue有哪些不同 说说你对这两个框架的看法

    ```
    都用了virtual dom的方式, 性能都很好

    ui上都是组件化的写法，开发效率很高

    vue是双向数据绑定，react是单项数据绑定，当工程规模比较大时双向数据绑定会很难维护

    vue适合不会持续的  小型的web应用，使用vue.js能带来短期内较高的开发效率. 否则采用react
    ```

41. let和const的区别

42. 平时用了es6的哪些特性，体验如何 和es5有什么不同

43. 介绍一下你对webpack的理解，和gulp有什么不同

44. webpack打包速度慢，你觉得可能的原因是什么，该如何解决

45. http响应中content-type包含哪些内容

46. 浏览器缓存有哪些，通常缓存有哪几种方式

47. 如何取出一个数组里的图片并按顺序显示出来

48. 使用模块化加载时，模块加载的顺序是怎样的，如果不知道，根据已有的知识，你觉得顺序应该是怎么样的

49. 介绍一下闭包和闭包常用场景

50. 为什么会出现闭包这种东西，解决了什么问题

51. 介绍一下你所了解的作用域链,作用域链的尽头是什么，为什么

52. 一个Ajax建立的过程是怎样的，主要用到哪些状态码

53. 使用promise封装

54. 知道语义化吗？说说你理解的语义化，如果是你，平时会怎么做来保证语义化

55. 说说content-box和border-box，为什么看起来content-box更合理，但是还是经常使用border-box

56. 介绍一下HTML5的新特性

57. 事件委托

58. 实现三个DIV等分排布在一行（考察border-box）

59. 说说你知道JavaScript的内存回收机制

60. 函数防抖和函数节流

61. 编程实现输出一个数组中第N大的数据实现两栏布局有哪些方法

62. 设置width的flex元素，flex属性值是多少

63. get和post有什么不同

64. 判断链表是否有环

65. 输出二叉树的最小深度

66. javaScript中的this是什么，有什么用，它的指向是什么

67. 全局代码中的this  是指向全局对象

68. 作为对象的方法调用时指向调用这个函数的对象。

69. 作为构造函数指向新创建的对象

70. 使用apply和call设置this

71. 写一个快速排序

72. 怎么实现从一个DIV左上角到右下角的移动，有哪些方法，都怎么实现

73. 简单介绍一下promise，他解决了什么问题

74. 写一个组合继承

75. 判断数组有哪些方法

    ```
    a instanceof Array

    a.constructor == Array

    Object.prototype.toString.call(a) == [Object Array]

    ```

76. 多页面通信有哪些方案，各有什么不同

77. 用Node实现一个用户上传文件的后台服务应该怎么做

    ```
    multer模块
    ```

78. XSS和CSRF攻击

    ```
    xss：比如在一个论坛发帖中发布一段恶意的JavaScript代码就是脚本注入，如果这个代码内容有请求外部服务器，那么就叫做XSS

    写一个脚本将cookie发送到外部服务器这就是xss攻击但是没有发生csrf

    防范：对输入内容做格式检查 输出的内容进行过滤或者转译

    CSRF：又称XSRF，冒充用户发起请求（在用户不知情的情况下）,完成一些违背用户意愿的请求 如恶意发帖，删帖

    比如在论坛发了一个删帖的api链接 用户点击链接后把自己文章给删了 这里就是csrf攻击没有发生xss

    防范：验证码 token 来源检测

    ```

79. 圣杯布局和双飞翼布局

80. 图片预加载和懒加载

81. UMD规范和ES6模块化，Commonjs的对比

82. 前端性能优化

83. 将url的查询参数解析成字典对象

84. 两个数组合并成一个数组排序返

85. 如何解决ajax无法后退的问题

    ```
    HTML5里引入了新的API

    即：history.pushState, history.replaceState

    可以通过pushState和replaceState接口操作浏览器历史，并且改变当前页面的URL。

    1. onpopstate监听后
    2. 实现一个once函数
    3. 分域名请求图片的原因和好处

    浏览器的并发请求数目限制是针对同一域名的，超过限制数目的请求会被阻塞

    浏览器并发请求有个数限制，分域名可以同时并发请求大量图片

    页面的加载顺序

    html顺序加载，其中js会阻塞后续dom和资源的加载，css不会阻塞dom和资源的加载但是会阻塞js的加载。

    浏览器会使用prefetch对引用的资源提前下载

    1.没有 defer 或 async，浏览器会立即加载并执行指定的脚本

    2.有 async，加载和渲染后续文档元素的过程将和 script.js 的加载与执行并行进行(下载异步，执行同步，加载完就执行)。

    3.有 defer，加载后续文档元素的过程将和 script.js 的加载并行进行（异步），但是 script.js 的执行要在所有元素解析完成之后，DOMContentLoaded 事件触发之前完成。
    ```

86. 计算机网络的分层概述

    ```
    tcp/ip模型：从下往上分别是链路层，网络层，传输层，应用层

    osi模型：从下往上分别是物理层，链路层，网络层，传输层，会话层，表示层，应用层。
    ```

87. jscss缓存问题

    ```
    浏览器缓存的意义在于提高了执行效率，但是也随之而来带来了一些问题，导致修改了js、css，客户端不能更新

    都加上了一个时间戳作为版本号

    <script type=”text/javascript” src=”{JS文件连接地址}?version=XXXXXXXX”></script>
    ```

88. setTimeout,setInterval,requestAnimationFrame之间的区别

89. webpack常用到哪些功能

    ```
    设置入口  设置输出目 设置loader  extract-text-webpack-plugin将css从js代码中抽出并合并 处理图片文字等功能 解析jsx解析bable
    ```

90. 介绍sass

    ```
    &定义变量 css嵌套 允许在代码中使用算式 支持if判断for循环
    ```

91. websocket和ajax轮询

    ```
    Websocket是HTML5中提出的新的协议，注意，这里是协议，可以实现客户端与服务器端的通信，实现服务器的推送功能。

    其优点就是，只要建立一次连接，就可以连续不断的得到服务器推送的消息，节省带宽和服务器端的压力。

    ajax轮询模拟长连接就是每个一段时间（0.5s）就向服务器发起ajax请求，查询服务器端是否有数据更新

    其缺点显而易见，每次都要建立HTTP连接，即使需要传输的数据非常少，所以这样很浪费带宽
    ```

92. tansition和margin的百分比根据什么计算

    ```
    transition是相对于自身,margin相对于参照物
    ```

93. es6的深度克隆

    ```
    IOS移动端click事件300ms的延迟响应

    一些情况下对非可点击元素如(label,span)监听click事件，ios下不会触发，css增加cursor:pointer就搞定了
    ```

94. 移动端兼容问题

95. js基本类型及判断方法

96.  meta属性

97. 三次握手四次挥手

98. 1.浏览器内核，原理

    ```
    又称渲染引擎，主要负责html,css的解析，页面布局，渲染和复合层合成。浏览器内核的不同最主要的问题是对css的支持度与属性表现差异。
    ```

    2.div居中，垂直居中

    ```
    1.flex
    父节点：
    father{
      display:flex;
      justify-content:center;
      align-items:center;
    }
    2.div{
      position:absolute;
      top:0;
      left:0;
      bottom:0;
      right:0;
      margin:auto;
    }
    3.
    <p class="outerBox calc">
        </p><p class="innerBox">calc</p>
    <p></p>
    ```


    /*绝对定位，clac计算位置*/
    .calc{
      position: relative;
    }
    .calc .innerBox{
      position: absolute;
      left:-webkit-calc((500px - 200px)/2);
      top:-webkit-calc((120px - 50px)/2);
      left:-moz-calc((500px - 200px)/2);
      top:-moz-calc((120px - 50px)/2);
      left:calc((500px - 200px)/2);
      top:calc((120px - 50px)/2);
    }
    4.
    ​```
    
    3.flex盒模型
    
    Flexbox 由 伸缩容器 和 伸缩项目 组成。通过设置元素的 display 属性为 flex 或 inline-flex 可以得到一个伸缩容器。设置为 flex 的容器被渲染为一个块级元素，而设置为 inline-flex 的容器则渲染为一个行内元素。
    主轴+交叉轴。
    
    #### 兼容性：
    
    ie10(-ms形式)，Chrome 22+, Opera 12.1+, 和 Opera 1.  1.  Mobile 12.1+
    
    1. 安卓
    
       2.3 开始就支持旧版本 display:-webkit-box;
       4.4 开始支持标准版本 display: flex;
    
    2. IOS 
    
       6.1 开始支持旧版本 display:-webkit-box;
       7.1 开始支持标准版本display: flex;
    
    3. PC 
    
       ![img](https://img-blog.csdn.net/20160922210759487)
    
    #### 容器的属性：
    
    1. flex-direction: row | row-reverse | column | column-reverse;//方向。
    
    2. justify-content: flex-start | flex-end | center | space-between | space-around;主轴对齐方式。
    
    3. align-items: flex-start | flex-end | center | baseline | stretch;交叉轴对齐方式
    
    4. align-content: flex-start | flex-end | center | space-between | space-around | stretch;定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
    
    5. flex-wrap: nowrap | wrap | wrap-reverse;换行。
       6.flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
    
       #### 项目属性：
    
       order
       flex-grow
       flex-shrink
       flex-basis：属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
       flex：flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto
       align-self：允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
    
    4.css选择器，可以继承的属性
    
    | 选择符类型                                    | 例子                                       | 例子描述                                     |
    | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
    | 通用选择器                                    | *                                        |                                          |
    | 类别选择器(.class)                            | `.intro`                                 | 选择class=”intro”的所有元素                     |
    | ID选择器(#id)                               | `#first`                                 | 选择id=”first”的所有元素                        |
    | 标签选择器(element)                           | `div`                                    | 选择所有`<div>`标签                            |
    | 后代选择器(element element)                   | `div p`                                  | 选择`<div>`元素内部的所有`<p>`元素                  |
    | 子选择器(element>element)                    | `div>p`                                  | 选择父元素为`<div>`的所有`<p>`元素                  |
    | 群组选择器(element,element)                   | `div,p`                                  | 选择所有`<div>`和`<p>`元素                      |
    | 相邻同胞选择器(element+element)                 | `div+p`                                  | 选择紧接在`<div>`之后的所有`<p>`元素                 |
    | 伪类选择器(`:link :visited :active :hover :focus :first-child`) | `a:link` `a:visited` `a:active` `a:hover` `input:focus` `p:first-child` | （选择所有未被访问的或所有已被访问的或活动的链接）（选择鼠标指针位于其上链接）（选择获得的焦点的input 元素） |
    | 伪元素选择器(`:first-letter` `:first-line` `:before` `:after` `:lang(language)`) | `p:first-letter` `p:first-line` `p:before` `p:after``p:lang(it)` | 选择每个 元素的首字母；选择每个 元素的首行；在每个 元素的内容之前插入内容；在每个 元素的内容之后插入内容；选择带有以 “it” 开头的 lang 属性值的每个 元素 |
    | 属性选择器(`[attribute] [attribute=value] [attribute~=value] [attribute|=value]` ) | `[target=_blank]`                        | [attribute~=value]选择包含一个以空格分隔的词为value的所有元素；[attribute\|=value]选择属性的值等于value，或值以 value- 开头的所有元素 |
    
    css继承特性主要是指文本方面的继承，盒模型相关的属性基本没有继承特性。 
    **不可继承的：** 
    *display、margin、border、padding、background、height、min-height、max-height、width、min-width、max-width、overflow、position、top、bottom、left、right、z-index、float、clear、 table-layout、vertical-align、page-break-after、page-bread-before和unicode-bidi。* 
    **所有元素可继承的：** 
    *visibility和cursor* 
    **终极块级元素可继承的：** 
    *text-indent和text-align* 
    **内联元素可继承的：** 
    *letter-spacing、word-spacing、white-space、line-height、color、font、font-family、font-size、font-style、font-variant、font-weight、text-decoration、text-transform、direction* 
    **列表元素可继承的：** 
    *list-style、list-style-type、list-style-position、list-style-image*
    
    5.cookie/localstorage/sessionstorage区别
    
    ​```
    
    ​```
    
    6.vue 绑定原理
    
    7.封装组件
    
    8.元素可绑定多个事件
    
    9.不用border怎么写直线
    
    10.vue 刷新?
    
    11.vue 生命周期
    
    12.vue 写一个页面，数据列表，随机两条数据交换位置，怎么交换？第一页的数据的第四条放到第64位？
    
    13.全屏品字布局
    
    14.帧动画？
    
    15.事件流？冒泡？捕获？事件委托？
    
     16.解构赋值用在哪里？
    
    17.作用域
    
    18.块级作用域怎样形成？es5/es6
    
    19.es5继承/es6继承 异同
    
    20.改变this的指向的方法,以及其区别
    
    21.301，302，303，304怎样发生的
    
    22.ajax原生js写出来，原理
    
    23.移动端和pc端差异
    
    24.vue 移动端 border 1px
    
    25.web安全性能方面，预防措施，具体到怎么写代码
    
    ​```
    服务攻击：针对程序漏洞进行攻击常见的有：
    SQL注入攻击
    文件上传攻击
    XSS跨站脚本攻击
    CSRF（Cross-site request forgery）跨站请求伪造
    程序逻辑漏洞
    暴力攻击：如DDOS，穷举密码等
    C段攻击 某台服务器被攻陷后通过内网进行ARP、DNS等内网攻击
    社会工程学 收集管理员等个人信息等资料尝试猜测其密码或者取得信任等
    最基本原则
    
    永远不要相信用户提交上来的数据（包括header\cookie\seesionId），都可能造假
    
    sql注入
    
    原理：
    
    后台的SQL语句通过字符串拼接后后导致执行各种意想不到的语句，配合sql语句的outfile功能，还能写入一个脚本文件到服务器。
    
    例子：
    
    比如根据用户post来的ID来列出单个人的信息 假设用户提交的id=1拼接出来为select * from table where id=1 这时执行正常假设用户提交的为id=1 and 1=1拼接出来为select * from table where and 1=1 这时执行异常，所以内容都被查出来了
    又如根据用户提交的job_rul=xxx来拼接，但是攻击者提交了job_rul=sql' or 1=1 limit 1--
    //正常的语句
    select * from job_info where job_url='sql' limt 10
    //攻击者提交的 “sql' or 1=1 limit 1-- ”后 变成下面这样 注意--后面有空格
    select * from job_info where job_url='sql' or 1=1 limit 1-- '  limt 10
    //上面语句后面 -- 和后面的内容 都表示注释
    //故只会执行 select * from job_info where job_url='sql' or 1=1
    //所有信息都被查出来了
    预防：
    
    在SQL拼接时注意过滤替换用户输入的字符串(但是本人不推荐这种方式)
    最安全的是mysql的预编译prepare，PHP的PDO将其实现了。因为语句先编译好了，用户怎么传都是一个参数。
    文件上传攻击
    
    没有过滤就接收用户上传的文件(如.php等文件)或者没有给文件重命名，最后通过攻击者通过web请求导致恶意脚本调用被执行
    
    常见漏洞
    
    CGI漏洞 IIS7.5以下，nginx < 0.8.x通过123.jpg/1.php可以把 jpg当php执行，原理是其解析向前归递造成的，有人问jpg文件怎么当脚本运行，其实是图片里可以插入程序代码的，具体实现就不透露了
    apache,IIS解析漏洞123.php.jpg apache当成php来运行（IIS6,实测apache2.2.xx有这个问题，2.4.xx已经修复）
    攻击原理：攻击者上传带有恶意脚本的123.jpg到网站，获得了http://url.com/123.jpg这个文件路径，然后通过http://url.com/123.jpg/1.php触发123.jpg所带的程序代码
    
    预防:
    
    用户上传的文件要重命名，这样的话即便攻击者上传了.php,.sh等脚本上来也会被重命名成指定的文件类型(如jpg)导致无法被攻击者执行
    确保使用的apache nginx等版本到最稳定的安全版本。
    XSS攻击
    
    原理：
    
    反射型： 比如根据$_GET['username']显示到网页上 有可能被人username=<script src='xxx'></script>注入代码发给用户点击从而被攻击.也有可能JS读取了URL部分字段导致的储存型：通过发表文章等把代码存入数据库别人看的时候没过滤就执行出来。危害：盗取管理员密码 cookie 控制用户ajax等。预防：入库过滤和显示时过滤(htmlspeicalchar命令等)
    
    csrf攻击
    
    原理：
    
    被攻击网站：用户登录后因为session缘故，浏览器关闭前所有请求都是合法的（不用再精彩账号密码认证了）当用户登录了网站A，这时候访问黑客的网站B，B站的js代码控制用户去调用网站A的一个脚本:
    
    GET方式 如<img src="http://A.com/deletePost.php"> 这样就相当于以用户的身份去触发了对A站http://A.com/deletePost.php脚本的操作
    POST 这种方式要复杂一点，但是还是可以被攻击者利用(构造一个隐藏的html表单，通过js去提交到站点A)以用户的名义去触发操作，比如删除修改密码等，配合XSS攻击效果更明显。
    预防：
    
    采用验证码，token,敏感操作避免GET(因为可以src等方式触发)
    
    逻辑漏洞
    
    比如删除（或修改）用户信息通过GET或POST id=2这样去修改信息支付漏洞：用户提交上来的商品数量为负数-10导致总价变化
    ​```
    
    26.怎么把一个盒子里的某个元素移动到另一个盒子
    
    27.settimeout/setinterval与requestAnimationFrame区别差异
    
    28.如何理解webpack
    
    https://github.com/slashhuang/blog/issues/1
    
    29.制作动画的时候用到什么
    
    30.html5新特性
    
    31.看过哪些源码
    
    32.浏览器缓存机制
    
    33.事件模型
    
    https://www.cnblogs.com/jyybeam/p/5794932.html
    
    34，函数节流
    
    35.为什么用vue?
    
    36.promise
    
    37.高清屏移动端border 1pxbug:
    
    1.用图片
    
    2.viewport+rem
    
    3.transform: scale(0.5)
    
    关于设备像素比devicePixelRatio可以参考这文章http://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/
    
    ​```
    1. <span style="font-size:18px;"><html>  
    2.   
    3.     <head>  
    4.         <title>1px question</title>  
    5.         <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">  
    6.         <meta name="viewport" id="WebViewport" content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">       
    7.         <style>  
    8.             html {  
    9.                 font-size: 1px;  
    10.             }             
    11.             * {  
    12.                 padding: 0;  
    13.                 margin: 0;  
    14.             }  
    15.               
    16.             .bds_b {  
    17.                 border-bottom: 1px solid #ccc;  
    18.             }  
    19.               
    20.             .a,  
    21.             .b {  
    22.                 margin-top: 1rem;  
    23.                 padding: 1rem;                
                    font-size: 1.4rem;  
                 }                
                 .a {  
                    width: 30rem;  
                 }                 
                .b {  
                     background: #f5f5f5;  
                    width: 20rem;  
                }  
             </style>  
             <script>             
                 var viewport = document.querySelector("meta[name=viewport]");  
                 //下面是根据设备像素设置viewport  
                 if (window.devicePixelRatio == 1) {  
                     viewport.setAttribute('content', 'width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no');  
                }  
                 if (window.devicePixelRatio == 2) {  
                    viewport.setAttribute('content', 'width=device-width,initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no');  
                 }  
                if (window.devicePixelRatio == 3) {  
                     viewport.setAttribute('content', 'width=device-width,initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no');  
                }  
                var docEl = document.documentElement;  
                 var fontsize = 10 * (docEl.clientWidth / 320) + 'px';  
                docEl.style.fontSize = fontsize;  
                  
             </script>  
         </head>  
     
        <body>  
            <div class="bds_b a">下面的底边宽度是虚拟1像素的</div>  
             <div class="b">上面的边框宽度是虚拟1像素的</div>  
       </body>  
       
     </html></span>  
    ​```
    
    38.递归/算法
    
    ​```
    var data=[
    {name:'aaa'},
    {name:'bbb',chdren:[
                        {name:'ccc',chdren:[
                                              {name:'ccc',chdren:[...]}
                        ]}
    ]}];
    ​```
    
    把name属性的值遍历出来？
    
    递归：在一个函数通过名字调用自身得情况下构成的。（arguments.callee:是一个指向正在执行得函数得指针，因此可以用它来实现对函数的递归调用，但是再严格模式下，不能通过脚本来访问，会导致错误）
    
    ​```
    var data=[
    
    {name:'aaa'},
    
    {name:'bbb',chdren:[
                         {name:'ccc'},
                         {name:'ddd',chdren:[ {name:'eee'}]}
    ]}
    ];
    var aa=(function f(data){
       data.map(function(value,i){
       	for(var val in value){
       		if(val==='name'){
       			console.log(value[val]);
       		}else if(value[val] instanceof Array){
     	     return f(value[val]);
       	}
       	}
       })
    })
    aa(data);
    ​```
    
    39：闭包的应用场景
    
    ​```
    闭包是指有权访问另一个函数作用域中的变量的函数。闭包只能取得包含函数中任何变量的最后一个值。闭包保存的是整个变量对象，而不是某个特殊的变量。闭包有权访问包含函数内部的所有变量。一般函数执行结束之后会被销毁，但是当函数返回闭包时它的作用域将会一直在内存中保存到闭包不存在为止。
    应用场景1：模仿块级作用域
    应用场景2：在对象中创建私有变量
    应用场景3：模块模式
    闭包典型代码：
    ​```
    
    40.继承
    
    ​```
    es5:
    组合继承：
    function SuperType(name){
    	this.name=name;
    	this.colors=['red','blue','green'];
    }
    SuperType.prototype.sayName=function(){
    	alert(this.name);
    }
    function SubType(name,age){
        SuperType.call(this,name);
        this.age=age;
    }
    SubType.prototype=new SuperType();
    SubType.prototype.constructor=SubType;
    SubType.prototype.sayAge=function(){
    	alert(this.age);
    }
    var instance1=new SubType('Nicholas',29);
    instance1.colors.push('black');
    alert(instance1.colors);
    console.log(instance1);
    instance1.sayName();
    instance1.sayAge();
    var instance2=new SubType('greg',27);
    alert(instance2.colors);
    instance2.sayName();
    instance2.sayAge();
    ​```
    
    41.怎么利用http协议缓存
    
    <meta http-equiv='Cache-control' content='no-cache'>
    
    42.你在项目遇到过的问题？怎么解决？
    
    43.自我介绍？
    
    44.为什么离职？
    
    45.职业规划？
    
    46.你有什么想了解的吗？
    
    ttxx 
    
    1.web框架，列举三种，对比优缺点。
    
    https://www.oschina.net/translate/web-frameworks-conclusions
    
    ​```
    
    ​```
    
    2.sql语法：
    
    表一：user
    
    id  name
    
    1   张三
    
    2   李四
    
    表二 mass
    
    id  user  num
    
    1     1       5 
    
    2     2       5 
    
    3     3       5 
    
    表二的user对应表一的id,查找每个人的num。
    
    select mass.user,mass.num from user left join mass on user.id = mass.user
    
    3.模拟post表单请求。
    
    ​```
      var http = require('http');
            var querystring = require('querystring');
    
            var post_options = {
                host: '192.168.1.22',
                port: '80',
                path: '/pkmslogin.form',
                method: 'post',
                auth: 'username:123456',
                'login-form-type':'pwd',
                headers: {
                     'Content-length': post_data.length,
        'Content-Type': 'application/x-www-form-urlencoded'
                }
            };
            var post_data = querystring.stringify({
                username:'username',
                password:'123456',
                'login-form-type':'pwd'
            });


            // Set up the request
            var post_req = http.request(post_options, function(res) {
                res.setEncoding('utf8');
                console.log(JSON.stringify(res.headers));
    
                res.on('data', function (chunk) {
    
                    console.log('Response: ' + chunk);
                });
            });
            console.log(JSON.stringify(post_req.headers));


            // post the data
            post_req.write(post_data);
            post_req.end();
    ​```
    
    4.说一下，在工作中遇到问题，你怎么排查？找到问题后怎么优化？
    
    5.有A,B,C,D,E个站，他们的路线及花费的时间如下，
    
    AB4,BC5,CD7,DC7,CE6,DE6,AC4.
    
    请编码完成以下功能，计算出路线总花费多少时间。
    
    输入路线：
    
    1.ABC
    
    2.BC
    
    3.DCE
    
    如没有则返回no such router.
    var aa={ab:4,bc:5,cd:7,dc:7,de:6,ac:4,ce:5};
    var bb=function(arr){
    	let cc=arr.split('-');
    	let all=0;
    	cc.reduce(function(a,b,c,d){
    		if(aa[a+b]){
    			 all=aa[a+b]+all;
    		}
    
        return d[c];
    });
    	return all==0?'no such result':all;
    
    }
    console.log(bb('a-c'));
    6.promise原理
    
    ​```
    含义：语法糖，一个对象，用来传递异步操作的消息。代表了某个未来才会知道结果的事件，并且这个事件提供统一的api，可供进一步处理。
    特点: 1. 对象的状态不受外界影响。
          2. 一旦状态改变就不会在变，任何时候都可以得到这个结果。
          3. 异步操作以同步操作流程表达，避免了层层嵌套的回调函数。提供统一接口，易于控制异步操作。
    缺点: 1.无法取消Promise,一旦新建立即执行，无法取消。
          2.若不设置回调函数，内部抛出错误不会反应到外部。
          3.当处于pending状态时，无法得知目前进展到哪一个阶段。
          方法：then,catch(有冒泡性质，建议放到最后),all,race,resolve,reject,done,finally 
     应用场景：
          1.加载图片
          const preloadImage=function(path){
            return new Promise(function(resolve,reject){
              var image=new Image();
              image.onload=resolve;
              image.onerror=reject;
              image.src=path;
            })
          }
          2.generator函数与promise结合
          3.ajax请求
    ​```
    
    ![IMG_1720](D:\Desktop\IMG_1720.JPG)

​     ![IMG_1719](D:\Desktop\IMG_1719.JPG)

7.封装ajax请求，用原生js写下：

```
function ajax(opt) {
  opt = opt || {};
  opt.method = opt.method.toUpperCase() || "POST";
  opt.url = opt.url || "";
  opt.async = opt.async || true;
  opt.data = opt.data || null;
  opt.success = opt.success || function() {};
  var xmlHttp = null;
  if (XMLHttpRequest) {
    xmlHttp = new XMLHttpRequest();
  } else {
    xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
  }
  var params = [];
  for (var key in opt.data) {
    params.push(key + "=" + opt.data[key]);
  }
  var postData = params.join("&");
  if (opt.method.toUpperCase() === "POST") {
    xmlHttp.open(opt.method, opt.url, opt.async);
    xmlHttp.setRequestHeader(
      "Content-Type",
      "application/x-www-form-urlencoded;charset=utf-8"
    );
    xmlHttp.send(postData);
  } else if (opt.method.toUpperCase() === "GET") {
    xmlHttp.open(opt.method, opt.url + "?" + postData, opt.async);
    xmlHttp.send(null);
  }
  xmlHttp.onreadystatechange = function() {
    if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
      opt.success(xmlHttp.responseText);
    }
  };
}
```

 

8.function bb(){

alert('bb');

}

bb();

function bb(){

alert('aa');

}

bb();

输出？

```
两次打印的都是aa,因为函数提升，两个重名的函数按优先级提升至代码顶部，相当于是这样：
function bb(){

alert('bb');

}
function bb(){

alert('aa');

}
bb();
bb();
所以，后者函数声明覆盖了前者，所以是两次都执行了后面的函数。
```

9.

```
this.x = 9;   

var module = {

  x: 81,

  getX: function() { return this.x; }

};

module.getX(); // 81

var retrieveX = module.getX();

retrieveX();   //9

// returns 9 - The function gets invoked at the global scope

// Create a new function with 'this' bound to module

// New programmers might confuse the

// global var x with module's property x

var boundGetX = retrieveX.bind(module);

boundGetX(); // 81

```

10.一个字符串，每三位添加一个逗号，就是金钱数字，然后做一种自动把钱数用逗号分隔的效果。

eg: var a='1234567';

你要通过你写的方法变成a='1,234,567'

11.用一个div标签实现十字架效果，三种方法至少。

     1. css 伪元素after,before
     2. boxshadow盖住四个角 
     3. svg/canvas
     4. 放背景图片
12.var a=[1,2,3,4,3,3,4,1,1,2];

写一个方法，把出现超过两次的给删了。

13.email正则表达式

```
/^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/
```

14.写一个vue组件，省略号组件，当内容超过整体宽度时自动隐藏并加上省略号。点击省略号时打开其内容的效果。


    15.
    var _fn=function(){
    	console.log(1);
    };
    (function(){
    	var _fn=function(){
    		console.log(2);
    	};
    	var fn1=function(){
    		this._fn.apply(this);
    	};
    	var obj={
    		_fn:function(){
    			console.log(3);
    		},
    		fn2:fn1.bind({
    			_fn:function(){
    				console.log(4);
    			}
    		}),
    		fn3:fn1
    	};
    	var fn4=obj.fn3;
    	var fn5=obj.fn2;
    	fn1();
    	obj.fn2();
    	obj.fn3();
    	fn4();
    	this.fn5();
    })();
    输出？？
    1，4，3，1，报错
    ​```

15.求一个字符串中，次数出现最多得字母及次数


	function aa() {
	  return arr.reduce(function(a, b) {
	    a[b] = a[b] + 1 || 1;
	
	    return a;
	  }, {});
	}
	
	var aa = aa();
	
	var maxNum = 0;
	
	var max = "";
	
	for (var i in aa) {
	  if (aa[i] > maxNum) {
	    maxNum = aa[i];
	
	    max = i;
	  }
	}
	
	for (var i in aa) {
	  if (aa[i] == maxNum) {
	    console.log(i);
	
	    console.log(aa[i]);
	  }
	}