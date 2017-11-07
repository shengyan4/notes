##  meta标签整合

## [name 属性]()

name 属性提供了名称/值对中的名称。HTML 和 XHTML 标签都没有指定任何预先定义的 <meta> 名称。通常情况下，您可以自由使用对自己和源文档的读者来说富有意义的名称。

"keywords" 是一个经常被用到的名称。它为文档定义了一组关键字。某些搜索引擎在遇到这些关键字时，会用这些关键字对文档进行分类。

类似这样的 meta 标签可能对于进入搜索引擎的索引有帮助：

```
<meta name="keywords" content="HTML,ASP,PHP,SQL">
```

如果没有提供 name 属性，那么名称/值对中的名称会采用 http-equiv 属性的值。

## [http-equiv 属性]()

http-equiv 属性为名称/值对提供了名称。并指示服务器在发送实际的文档之前先在要传送给浏览器的 MIME 文档头部包含名称/值对。

当服务器向浏览器发送文档时，会先发送许多名称/值对。虽然有些服务器会发送许多这种名称/值对，但是所有服务器都至少要发送一个：content-type:text/html。这将告诉浏览器准备接受一个 HTML 文档。

使用带有 http-equiv 属性的 <meta> 标签时，服务器将把名称/值对添加到发送给浏览器的内容头部。例如，添加：

```
<meta http-equiv="charset" content="iso-8859-1">
<meta http-equiv="expires" content="31 Dec 2008">

```

这样发送到浏览器的头部就应该包含：

```
content-type: text/html
charset:iso-8859-1
expires:31 Dec 2008

```

当然，只有浏览器可以接受这些附加的头部字段，并能以适当的方式使用它们时，这些字段才有意义。

```

      Meta http-equiv属性详解(转)  http://kinglyhum.iteye.com/blog/827807
      1、Expires(期限) 
   说明：可以用于设定网页的到期时间。一旦网页过期，必须到服务器上重新传输。 
   用法：
   Html代码  收藏代码
   ＜meta http-equiv="expires" content="Wed, 20 Jun 2007 22:33:00 GMT"＞  

   注意：必须使用GMT的时间格式。 

   2、Pragma(cache模式) 
   说明：是用于设定禁止浏览器从本地机的缓存中调阅页面内容，设定后一旦离开网页就无法从Cache中再调出 
   用法：
   Html代码  收藏代码
   ＜meta http-equiv="Pragma" content="no-cache"＞  

   注意：这样设定，访问者将无法脱机浏览。 

   3、Refresh(刷新) 
   说明：自动刷新并指向新页面。 
   用法：
   Html代码  收藏代码
   ＜meta http-equiv="Refresh" content="2；URL=http://www.net.cn/"＞  

   注意：其中的2是指停留2秒钟后自动刷新到URL网址。 

   4、Set-Cookie(cookie设定) 
   说明：如果网页过期，那么存盘的cookie将被删除。 
   用法：
   Html代码  收藏代码
   ＜meta http-equiv="Set-Cookie" content="cookievalue=xxx;expires=Wednesday, 20-Jun-2007 22:33:00 GMT； path=/"＞  

   注意：必须使用GMT的时间格式。 

   5、Window-target(显示窗口的设定) 
   说明：强制页面在当前窗口以独立页面显示。 
   用法：
   Html代码  收藏代码
   ＜meta http-equiv="Window-target" content="_top"＞  

   注意：用来防止别人在框架里调用自己的页面。 

   6、content-Type(显示字符集的设定) 
   说明：设定页面使用的字符集。 
   用法：
   Html代码  收藏代码
   ＜meta http-equiv="content-Type" content="text/html; charset=gb2312"＞  


   7、Pics-label(网页等级评定) 
   用法：
   Html代码  收藏代码
   <meta http-equiv="Pics-label" contect="">  

   说明：在IE的internet选项中有一项内容设置，可以防止浏览一些受限制的网站，而网站的限制级别就是通过meta属性来设置的。 

   8、Page_Enter、Page_Exit 
   设定进入页面时的特殊效果
   Html代码  收藏代码
   <meta http-equiv="Page-Enter"    contect="revealTrans(duration=1.0,transtion=    12)">    

   设定离开页面时的特殊效果
   Html代码  收藏代码
   <meta http-equiv="Page-Exit"    contect="revealTrans(duration=1.0,transtion=    12)">    


   Duration的值为网页动态过渡的时间，单位为秒。  
   Transition是过渡方式，它的值为0到23，分别对应24种过渡方式。如下表：  
   0    盒状收缩    1    盒状放射  
   2    圆形收缩    3    圆形放射  
   4    由下往上    5    由上往下  
   6    从左至右    7    从右至左  
   8    垂直百叶窗    9    水平百叶窗  
   10    水平格状百叶窗    11垂直格状百叶窗  
   12    随意溶解    13从左右两端向中间展开  
   14从中间向左右两端展开    15从上下两端向中间展开  
   16从中间向上下两端展开    17    从右上角向左下角展开  
   18    从右下角向左上角展开    19    从左上角向右下角展开  
   20    从左下角向右上角展开    21    水平线状展开  
   22    垂直线状展开    23    随机产生一种过渡方式  

   9、清除缓存（再访问这个网站要重新下载！） 
   Html代码  收藏代码
   <meta http-equiv="cache-control" content="no-cache">  


   10、设定网页的到期时间 
   Html代码  收藏代码
   <meta http-equiv="expires" content="0">  


   11、关键字,给搜索引擎用的 
   Html代码  收藏代码
   <meta http-equiv="keywords" content="keyword1,keyword2,keyword3">  


   12.页面描述 
   Html代码  收藏代码
   <meta http-equiv="description" content="This is my page"> 
   13、
    <meta http-equiv="X-UA-Compatible" content="IE=edge,Chrome=1" />
      IE=edge表示强制使用IE最新内核，chrome=1表示如果安装了针对IE6/7/8等版本的浏览器插件Google Chrome Frame（可以让用户的浏览器外观依然是IE的菜单和界面，但用户在浏览网页时，实际上使用的是Chrome浏览器内核），那么就用Chrome内核来渲染。关于此meta标签的具体说明，可参见StackOverflow上的精彩回答，
      <meta>标签高人的英文解释可以参看
      http://stackoverflow.com/questions/6771258/whats-the-difference-if-meta-http-equiv-x-ua-compatible-content-ie-edge-e
      我有加了一句
    14.<meta http-equiv="X-UA-Compatible" content="IE=9" />

       内核控制Meta标签，因为目前国内的主流浏览器都是双内核，故而添加meta标签来告诉浏览器使用什么内核来渲染页面 
       15.//SPM是淘宝社区电商业务（xTao）为外部合作伙伴（外站）提供的一套跟踪引导成交效果数据的解决方案。
        下面是一个跟踪点击到宝贝详情页的引导成交效果数据的SPM示例：http:// detail. tmall. com/ item.htm?id= 3716461318 &&spm= 2014.123456789.1.2其中spm=2014.123456789.1.2 便是下文所说的SPM编码SPM编码：用来跟踪页面模块位置的编码，标准spm编码由4段组成，采用a.b.c.d的格式（建议全部使用数字），其中，a代表站点类型，对于xTao合作伙伴（外站），a为固定值，a=2014b代表外站ID（即外站所使用的TOP appkey），比如您的站点使用的TOP appkey=123456789，则b=123456789c代表b站点上的频道ID，比如是外站某个团购频道，某个逛街频道，某个试用频道 等d代表c频道上的页面ID，比如是某个团购详情页，某个宝贝详情页，某个试用详情页 等完整的SPM四位编码能标识出某网站中某一个频道的某一个具体页面。

      链接：https://www.zhihu.com/question/37262526/answer/81544016
       <meta name="data-spm" content="a231o">

       16.//微软验证该站点拥有者信息
         <meta name="msvalidate.01" content="66DC2E5B38AECFB3E244BB04EBAF348B" />
         //谷歌验证该站点拥有者信息
         <meta name="google-site-verification" content="bwl_CutQ7wjeaHn4M_N2h3642IFiGcpHEC5Z-CpeisI" />
         //搜狗验证该站点拥有者信息
         <meta name="sogou_site_verification" content="9w2mwdLNcz"/>
```



## [content 属性]()

content 属性提供了名称/值对中的值。该值可以是任何有效的字符串。

content 属性始终要和 name 属性或 http-equiv 属性一起使用。

## [scheme 属性]()

scheme 属性用于指定要用来翻译属性值的方案。此方案应该在由 <head> 标签的 profile 属性指定的概况文件中进行了定义。

## 全局属性

<meta> 标签支持 [HTML 中的全局属性](http://www.w3school.com.cn/tags/html_ref_standardattributes.asp)。