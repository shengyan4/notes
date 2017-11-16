# 多个if与多个else if详解（转载自http://blog.csdn.net/cd520yy/article/details/49533217）

下面两句代码，执行结果

形式如下

if ……if……if……else

 if ……else if …… else if……else……

通过观看下面的代码与结果截图可以明白多个if与多个else if 及else执行的情况，简单说就是如果是多个else if的话，只要第一个if条件成立，即使满足else if的条件也不会执行else if及else的内容，如果是多个if的话，最后的else会执行的；else与最近的if匹配，包括else if 的if

if与多个else if是分枝情况。只执行其中一条代码，if与多个if是并列情况，会顺序执行

![\](http://img.blog.csdn.net/20140112210138000?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd29haWx2bWVuZ21lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

多个if会顺序执行，最后的else与最近的if匹配

具体代码如下：

![\](http://img.blog.csdn.net/20140112205236609?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd29haWx2bWVuZ21lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

if与多个else if，只会执行其中一个条件，所以只打印一个结果

![\](http://img.blog.csdn.net/20140112205217656?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd29haWx2bWVuZ21lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![\](http://img.blog.csdn.net/20140112205159640?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd29haWx2bWVuZ21lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![img](http://img.blog.csdn.net/20140112205138781?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd29haWx2bWVuZ21lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)