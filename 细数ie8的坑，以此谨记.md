# 细数ie8的坑，以此谨记

## 一、console.log

ie8对console经常会报该console未定义，而且会影响后面的代码，切记用完删了它。

## 二、不支持h5标签。

如果一定要用可以在网上找那种插件处理使他间接变为支持它。

## 三、不支持css3

解决办法可以css hack,或者js先判断是否是ie8然后另外在加上专门针对ie8的css文件。我用的是第二种，因为我是先做的高版本的向下兼容，所以我觉得这样更方便。

    function isIeEight(){//判断是否是ie,true为是，false为不是
    var browser=navigator.appName
    var b_version=navigator.appVersion
    var version=b_version.split(";");
    var trim_Version=version[1].replace(/[ ]/g,"");
    if((browser=="Microsoft Internet Explorer" && trim_Version=="MSIE8.0")){
        return true;
    }else{
        return false;
    }
    }
    if(isIeEight()==true){//如果是ie8则引入相关css文件
            var filename='css/ie-eight.css?version=' + new Date().getTime();
            var fileref=document.createElement("link");
            fileref.setAttribute("rel", "stylesheet");
            fileref.setAttribute("type", "text/css");
            fileref.setAttribute("href", filename);
            document.getElementsByTagName("head")[0].appendChild(fileref);
        }
# 四、自定义快捷键

做了一个自定义快捷键的功能，主要是屏蔽浏览器自带的快捷键功能，加上自己自定义的功能。在其他浏览器上没问题，但是在ie上就有问题。加了return false之后瞬间变好。代码如下



    document.onkeyup=function(event){//快捷键功能，回车键
      var e=e||window.event;
      var keyCode=e.keyCode||e.which;
      if(keyCode==13){//回车键
          event.keyCode=0;   
        　event.returnValue=false;
        //自定义快捷键事件;
        return false;
      }
    }
# 五、无限次的连续点击发出的无限次请求或者弹窗

不管是给后台发请求还是自己的弹窗，都需要加一个过滤。举个例子：如果有个登录界面，有个登录按钮，你要做个判断，在1秒或者两秒内只允许用户发送一次请求，这样的话你可以加个判断，两秒之后才允许事件第二次点击，发送第二次请求。两秒之内的连续点击登录是无效的。代码如下：

    //htlml
    <button id='submit'>登录</button>
    //js
    function stopBtn(a){
      $(a).attr('disabled',true);
      setTimeout(function(){
        $(a).removeAttr('disabled');
      },2000);
    }
    if(typeOf($('#submit').attr('disabled')=='undefined')){
      stopBtn('#submit');
    }



## 六、去掉ie8浏览器上的兼容视图（向下以ie7的模式出现）

`<meta http-equiv='X-UA-Compatible' content='IE=edge,chrome=1'>//让浏览器以最高的浏览器版本渲染页面`

