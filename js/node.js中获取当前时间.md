# node.js中获取当前时间（转自http://blog.csdn.net/qq_15267341/article/details/52443132）

1 左下角 cmd 将盘符定位到webstorm的工作目录路径下

![这里写图片描述](http://img.blog.csdn.net/20160905192818020)

------

![这里写图片描述](http://img.blog.csdn.net/20160905193051974)

------

2 打开npm网站（[https://www.npmjs.com/](https://www.npmjs.com/)）

------

![这里写图片描述](http://img.blog.csdn.net/20160905193622196)

------

![这里写图片描述](http://img.blog.csdn.net/20160905193815335)

------

![这里写图片描述](http://img.blog.csdn.net/20160905195928909)

------

![这里写图片描述](http://img.blog.csdn.net/20160905200021753)

------

回车后开始下载，下载后，会出现在Node_Moudles文件夹下

![这里写图片描述](http://img.blog.csdn.net/20160905200152598)

------

新建一个js文件，文件内容如下：

```
var sd = require('silly-datetime');
var time=sd.format(new Date(), 'YYYY-MM-DD HH:mm');
console.log(time);123
```

------

运行Js文件，控制台输出如下：

------

![这里写图片描述](http://img.blog.csdn.net/20160905200354680)