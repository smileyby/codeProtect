移动时代的前端加密
================

## 背景

相比其他被译成二进制的应用，前端这种纯文本应用，太用以被解读和窜改。

### 前端为什么加密？

加密重要的目的是出于对商业利益的保护。

*	由于作品太容易被复制窜改，容易会失去渠道先机
	* 窜改不限于以下：
	* 署名被移除或替换
	* 链接地址被替换
	* 文案被修改
	* 广告被移除，替换或植入
	* ...

一些轻度游戏，用户只会玩一两次，生命周期也就两三天。如果你开发的游戏被人山寨且他的渠道笔记更广，那么对于流量就是致命打击。

*	HTML5被山寨后太廉价
	* 某宝上搜索"HTML5微信小游戏" 400套/10元	

*	避免泄露一些用于运营的脚本
	* 参考：[锤子手机天猫开卖遇乌龙事件](http://www.chinaz.com/news/2014/1013/370303.shtml)

## 前端加密目标

总之就是减少加密的成本增加破解的成本：如果每次画一分钟加密的应用，都需要花2小时以上去破解那就算成功了。

*	加密后的文件不易过大；
	* 100K文件如果加密后到1M无疑增加了用户使用的成本和体验。	
*	没有人工介入不能破解；
	* 即：破解的过程需要人工介入，人工成本无疑是最大的开销。
*	限制在其他域名部署；
	* 守护代码和业务放在一起，部署到其他域名则不能正常使用。
*	不容易被调试跟踪；
	* 对主流的调试工具有防范能力，如：Firebug、Chrome 开发者工具。	

### 那些代码不需要加密

*	开源项目
*	用于学习的项目

## 加密代码的方法

### 降低可读性的方法

#### 压缩（compression）

压缩的目的通常是减少传输量，但也起到降低可读性的作用。

去掉注释，多余的分隔符，空白字符，标识符简写。

这类工具有很多：[YUI Compressor](http://yui.github.io/yuicompressor/)、[UglifyJS](https://github.com/mishoo/UglifyJS)、[Google Closure Compiler](https://developers.google.com/closure/compiler/)

“标识符简写”是一种压缩也是一种混淆。

#### 混淆（obfuscation）

混淆常见的方法是分离静态资源，打乱控制流，增加无意义的代码。

[UglifyJS](https://github.com/mishoo/UglifyJS)和[Google Closure Compiler](https://developers.google.com/closure/compiler/)这类工具实际上也会做简单改变语句。

混淆是降低可读性的利器，有一款商业产品[jscrambler](https://jscrambler.com/en/compare-plans)，最高配每个月95美刀。

##### 标识符混淆
 
混淆前：

```javascript

function render(obj) {
  /* ... */
  console.log(obj.title);
}
render({title: 'buy'});

```

混淆后：

```javascript

function a(e){/* ... */console.log(e.title)}a({title:'buy'})

```

##### 逻辑混淆

混淆前：

```javascript

function render(obj) {
  /* ... */
  console.log(obj.title);
}
render({title: 'buy'});

```

混淆后：

```javascript

var self=this,o={};o.__defineSetter__('t',function(e){self[t('elosnoc')][t('gol')](e[t('eltit')])});function t(e){return e.split('').reverse().join('')};o[t('eltit')]=t('yub');o.t=o

```

##### [aaencode](http://utf-8.jp/public/aaencode.html)

混淆前：

```javascript

alert("Hello, JavaScript")

```

混淆后：

```javascript

ﾟωﾟﾉ= /｀ｍ´）ﾉ ~┻━┻   //*´∇｀*/ ['_']; o=(ﾟｰﾟ)  =_=3; c=(ﾟΘﾟ) =(ﾟｰﾟ)-(ﾟｰﾟ); (ﾟДﾟ) =(ﾟΘﾟ)= (o^_^o)/ (o^_^o);(ﾟДﾟ)={ﾟΘﾟ: '_' ,ﾟωﾟﾉ : ((ﾟωﾟﾉ==3) +'_') [ﾟΘﾟ] ,ﾟｰﾟﾉ :(ﾟωﾟﾉ+ '_')[o^_^o -(ﾟΘﾟ)] ,ﾟДﾟﾉ:((ﾟｰﾟ==3) +'_')[ﾟｰﾟ] }; (ﾟДﾟ) [ﾟΘﾟ] =((ﾟωﾟﾉ==3) +'_') [c^_^o];(ﾟДﾟ) ['c'] = ((ﾟДﾟ)+'_') [ (ﾟｰﾟ)+(ﾟｰﾟ)-(ﾟΘﾟ) ];(ﾟДﾟ) ['o'] = ((ﾟДﾟ)+'_') [ﾟΘﾟ];(ﾟoﾟ)=(ﾟДﾟ) ['c']+(ﾟДﾟ) ['o']+(ﾟωﾟﾉ +'_')[ﾟΘﾟ]+ ((ﾟωﾟﾉ==3) +'_') [ﾟｰﾟ] + ((ﾟДﾟ) +'_') [(ﾟｰﾟ)+(ﾟｰﾟ)]+ ((ﾟｰﾟ==3) +'_') [ﾟΘﾟ]+((ﾟｰﾟ==3) +'_') [(ﾟｰﾟ) - (ﾟΘﾟ)]+(ﾟДﾟ) ['c']+((ﾟДﾟ)+'_') [(ﾟｰﾟ)+(ﾟｰﾟ)]+ (ﾟДﾟ) ['o']+((ﾟｰﾟ==3) +'_') [ﾟΘﾟ];(ﾟДﾟ) ['_'] =(o^_^o) [ﾟoﾟ] [ﾟoﾟ];(ﾟεﾟ)=((ﾟｰﾟ==3) +'_') [ﾟΘﾟ]+ (ﾟДﾟ) .ﾟДﾟﾉ+((ﾟДﾟ)+'_') [(ﾟｰﾟ) + (ﾟｰﾟ)]+((ﾟｰﾟ==3) +'_') [o^_^o -ﾟΘﾟ]+((ﾟｰﾟ==3) +'_') [ﾟΘﾟ]+ (ﾟωﾟﾉ +'_') [ﾟΘﾟ]; (ﾟｰﾟ)+=(ﾟΘﾟ); (ﾟДﾟ)[ﾟεﾟ]='\\'; (ﾟДﾟ).ﾟΘﾟﾉ=(ﾟДﾟ+ ﾟｰﾟ)[o^_^o -(ﾟΘﾟ)];(oﾟｰﾟo)=(ﾟωﾟﾉ +'_')[c^_^o];(ﾟДﾟ) [ﾟoﾟ]='\"';(ﾟДﾟ) ['_'] ( (ﾟДﾟ) ['_'] (ﾟεﾟ+(ﾟДﾟ)[ﾟoﾟ]+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+((ﾟｰﾟ) + (ﾟΘﾟ))+ (c^_^o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟｰﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟΘﾟ)+ (c^_^o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ ((ﾟｰﾟ) + (o^_^o))+ (ﾟДﾟ)[ﾟεﾟ]+((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟｰﾟ)+ (c^_^o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟΘﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ ((o^_^o) +(o^_^o))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (o^_^o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (o^_^o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟΘﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ (c^_^o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o^_^o) +(o^_^o))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟｰﾟ)+ ((o^_^o) - (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟΘﾟ)+ (ﾟДﾟ)[ﾟoﾟ]) (ﾟΘﾟ)) ('_');

```

##### [jjencode](http://utf-8.jp/public/jjencode.html)

混淆前：

```javascript

alert("Hello, JavaScript")

```

混淆后：

```javascript

$=~[];$={___:++$,$$$$:(![]+"")[$],__$:++$,$_$_:(![]+"")[$],_$_:++$,$_$$:({}+"")[$],$$_$:($[$]+"")[$],_$$:++$,$$$_:(!""+"")[$],$__:++$,$_$:++$,$$__:({}+"")[$],$$_:++$,$$$:++$,$___:++$,$__$:++$};$.$_=($.$_=$+"")[$.$_$]+($._$=$.$_[$.__$])+($.$$=($.$+"")[$.__$])+((!$)+"")[$._$$]+($.__=$.$_[$.$$_])+($.$=(!""+"")[$.__$])+($._=(!""+"")[$._$_])+$.$_[$.$_$]+$.__+$._$+$.$;$.$$=$.$+(!""+"")[$._$$]+$.__+$._+$.$+$.$$;$.$=($.___)[$.$_][$.$_];$.$($.$($.$$+"\""+$.$_$_+(![]+"")[$._$_]+$.$$$_+"\\"+$.__$+$.$$_+$._$_+$.__+"(\\\"\\"+$.__$+$.__$+$.___+$.$$$_+(![]+"")[$._$_]+(![]+"")[$._$_]+$._$+",\\"+$.$__+$.___+"\\"+$.__$+$.__$+$._$_+$.$_$_+"\\"+$.__$+$.$$_+$.$$_+$.$_$_+"\\"+$.__$+$._$_+$._$$+$.$$__+"\\"+$.__$+$.$$_+$._$_+"\\"+$.__$+$.$_$+$.__$+"\\"+$.__$+$.$$_+$.___+$.__+"\\\"\\"+$.$__+$.___+")"+"\"")())();

```

## 加密

这里“加密”指代码内容可逆编码。而文中“前端加密”指页面和相关资源文件处理后能正常运行。

### 简单base64

加密前：

```javascript

function a(e){/* ... */console.log(e.title)}a({title:'buy'})

```

加密后：

```javascript

eval(atob("ZnVuY3Rpb24gYShlKXsvKiAuLi4gKi9jb25zb2xlLmxvZyhlLnRpdGxlKX1hKHt0aXRsZTonYnV5J30p"));

```

### Packer

加密前：

```javascript

function a(e){/* ... */console.log(e.title)}a({title:'buy'})

```

加密后：

```javascript

eval(function(p,a,c,k,e,r){e=String;if(!''.replace(/^/,String)){while(c--)r[c]=k[c]||c;k=[function(e){return r[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('3 0(1){4.5(1.2)}0({2:\'6\'})',7,7,'a|e|title|function|console|log|buy'.split('|'),0,{}))

```

## 新技术带来的新思路

到移动时代我们可以放心的用上HTML5、CSS3，使用一些新特性结合已有的方案，可以更大程度增加破解的成本。

### 代码可以放置其他位置

将代码放在非JS文件中，增加代码定位的难度。
*	放到png中

利用HTML [Canvas 2D Context](https://www.w3.org/TR/2dcontext/)获取二进制数据的特性，可以用图片存储脚本资源。 

*	放在css文件中

利用`content`样式能存放字符串的特性，同样可以用来存储脚本资源。

### 执行代码字符串

无论代码放到哪里，都需要执行。执行代码字符串的方式如下几种：

*	创建`<script>`执行

```javascript

var script = document.createElement('script');
script.src = 'data:application/javascript,console.log("Hello%20world!"))';
document.querySelector('script').parentNode.appendChild(script);

```

*	调用`setTimeout()`/`setInterval()`执行

```javascript

setTimeout('console.log("Hello world!")', 0);

```

*	创建`new Function()`执行

```javascript

new Function('console.log("Hello world!")')();

```

*	使用`Worker`执行

```javascript

var URL = window.URL || window.webkitURL;
var Blob = window.Blob || window.webkitBlob;
var blobURL = URL.createObjectURL(
    new Blob(['console.log("Hello World!")'], {type: 'application/javascript'})
);
new Worker(blobURL);
URL.revokeObjectURL(blobURL);

```

*	使用DOM事件执行

```javascript

var div = document.createElement('div');
div.innerHTML = "<img src=! onerror=\"console.log('Hello world!')\">";

```

*	location复制javascript协议的连接

```javascript

location = 'javascript:console.log("Hello world!");';

```


## 放置开发者工具

在复杂的前端加密也难对付调试工具的跟踪解析

那么如何判断浏览器是否开启控制台？这个问题由[完美解决方案](http://stackoverflow.com/questions/7798748/find-out-whether-chrome-console-is-open/30638226#30638226)

判断如果控制台起开则阻塞Javascript执行

```javascript

while(1){} // 卡死

```

## 混合加密

单个方法总是容易被破解，但组合起来就千变万化不那么容易被破解了！破解成本显然指数增长。

*	嵌套加密

```javascript

「C 方法加密
  「A 方法加密
   ...
   」
   ...
  「B 方法加密
   ...
   」
 」

```

*	随机加密

```javascript

「随机方法加密
  「随机方法加密
   ...
   」
   ...
  「随机方法加密
   ...
   」
 」

```

### 实际案例

这里安利下：
*	独立开发的饭吗预处理工具[jdists](https://github.com/zswang/jdists)，非常适合用来做混合加密
*	参考：[jdists 混合加密示例](https://github.com/zswang/jdists/wiki/%5Bcase%5DCode-mixed-encryption)

## 更有力的防范

很难做到100%放置逆向工程，只是增加一些破解的成本。

### 使用专属素材

修改素材就是要做同一套素材，这个其实不必修改代码容易到哪里去。

### 不是单一的前端应用，依赖服务端的存储

前端容易破解，后端却不容易，物理上就隔离了。

### 更好的产品和服务，更快的迭代

再怎么山寨也山寨不了精髓

## 参考资料

*	[求助前端JS都是用什么加密的？](https://www.zhihu.com/question/28468459)
*	[将js/css脚本放到png图片中的实践](http://blog.csdn.net/zswang/article/details/7061560)
*	[将 JS 代码放到 CSS 文件中](http://weibo.com/1486697205/BBTqZzYOM)
*	[Find out whether Chrome console is open?](http://stackoverflow.com/questions/7798748/find-)

文章转自[http://div.io/topic/1220](http://div.io/topic/1220)

