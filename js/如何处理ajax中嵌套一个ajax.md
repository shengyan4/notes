# [如何处理ajax中嵌套一个ajax](http://www.cnblogs.com/CoffeeEddy/p/5507972.html)

在做项目的时候 遇到过第二次了 当我第二次去问'公子'的时候 被吐槽了 原来我以前遇到过 只是忘记了...他老人家竟然还记得...

ajax由于他的异步特性 在第一次请求中的循环中嵌套第二个ajax会数据会读不出来

我这边一共有三种方法 

**第一种**

描述：如果条件许可,把两次请求都放在服务端处理掉一起发回来,这些就在客户端只有一次ajax了

优点：代码放在服务端,安全性比较,且服务端处理速度较快

缺点：可能请求的数据格式是json,这样在服务端处理JSON数据还需要对JSON进行反序列化,这样就比较麻烦

**第二种**

描述：是我第一次解决这个问题的时候用的比较蠢的办法,第一次请求的ajax,循环值PUSH到公共变量中去，然后用这个公共变量作为参数去请求第二个ajax

![img](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

优点：节省开销

缺点：这样写的确有点蠢...除了蠢之外 我再补充一点 这样做第二次ajax只能是自己去请求自己服务器,如果是别人的服务 不可能给你拆分参数

**第三种**

描述：使用async ：false。ajax默认async是为ture的，当async: true 时，ajax请求是异步的。但是其中有个问题：ajax请求和其后面的操作是异步执行的，那么当页面还未执行完，就可能已经执行了 ajax请求后面的操作。当async:false时，ajax请求为同步，这时Ajax请求将整个浏览器锁死,直到请求结束

![img](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

优点：可以按照逻辑顺序正常的写代码

缺点：同步时整个页面是被锁死的



转载自：[http://www.cnblogs.com/CoffeeEddy/p/5507972.html](http://www.cnblogs.com/CoffeeEddy/p/5507972.html)