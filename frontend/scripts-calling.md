#B2B2C JavaScript 接口调用说明

##概览

B2B2C 产品​所有 JavaScript（以下简称JS） 组件和接口都依赖于 Jquery 1.x 版，绝大部分的JS文件已经在每个页面的head里引入，所以只需要直接调用即可。下面列出其中的JS目录结构。

    app/
    ├── site/
    │   └── statics/
    │      └── scripts/
    │           ├── lib/                    -包含所有的框架级文件
    │           │   ├── lab.js              -异步加载模块框架
    │           │   ├── jquery.js           -jquery框架
    │           │   └── jquery.cookie.js    -jquery的cookie读写插件
    │           ├── tools.js                -所有的工具函数
    │           ├── dialog.js               -弹层/对话框组件
    │           ├── switchable.js           -可切换组件（Tabs，幻灯，跑马灯）
    │           └── uploader.js             -文件（图片）上传组件
    └── topc/
        └── statics/
           └── scripts/
                ├── main.js                 -前台环境用到的全局函数
                ├── area_select.js          -地区三级联动组件
                ├── datepicker.js           -日期选择组件
                ├── formplus.js             -包含表单填写/密码检测/表单元素兼容性相关
                ├── localstorage.js         -本地存储兼容性组件
                ├── tips.js                 -工具提示组件
                └── validate.js             -表单验证组件和异步提交拦截

##工具

相当多的针对JS的工具在这里同样适用，常用的有：jslint(jshint)，chrome 开发者工具，Dash(Mac平台文档工具)，JSON editor。

**下面首先介绍一下在 site 中定义的 JS 接口调用说明。**

##工具类函数 _tools.js_

系统级工具类函数，全局使用。包括以下几个函数/插件：

###获取 head 元素

`document.head` 表示 `<head>` 元素，此处统一浏览器兼容性，在需要异步插入 style 文件或 scrpit 文件到文档头部时调用。

###判断是否为 dom 元素

**函数：**
isElement(dom)

**作用：**
判断参数是否为一个 dom 元素。

**参数：**
dom：任意类型的参数

**返回值：**
(Boolean)是 dom 对象返回 true，否则返回 false。

###模板字符替换

**函数：**
substitute(string, object)

**作用：**
以json直接量替换字符串中的 {xx} 部分，可看做一个简单的html模板

**参数：**
- string：要被替换的字符串
- object：对应的json直接量

**返回值：**
(String)被替换后的字符串

###解析JS代码

**函数：**
evalScripts(string, content, execScript)

**作用：**
解析文本中js代码，并可以立即执行

**参数：**
- string：要解析的字符串
- content：解析后的字符串插入到此dom元素
- execScript：是否立即执行JS代码

**返回值：**
(String)解析后的字符串(html代码)

###页面最大 z-index

**函数：**
maxZindex(scope, increase)

**作用：**
获取页面所有元素中最大的z-index是多少

**参数：**
- scope：查找范围的dom元素
- increase：增量

**返回值：**
(Number)最终计算的结果

###获取配置项

**函数：**
dataOptions(element, prefix)

**作用：**
从元素的 `data-attrs` 属性中组织出配置项，以JSON方式输出。

**参数：**
- element: 要获取的目标dom元素
- prefix: 必需，前缀，在 `data-` 后面、要获取的配置项名前面。

**返回值：**
(JSON)最终配置项

###倒计时时钟

**函数：**
countdown(element, options)

**作用：**
通用倒计时，包括倒计时所在容器，倒数秒数，显示方式，回调。

**参数：**
- element: 要显示的位置的dom元素
- options: 配置参数，包含以下参数：
    - start：倒计时的秒数
    - secondOnly：显示方式（是否只显示秒）
    - callback：倒计时完成后的回调

**返回值：**
无

###获取元素（包含自身）的html字符串

**函数：**
$.fn.outerHtml()

**作用：**
类似IE的outerHTML，在此统一为 `outerHtml`，绑定到jQuery对象上。

**参数：**
无

**返回值：**
(String)包含自身的html字符串

###获取元素的内补或外补​

**函数：**
$.fn.patch(type)

**作用：**
获取元素内边距、外边距或边框宽度

**参数：**
type：获取哪一部分样式，可以为padding,margin或border

**返回值：**
(JSON)根据type计算出的最终结果

###dom 9点定位

**函数：**
$.fn.locate(options)

**作用：**
根据9个方位由一个dom定位到另一个dom上的位置

**参数：**
- options：配置项的JSON直接量，包括：
    - relative：要定位到的元素，默认为body
    - x：定位dom的x轴的位置，默认为中心
    - y：定位dom的y轴的位置，默认为中心

**返回值：**
(DOM)自身

###数组some方法

**函数：**
Array.some(fn, thisArg)

**作用：**
为不支持数组some方法的浏览器增加some方法，使得回调中执行的结果只要有一个为真值，就返回true，否则为false。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略。
some 不会改变原有数组。

**参数：**
- fn：要对每个数组元素执行的回调函数，此函数可以有三个参数：当前元素，当前元素的索引和当前的数组对象。
- thisArg：在执行回调函数时定义的this对象。如果没有定义或者为null，那么将会使用全局对象。

**返回值：**
(Boolean)根据执行结果返回true或false

**示例：**

检查是否所有的数组元素都大于等于10：

    function isBigEnough(element, index, array) {
        return (element >= 10);
    }
    var passed = [2, 5, 8, 1, 4].some(isBigEnough);
    // passed is false
    passed = [12, 5, 8, 1, 4].some(isBigEnough);
    // passed is true

    [2, 5, 8, 1, 4].some(isBigEnough)： false
    [12, 5, 8, 1, 4].some(isBigEnough)： true

**延伸方法：**
$.fn.some：给jQuery对象增加类似数组的some方法

###数组every方法

**函数：**
Array.every(fn, thisArg)

**作用：**
为不支持数组every方法的浏览器增加every方法，使得回调中执行的结果必须全部为真值，才返回true，否则为false。它只对数组中的非空元素执行指定的函数，没有赋值或者已经删除的元素将被忽略。
every 不会改变原有数组。

**参数：**
- fn：要对每个数组元素执行的回调函数，此函数可以有三个参数：当前元素，当前元素的索引和当前的数组对象。
- thisArg：在执行回调函数时定义的this对象。如果没有定义或者为null，那么将会使用全局对象。

**返回值：**
(Boolean)根据执行结果返回true或false

**示例：**

测试数组大小：

    function isBigEnough(element, index, array) {
        return (element >= 10);
    }
    var passed = [12, 5, 8, 130, 44].every(isBigEnough);
    // passed is false
    passed = [12, 54, 18, 130, 44].every(isBigEnough);
    // passed is true

    [12, 5, 8, 130, 44].every(isBigEnough): false
    [12, 54, 18, 130, 44].every(isBigEnough): true

**延伸方法：**
$.fn.some：给jQuery对象增加类似数组的some方法

###创建面向对象函数

**函数：**
new Class(o, props)

**作用：**
创建类似面向对象编程的函数

**参数：**
- o：JSON对象，包含所有Class的方法，如果含有 `init` 方法就直接执行。
- props：JSON对象，为此对象添加其它公有方法。

**返回值：**

###打印调试

**函数：**
log(...)

**作用：**
替代 `console.log`，打印所列出的变量。并且带历史记录history。如果不支持 `console` 就用 `alert` 替代

**参数：**
可以有多个，要打印的变量值或任意类型的值

**返回值：**
无

##弹框组件 _dialog.js_

此弹框组件包含几个应用：弹层对话框/遮罩层/消息提示框。下面先来看下弹层对话框的应用。

###对话框 Dialog

####基本结构

以下对话框包含了对话框容器、头部和正文部分。

    <div class="dialog" data-module="dialog" tabindex="1">
      <div class="dialog-body">
        <div class="dialog-header">
          <h2>提示</h2>
          <span><button type="button" class="dialog-btn-close" title="关闭"><i>×</i></button></span>
        </div>
        <div class="dialog-content">
          <p>当我们提供两个，的所有属性都添加个参数提供数被省略。在这种情况下这样，我们可以加新的功能。这对于插件开新函数时是很有用的。请记住，目标对象将被修改，并且将通回。然而，如果我们想保留原对象，我们可以通过传递一个空对象作</p>
        </div>
      </div>
    </div>

####具体行为

一般情况下，通过点击行为弹出对话框，需要 JavaScript 脚本添加事件，如果需要遮罩层遮住下面的内容，需要增加 modal 参数：

    <button class="btn" id="btn_dialog"><span><span>对话框1</span></span></button>
    <div id="dialog" style="display:none">
      <p>当我们提供两个，的所有属性都添加个参数提供数被省略。在这种情况下这样，我们可以加新的功能。这对于插件开新函数时是很有用的。请记住，目标对象将被修改，并且将通回。然而，如果我们想保留原对象，我们可以通过传递一个空对象作</p>
    </div>

    $('#btn_dialog').click(function (e) {
        $('#dialog').dialog({
            width: 500,
            modal: true
        });
    });

或者如果是调用一个外部地址或内容为异步加载进来的，还可以使用：

    $.dialog(url, {
        modal: true,
        async: 'ajax'
    });

还可以通过 data 属性调用对话框组件，不需写 JS 代码，只需要通过在一个起控制器作用的元素（例如：按钮/链接）上添加 `data-toggle="dialog"` 属性，同时加入 `data-target="#foo"` 属性，或者链接的 `href="#foo"` 属性，用于指向被控制的对话框。

      <button type="button" class="btn btn-lg" data-toggle="dialog" data-target="#one_dialog"><span><span>打开对话框</span></span></button>

      <div id="one_dialog">
        <p>我只是一段文字。</p>
      </div>

####参数

可以将选项通过 data 属性或 JS 代码传递。对于 data 属性，需要将参数名称放到 `data-dialog-` 之后，例如 `data-dialog-modal="true"`。

名称 | 类型 | 默认值 | 描述
----|----|----|----
target|String or dom|null|弹出框内容元素，接受url或jquery对象
width|Number|0/auto|对话框外框宽度，默认自适应
height|Number|0/auto|对话框外框高度，默认自适应
type|String|null|弹框类型:nohead,notitle,noclose,或模板字串
title|String|'提示'|弹出框标题
load|Function|null|弹出框载入时触发事件
show|Function|null|弹出框显示时触发事件
close|Function|null|弹出框关闭时触发事件
autoHide|Number/Boolean|false|是否在几秒后自动消失
modal|Boolean/JSON|false|弹框是否出现遮罩，及遮罩的显示参数，参数含义请查看 Mask 一节。
locate|JSON|(a JSON)|弹出框的定位，默认定位到屏幕中央，更多参数含义请直接查看 `$.fn.locate` 函数
useIframeShim|Boolean|false|是否使用iframe遮盖，以便解决某些情况下 flash 层级问题。
async|Boolean/String|false|异步加载内容的方式
frameTpl|String|(html)|用iframe方式调用的模板
ajaxTpl|String|(html)|用ajax方式调用时的loading模板
asyncOptions|JSON|(a JSON)|异步请求时的参数，具体参数含义请参照 `jQuery.ajax`，同样支持 ajax 的事件
component|JSON|(a JSON)|弹出框的构成组件的class名，包括框容器，内容，头部，正文，关闭按钮

####方法

#####setHeight(el)
根据 el 或 this.container(框容器) 的高重新设置 this.content(内容区) 的高，一般在重新获取内容并且高度发生变化后使用。

#####locate(options)
根据 options 参数重新定位弹出框

####衍生实例

#####$.dialog.alert(msg, options)
弹出一个类似系统 alert 效果的对话框，分别传入消息文本和弹出框参数

#####$.dialog.confirm(msg, fn, options)
弹出一个类似系统 confirm 效果的对话框，分别传入消息文本，回调和弹出框参数。

#####$.dialog.image(url, options)
弹出一个图片预览对话框，分别传入图片地址和弹出框参数

#####$.dialog.ajax(url, options)
Dialog 的 async 参数为 'ajax' 的快捷方式，分别传入异步调用的地址和弹出框参数。

#####$.dialog.iframe(url, options)
Dialog 的 async 参数为 'iframe' 的快捷方式，分别传入调用 iframe 的src和弹出框参数。

###遮罩 Mask

在页面上置放一个层，使页面上相应区域不可操作。一般用于对话框，也可以单独使用，如某块区域 loading 时可以在等待时用遮罩使整块区域不可做其他操作。

####实例

用于全屏遮盖的实例，只需要

    var mask = new Mask();

####参数

名称 | 类型 | 默认值 | 描述
----|----|----|----
class|String|'mask'|用于遮罩的默认样式名
target|dom|document.body|遮罩的区域（范围）
width|Number|0|遮罩区的宽度
height|Number|0|遮罩区的高度
zIndex|Number|null|遮罩的层级
locate|Boolean|false|是否需要对遮罩定位（默认以中心点定位）
resize|Boolean|false|是否需要根据窗口缩放自动适应大小

####方法

#####setSize()
设定遮罩层的大小。

#####locate()
对遮罩层重新定位

#####toggle()
根据遮罩层当前状态显示/隐藏遮罩层

###消息提示框

用于页面成功/错误消息提示，形式为弹层，具体样式由另外的样式文件定义。

####实例

最简单的调用方式：

    Message('一般操作提示信息');

如果需要在3秒后消失，并且消失后刷新当前页面：

    Message('提示：出错，请重试！', 'error', 3, function() {
        location.reload();
    });

####参数

名称 | 类型 | 默认值 | 描述
----|----|----|----
msg|String|null|要显示的消息
type|String|'show'|要显示消息的类型
delay|Number|3|自动隐藏消息框的时间
callback|Function|nil|隐藏消息框后的回调函数

####方法

#####show(msg, delay, callback)
消息的类型为 'show' 时的简写，参数意思同上。

    Message.show('show message');

#####error(msg, delay, callback)
消息的类型为 'error' 时的简写，参数意思同上。

#####success(msg, delay, callback)
消息的类型为 'success' 时的简写，参数意思同上。

#####hide(type)
直接隐藏显示出来的消息提示框。type 意义同上。

##可切换组件 _switchable.js_

可切换组件广泛应用于网页的特效中，很多效果都可以用可切换组件用不同的参数及样式组合实现。这里不可能列出所有的情况，只能给出一些用法和实例，更多的实例待各位慢慢研究和发掘了。

###实例

####标签切换组件（Tabs）

    <div id="demo1" class="section" data-toggle="switchable" data-switchable-config="{&quot;events&quot;:&quot;click&quot;}">
      <ul class="switchable-triggersList">
        <li class="">标题 A</li>
        <li class="active">标题 B</li>
        <li class="">标题 C</li>
        <li class="">标题 D</li>
        <li class="">标题 E</li>
      </ul>
      <div class="switchable-content">
        <div style="display: none;">标题 A内容</div>
        <div style="display: block;">index = 1</div>
        <div style="display: none;">index = 2</div>
        <div style="display: none;">内容 D</div>
        <div style="display: none;">index = 4</div>
      </div>
    </div>

这是一个标准的标签切换组件，用于多内容在一块区域中切换显示/隐藏。

####广告幻灯片（Slider）

广告幻灯片大多用于广告或商品图片的轮播，可以支持渐隐渐显，左右滚动，上下滚动几种形式。Slider 比 Tabs 多了几项参数。

    <div id="demo2" class="section loading" data-toggle="switchable" data-switchable-config="{&quot;effect&quot;: &quot;scrollx&quot;, &quot;autoplay&quot;: true, &quot;interval&quot;: 2, &quot;events&quot;: &quot;click&quot;, &quot;circle&quot;: true}">
      <ol class="switchable-content">
        <li><a href="#" target="_blank"><img src="img/slider-img1.jpg"></a></li>
        <li style="display:none;"><a href="#" target="_blank"><img src="img/slider-img2.jpg"></a></li>
        <li style="display:none;"><a href="#" target="_blank"><img src="img/slider-img3.jpg"></a></li>
        <li style="display:none;"><a href="#" target="_blank"><img src="img/slider-img4.jpg"></a></li>
        <li style="display:none;"><a href="#" target="_blank"><img src="img/slider-img5.jpg"></a></li>
      </ol>
      <ul class="switchable-triggersList">
        <li class="active">1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
      </ul>
    </div>

####图片文字广告

    <div id="demo3" class="section" data-toggle="switchable" data-switchable-config='{"effect": "fade", "duration": 0.8, "delay": 0.1, "autoplay": true}'>
      <div class="switchable-content">
        <p class="active"><a target="_blank" href="#"><img src="img/59e5194738d5a71869d9109b457d554a.jpg"></a></p>
        <p><a target="_blank" href="#"><img src="img/75ecf47c041778f4271b2a8fe978322e.jpg"></a></p>
        <p><a target="_blank" href="#"><img src="img/414cc9a743bc5923189535b59c00c10e.jpg"></a></p>
        <p><a target="_blank" href="#"><img src="img/501bb0c35d1f300a1fb78b28cd1e5929.jpg"></a></p>
        <p><a target="_blank" href="#"><img src="img/63e32154522a7fb6a89b78cd4f9b0876.jpg"></a></p>
        <p><a target="_blank" href="#"><img src="img/ec26c2d603459f73d1a46713ce88abce.jpg"></a></p>
      </div>

      <ul class="switchable-triggersList">
        <li class="active"><a target="_blank" href="#">新品限时抢</a></li>
        <li><a target="_blank" href="#">夏日冰凉价</a></li>
        <li><a target="_blank" href="#">父亲节特惠</a></li>
        <li><a target="_blank" href="#">T恤99两件</a></li>
        <li><a target="_blank" href="#">父亲节礼物</a></li>
        <li><a target="_blank" href="#">千件秒杀</a></li>
      </ul>
    </div>

这种方式使得图片广告可以带上标题/描述。

####图片轮播浏览（图片延迟加载）

初始的时候第2，3页面的图片并没有加载，只是翻动到当前页的时候才会加载此页的所有图片，控制的参数为 `lazyload: true`。

    <div id="demo4" class="section scrollable" data-toggle="switchable" data-switchable-config='{
      "effect": "scrollx",
      "step": 5,
      "viewSize": [680],
      "lazyload": true,
      "circle": true,
      "hasFlips": true
      }'>
      <span class="prev">&lsaquo; 上一页</span>
      <span class="next">下一页 &rsaquo;</span>
      <div class="scroller">
        <div class="switchable-content">
          <!-- 1-5 -->
          <img src="img/imgview1.jpg" />
          <img src="img/imgview2.jpg" />
          <img src="img/imgview3.jpg" />
          <img src="img/imgview4.jpg" />
          <img src="img/imgview5.jpg" />
          <!-- 5-10 -->
          <img data-src="img/imgview6.jpg" />
          <img data-src="img/imgview7.jpg" />
          <img data-src="img/imgview8.jpg" />
          <img data-src="img/imgview9.jpg" />
          <img data-src="img/imgview10.jpg" />
          <!-- 10-15 -->
          <img data-src="img/imgview11.jpg" />
          <img data-src="img/imgview12.jpg" />
          <img data-src="img/imgview13.jpg" />
          <img data-src="img/imgview14.jpg" />
          <img data-src="img/imgview15.jpg" />
        </div>
        <ul class="switchable-triggersList">
          <li class="active">&bull;</li>
          <li>&bull;</li>
          <li>&bull;</li>
        </ul>
      </div>
    </div>

####跑马灯文字广告

此为无缝滚动文字广告，需要配置参数中加入：`circle: true`

    <div id="demo5" class="scroll-news" data-toggle="switchable" data-switchable-config='{
      "hasTriggers": false,
      "autoplay": true,
      "effect": "scrolly",
      "circle": true
      }'>
      <ul class="news-items switchable-content">
        <li><a href="#" target="_blank">“一分钱”轻松体验shopex网购流程</a></li>
        <li><a href="#" target="_blank">开通网银，支付宝为您一路护航</a></li>
        <li><a href="#" target="_blank">新手买家？帮助教程带您走进淘宝</a></li>
        <li><a href="#" target="_blank">尽情挥洒你的创意，共建shopex联盟</a></li>
        <li><a href="#" target="_blank">认准标识，精选实力卖家任您选择</a></li>
        <li><a href="#" target="_blank">收藏</a> + <a href="#" target="_blank">购物车</a>，逛街看店更便捷</li>
      </ul>
    </div>

####缩略图配文字描述广告

此例结构稍显复杂，触发点为缩略图，并且连带标题和描述一起切换，需要用到事件回调。

    <div id="slideFocus" data-toggle="switchable">
      <div class="content">
        <div class="pic loading">
          <ul class="switchable-content">
            <li><a href="#"><img src="img/compleximg1.jpg"/></a></li>
            <li><a href="#"><img src="img/compleximg2.jpg"/></a></li>
            <li><a href="#"><img src="img/compleximg3.jpg"/></a></li>
            <li><a href="#"><img src="img/compleximg4.jpg"/></a></li>
            <li><a href="#"><img src="img/compleximg5.jpg"/></a></li>
            <li><a href="#"><img src="img/compleximg6.jpg"/></a></li>
          </ul>
        </div>
        <div class="txt">
          <ul class="desc-list">
            <li style="display: block">
              <h4><a title="《神话》热播 第05集21:10分上线" target="_blank" href="#">《神话》热播 第05集21:10分上线</a></h4>
              <p>秦代的刑场上，一个身着奇装异服的年轻人从天而降，众将士议论纷纷，不敢近前，不知是叛贼来劫法场，还是神仙妖孽下凡……</p>
            </li>
            <li style="display: none">
              <h4><a target="_blank" href="#">2010年第一场强降雪飘落京城</a></h4>
              <p>继昨日凌晨降下新年首场小雪，今日首场强降雪飘落京城，据气象台预计雪后北京将降温至-16℃，这也将创下近40年来的最低温记录。</p>
            </li>
            <li style="display: none">
              <h4><a target="_blank" href="#">娱乐圈丑闻 导演张一白吸毒被抓</a></h4>
              <p>正拍摄《杜拉拉升职记》的导演张一白因吸毒被抓，据传是其捧红的徐静蕾“告密”，衣食无忧、事业有成的明星，为何屡屡曝出吸毒丑闻？</p>
            </li>
            <li style="display: none">
              <h4><a target="_blank" href="#">詹姆斯-卡梅隆传奇巨制《阿凡达》</a></h4>
              <p>曾因《泰坦尼克号》创造过票房记录的好莱坞导演詹姆斯-卡梅隆，经过了14年的酝酿，耗资4亿美元拍制的科幻巨献《阿凡达》将于4日在中国上映！</p>
            </li>
            <li style="display: none">
              <h4><a target="_blank" href="#">2009年大学生十大杯具事件</a></h4>
              <p>从70岁老教授潜规则艺校女生，到上海贫困女硕士自杀，09年发生在大学生身上的是非真是多得来令人惊诧，除了心酸就只有“杯具”了。</p>
            </li>
            <li style="display: none">
              <h4><a target="_blank" href="#">尚周刊 第三期</a></h4>
              <p>全新《尚周刊》在新年里与大家新鲜见面。简约是本周最IN的街头风！闪亮派对妆是每个潮女必备美妆术！想了解更多时尚资讯来《尚周刊》！</p>
            </li>
          </ul>
          <ul class="switchable-triggersList">
            <li class="active"><img src="img/compleximg-thumbs1.jpg"/></li>
            <li><img src="img/compleximg-thumbs2.jpg"/></li>
            <li><img src="img/compleximg-thumbs3.jpg"/></li>
            <li><img src="img/compleximg-thumbs4.jpg"/></li>
            <li><img src="img/compleximg-thumbs5.jpg"/></li>
            <li><img src="img/compleximg-thumbs6.jpg"/></li>
          </ul>
        </div>
      </div>
    </div>

    $('#slideFocus').data('switchableConfig', {
        autoplay: true,
        effect: 'scrollx',
        circle: true,
        onBeforeSwitch: function(e, toIndex) {
            $('.desc-list li').each(function(index, value) {
                value.style.display = index == toIndex ? '' : 'none';
            });
        }
    });

####手风琴折叠效果

这里只需要一个参数：`type: 1`，其它都是通过样式控制其展示方式

    <div id="accordion1" class="section" data-toggle="switchable" data-switchable-config='{"type":1}'>
      <div class="switchable-trigger active"><span>标题A</span></div>
      <div class="switchable-panel">
        支持鼠标滑过和点击触点两种方式
      </div>
      <div class="switchable-trigger"><span>标题B</span></div>
      <div class="switchable-panel" style="display:none;">
        内容B<br/>内容B<br/>内容B
      </div>
      <div class="switchable-trigger"><span>标题C</span></div>
      <div class="switchable-panel" style="display:none;">
        内容C<br/>内容C<br/>内容C<br/>内容C<br/>内容C
      </div>
      <div class="switchable-trigger"><span>标题D</span></div>
      <div class="switchable-panel" style="display:none;">
        内容D<br/>内容D<br/>内容D
      </div>
    </div>

更多的实例等待你去完成……

###用法

通过 JS 应用可切换组件：

    $('#demo1').switchable(options);

###参数

可以通过 `data` 属性或 JS 传递参数。对于 `data` 属性，将参数名附着到 `data-switchable-` 后面，例如 `data-switchable-*=""`。

名称 | 类型 | 默认值 | 描述
----|----|----|----
type|Number|0|获取 triggers 和 panels 的方式，是通过 navCls 和 contentCls 还是通过 triggerCls 和 panelCls
navCls|String|switchable-triggersList|通过此类获取触发条件的容器
contentCls|String|switchable-content|通过此类获取显示内容的容器，但不是具体的内容面板
triggerCls|String|switchable-trigger|通过此类获取具体的触发条件，此情况下，一般触发条件不在同一个容器
panelCls|String|switchable-panel|通过此类获取具体的显示内容面板，此情况下，一般内容面板不在同一个容器
hasTriggers|Boolean|true|是否需要触发点
activeIndex|Number|0|初始时被激活的索引
activeCls|String|active|被激活时的css样式名
events|Array Events|['click', 'hover']|触发条件事件响应数组，目前支持click和hover
hasFlips|Boolean|false|是否有翻页按钮
backward|dom|.prev|向后翻页按钮的dom节点
forward|dom|.next|向前翻页按钮的dom节点
step|Number|1|一次切换的内容面板数
delay|Number|0.1|延迟执行切换的时间间隔（秒）
viewSize|Array|[]|一般自动设置，除非自己需要控制，显示内容面板的[宽, 高]
autoplay|Boolean|false|是否自动播放
interval|Number|3|自动播放间隔时间（秒）
pauseOnHover|Boolean|true|鼠标悬停在容器上是否暂停自动播放
effect|String/Function|none|过渡特效，默认为直接显示/隐藏，也可以直接传入特效函数
duration|Number|0.5|动画的时长（秒）
easing|String|linear|缓动函数，默认为线性缓动（jQuery本身只提供 linear 和 swing 两种缓动函数，更多的效果请看 [jQuery easing 插件](http://gsgd.co.uk/sandbox/jquery/easing/)
circle|Boolean|false|是否循环播放​
lazyload|Boolean|false|是否延迟加载
lazyDataType|String|data-src|延迟加载的类型，支持图片延迟加载，及文本数据和脚本延迟加载

###事件

注：这些事件也是以参数的形式写在 options 里面的。

事件类型|事件描述
----|----
onBeforeSwitch|在切换到下一个标签页前触发
onAfterSwitch|在切换到下一个标签页后触发

##文件/图片上传组件 _uploader.js_

###实例和用法

此组件为用 ajax 上传文件，为了兼容 IE8 + 利用了 iframe 作为容器。用 JS 方式这样调用：

    $().AjaxFileUpload(options);

此时需要一个 dom 元素里面放入一个 file 控件。

在 _uploader.js_ 末尾引用了一个实例，封装了一个多图片上传组件，此组件基本结构如下：

    <form method="post" action="" enctype="multipart/form-data">
      <div class="images-uploader">
        <div class="handle img-thumbnail action-upload">
          <input type="file" name="images[]" multiple accept="image/*" data-size="2048000" data-max="5" data-remote="upload_images.php" class="action-file-input">
          <span class="icon-add"></span>
        </div>
      </div>
    </form>

此即为

    $('.images-uploader').AjaxFileUpload(options);

的实例，并且加入onComplete事件以处理上传成功后的后继动作（显示图片，加入可删除图片的操作方式）。

###参数

同样支持两种方式传递参数：在 file 控件上写 `data-` 属性以及 JS 方式传入参数。

名称 | 类型 | 默认值 | 对应的 data 属性 | 描述
----|----|----|----|----
url|String|upload.php|data-remote|一个文件地址，作用是对上传文件进行处理并返回处理后的结果。
size|Number|null|data-size|文件大小上传限制（字节数）
type|String|null|data-accept|文件类型上传限制（暂未实现）
limit|Number|0|data-max|文件上传数量限制

###事件

事件类型|事件参数|事件描述
----|----|----
onChange|文件名称/上传组件容器|在 file 控件的值被更改后触发
onSubmit|文件名称/上传组件容器|在文件被上传时触发
onComplete|文件名称/上传组件容器|在文件被上传成功后触发

**以下再介绍下在 topc 中定义的 JS接口调用说明。**

##通用函数库 _main.js_

###商品价格格式化 _priceControl 对象_

此对象内置几个方法/属性，用于转换商品价格/货币单位。调用方式：

    priceControl.xxx();

####spec

此属性包含以下几个属性：

- decimals: 2，小数位数。
- dec_point: '.'，小数位分隔符。
- thousands_sep: ''，千分位分隔符。
- sign: '\uffe5'，货币符号。

####format(num, force)

**作用：**
对传入的数字进行格式化

**参数：**

- num：要格式化的数值（或字符串形式的数值）
- force：是否需要给货币符号单独加样式

**返回值：**
(String)根据spec格式化后的价格

####number(format)

**作用：**
把传入的参数转换成数值。

**参数：**

- format：可以是String,Number,或者是表单元素，或者是任何包含数值的容器。

**返回值：**
(Number)转换后的数值

####calc(calc, n1, n2, noformat)

**作用：**
计算两个商品价格的值。

**参数：**

- calc：加(add)或减(diff)。
- n1：必须，可以是数值，或者可以被number方法转换成数值。
- n2：同n1。
- noformat：是返回数值，还是返回被format方法格式化后的价格结果。

**返回值：**
(String/Number)计算后的结果（价格或数值）。

####add(n1, n2, flag) / diff(n1, n2, flag)
分别是calc('add', ...) 和 calc('diff', ...)的快捷方法。

###更新购物车数量

**函数：**
updateCartNumber

**作用：**
更新购物车里的商品数量/种类

**参数：**

- content：要更新的dom 元素

**返回值：**
无

##多级地区选择组件 _area_select.js_

###实例

    <div id="area"></div>

    $('#area').multiSelect({
        dataUrl: 'public/app/ectools/statics/js/area.json'
    });

###参数

名称 | 类型 | 默认值 | 描述
----|----|----|----
dataUrl|String|data.json|地区数据源文件，以json直接量方式保存
level|Number|3|选择地区的层级，默认到区县
name|String|area[]|选择地区表单的名称
initData|String|null|初始地区数据，以逗号分隔的id

##日期选择组件 _datepicker.js_

一般的日期选择组件，像一个日历一样，可选取需要的日期，也可手动输入。

###实例

普遍采用的单个日期选择框的实例如下：

    <div class="input-comb date" data-toggle="datepicker">
      <input type="date" class="input-ln">
      <span class="input-comb-addon"><i class="icon-calendar"></i></span>
    </div>

如果需要调用一个日期范围，比如开始日期和结束日期，那么需要多一点代码如下：

    <div class="input-daterange" data-toggle="datepicker">
      <input type="date" value="2014-04-05">
      <span class="add-on">to</span>
      <input type="date" value="2014-04-09">
    </div>

###用法

可以通过 data 属性调用日期选择组件，只需要通过在一个容器元素上添加 `data-toggle="datepicker"` 属性，并在里面加入一个 input 表单元素即可。如果因为某种原因需要将此功能关闭，即解除以 `data-api` 为命名空间并绑定在文档上的事件，可以下面这样：

    $(document).off('.datepicker.data-api');

除此之外，也可以使用 JS 方式调用。

    $('.datepicker').datepicker(options);

###参数

同样支持两种方式传递参数：在 date 容器上写 `data-date-` 属性以及 JS 方式传入参数。例如：startDate 用 data- 属性表示就是 data-date-start-date，format 就是 data-date-format，daysOfWeekDisabled 就是 data-date-days-of-week-disabled。

名称 | 类型 | 默认值 | 描述
----|----|----|----
autoclose|Boolean|false|选中日期后是否马上关闭日期选择器
beforeShowDay|Function(Date)|$.noop|传入一个日期，返回以下几个结果：underfined/Boolean(日期是否能被选取)/String（用于添加在此日期单元的样式名）/JSON（以上几种形式组合）
calendarWeeks|Boolean|false|是否在左侧显示周数
clearBtn|Boolean|false|是否显示“清除”按钮。
daysOfWeekDisabled|String/Array|''|一周中的第几天不能被选择，值从0到6代表周日到周六。
endDate|Date|不限|能选择到的最大日期
forceParse|Boolean|true|是否把用户输入的值强制解析为正确的日期格式。
format|String|yyyy-mm-dd|正确的日期格式
inputs|Array|无|使用范围日期选择时，可用的输入框元素。
keyboardNavigation|Boolean|true|是否可以使用快捷键。
minViewMode|String|0|可用的最小视图，用0表示天，1表示月，2表示年。
multidate|Boolean/Number|false|是否可填入多个日期。如果是数字，表示总共可以填写的日期的数量。分隔符以multidateSeparator给出。
multidateSeparator|String|,|以什么符号分隔填入的多个日期。
orientation|String|auto|允许选择组件的弹出框的位置固定在什么地方。可选择的值有 left, right, top, bottom 几种方向或方向的组合。
startDate|Date|不限|能选择到的最小日期
startView|Number/String|0|打开组件时默认所在的视图是哪个。用0表示月，1表示年，2表示10年。
todayBtn|Boolean|false|是否显示“今天”按钮
todayHighlight|Boolean|true|是否高亮“今天”的日期。
weekStart|Number|1|一星期开始的日期，0-6表示周日到周六。

###方法

方法以如下形式调用，第一个参数为方法名，后面的为方法的参数。

    $('.datepicker').datepicker('method', arg1, arg2);

####remove()
删除此组件，对应的事件，对象以及生成的html结构。

####show()
显示此组件。

####hide()
隐藏此组件。

####update(date)
用给定的日期或当前的输入值更新组件。

####setDate(date)
为组件设定给定的日期。

####setUTCDate(date)
为组件设定给定的UTC日期。

####setDates(date...)
为组件设定给定的一组日期，用于多日期选择。

####setUTCDates(date...)
为组件设定给定的一组UTC日期。

####getDate()
返回当前选定的日期，如果是多日期选择，返回最后选定的日期。

####getUTCDate()
返回当前选定日期的UTC格式。

####getDates()
返回当前选定的一组日期，用于多日期选择。

####getUTCDates()
返回当前选定的一组日期的UTC格式。

####setStartDate(date)
为组件设定最小可选择日期。

####setEndDate(date)
为组件设定最大可选择日期。

####setDaysOfWeekDisabled(String/Array)
设定一周中有哪些天是被禁止选择的。

###事件

此组件中的事件在使用时，类似一般的事件函数的用法，使用如下形式：

    $'.datepicker').datepicker()
        .on(event, function(e) {
            // e 包含此事件中的扩展属性
        });

事件类型|事件描述
----|----
show|当日期选择器弹出框显示时触发此事件。
hide|当日期选择器弹出框隐藏时触发此事件。
clearDate|当输入的日期被清除时（或按下“清除”按钮）触发此事件。
changeDate|当日期更改时触发此事件。
changeYear|当视图从十年视图变成年视图时触发此事件。
changeMonth|当视图从年视图变成月视图时触发此事件。

###快捷键导航

用快捷键导航时，当前选择的日期会聚焦，就像鼠标触发的效果，而当日期被清除或弹框隐藏时同样会失焦。

####上，下，左，右光标键

左/右键控制前后翻动一天，上/下键控制前后翻动一星期。

加入 shift 键后，上/左控制向前翻动一个月，下/右控制向后翻动一个月。

加入 ctrl/shift+ctrl 键后，上/右控制向前翻动一年，下/右控制向后翻动一年。

####回车键

当日期选择器可见时，回车切换当前聚焦的日期，不可见时，为正常的表单行为。

当日期被选中时，则clearDate事件会被触发，否则触发changeDate事件。如果autoclose为true，则按下回车将会同时隐藏选择器。

####逃逸(Escape)键

逃逸键将取消选择或隐藏选择器。

##本地存储 _localstorage.js_

本地存储将用于用户的个人浏览历史记录。因为IE8 不支持 html5 的本地存储机制，而且每个浏览器支持的情况不同，故在此封装了一个叫 BrowserStore 的类，并开放了一些方法以统一处理浏览器兼容性问题。

###用法

有两种调用方法：

    var storage = new BrowserStore();
    storage.get('history');

    browserStore.get('history');

###方法

####set(key, value)
以 key 为键值存储 value，可以是 String 或是以 String 方式表示的 JSON 字符串。

####get(key, callback)
获取以 key 为键值存储的数据，然后以获取的值为参数调用 callback 函数。

####remove(key)
删除以 key 为键值存储的数据。

####clear()
清除此次本地存储的所有数据。

##工具提示 _tips.js_

此工具提示主要用于表单验证提醒。

###实例

    <div class="form-row">
      <label for="for_input_email" class="form-label">电子邮件</label>
      <span class="form-act row input-row has-error">
        <input type="email" class="col-5" id="for_input_email" placeholder="请输入电子邮件" required>
        <span class="icon-alert caution">此项必填</span>
      </span>
    </div>

###用法

    var tips = new Tips(options);

###参数

同样支持两种方式传递参数：在 date 容器上写 `data-date-` 属性以及 JS 方式传入参数。例如：startDate 用 data- 属性表示就是 data-date-start-date，format 就是 data-date-format，daysOfWeekDisabled 就是 data-date-days-of-week-disabled。

名称 | 类型 | 默认值 | 描述
----|----|----|----
form|String|inline|Tips 的形态，是 inline 还是 block，会影响到 Tips 的标签名。
target|String/dom|'.form-row'|要插入的父元素
type|String|error|Tips 的类型，可以是 error 或 success。
class|String|caution|预置样式名。

###事件

事件类型|事件描述
----|----
onShow|当工具提示框显示时触发此事件。
onHide|当工具提示框隐藏时触发此事件。

###方法

####success(msg, options)
在提示成功消息时调用。包括以下参数：
- msg:成功消息语
- options:配置选项，参数同 Tips 的配置选项

####error(msg, options)
在提示失败/错误消息时调用。参数意义同上。

##表单验证 _validate.js_

表单验证包括两个方面，一是通过对所有的 form 表单提交事件进行统一拦截，然后根据不同的验证类型验证后进行统一化的提示信息，一是对提交事件进行处理，默认基于 ajax 方式进行异步提交，以在本页面输出后端返回的提交结果（是成功还是失败，失败了会有失败原因），当然还可以选择同步方式提交。所以表单验证是需要前后端结合更加紧密的工作形式。

###实例

    <form action="" method="post" role="form" data-validate-group=".form-act">
      <div class="form-row">
        <label for="for_input_uname" class="form-label">用户名</label>
        <span class="form-act row input-row">
          <input type="text" class="col-5" id="for_input_uname" placeholder="请输入用户名" required data-validate="alphanumber">
        </span>
      </div>
      <div class="form-row">
        <label for="for_input_email" class="form-label">电子邮件</label>
        <span class="form-act row input-row">
          <input type="email" class="col-5" id="for_input_email" placeholder="请输入电子邮件" required>
        </span>
      </div>
      <div class="form-row">
        <label for="for_input_password" class="form-label">密码</label>
        <span class="form-act row input-row">
          <input type="password" class="col-5" id="for_input_password" name="password" placeholder="请输入密码" required>
        </span>
      </div>
      <div class="form-row">
        <label for="for_input_npassword" class="form-label">确认密码</label>
        <span class="form-act row input-row">
          <input type="password" class="col-5" id="for_input_npassword" placeholder="请再次输入密码" required data-equalto="password">
        </span>
      </div>
      <div class="form-row">
        <label for="for_input_age" class="form-label">年龄</label>
        <span class="form-act row input-row">
          <input type="number" class="col-5" id="for_input_age" data-validate="posint">
        </span>
      </div>
      <div class="form-row">
        <label for="for_input_contact" class="form-label">联系方式</label>
        <span class="form-act row input-row">
          <input type="tel" class="col-5" id="for_input_contact">
        </span>
      </div>
      <div class="form-row">
        <label for="for_input_address" class="form-label">详细地址</label>
        <span class="form-act row input-row">
          <input type="text" class="col-5" id="for_input_address" minlength="10">
        </span>
      </div>
      <div class="form-row">
        <label for="for_input_zip" class="form-label">邮编</label>
        <span class="form-act row input-row">
          <input type="text" class="col-5" id="for_input_zip" maxlength="6" required data-validate="zip">
        </span>
      </div>
      <div class="form-row">
        <div class="form-act">
          <button type="submit" class="btn"><span><span>提交</span></span></button>
        </div>
      </div>
    </form>

###用法

由于是 JS 做了统一事件拦截，所以几乎完全不用写 JS 就可以完成自动验证。如果因为某种原因需要手动调用验证，只需要如下方式书写：

    $('#validate_form').validation(options);

###参数

同样支持两种方式传递参数：在 date 容器上写 `data-validate-` 属性以及 JS 方式传入参数。像以上实例中 `data-validate-group=".form-act"` 就是以 data- 方式传递参数。

名称 | 类型 | 默认值 | 描述
----|----|----|----
range|String|-|要验证表单元素的范围，是自身还是容器里所有的表单元素。
group|String|.form-row|要插入的验证提示信息的父元素。
tips|JSON|form:inline,class:caution|验证提示信息的配置参数，见工具提示一节。

###验证类型

一个验证器包含两部分----验证不通过提示消息和验证函数，下面列出内建的验证器能验证出的数据类型，所有区域性格式以中国为准。

名称 | 默认提示消息 | 描述
----|----|----
required|此项必填|必要性验证，检查值是否为空
minlength|输入不正确，至少x个字符|检查值的长度是否大于x
maxlength|输入不正确，最多x个字符|检查值的长度是否小于x
number|请填写数值|检查值的类型是否为数值型
digits|只能填写数字|检查值是否是0-9组成的数字
posint|请填写正整数|检查值是否为正整数（大于0的整数）
positive|请填写大于0的数值|检查值是否大于0
natural|请填写大于等于0的整数|检查值是否为大于等于0的整数（原自然数）
nonneg|请填写大于等于0的数值|检查值是否大于等于0
alpha|只能填写英文字母|检查值是否全是英文字母
alphanumber|请填写英文字母或者数字|检查值是否为英文字母和数字组成
alphanumcn|请填写英文字母、中文或数字|检查值是否为英文字母、中文或数字组成
uncnchar|不能填写中文字符|检查值是否含有中文
date|请填写日期，格式yyyy-mm-dd|检查值是否为正确的日期格式
email|请填写正确的email地址|检查值是否为正确的邮件格式
emaillist|请填写正确的email地址，以","或";"分隔|检查值是否为逗号或分号分隔的邮件地址列表
mobile|请填写正确的手机号码|检查值是否为正确的手机格式
landline|请填写正确的座机号码|检查值是否为正确的座机格式
tel|请填写正确的固话或手机号码|检查值是否为正确的手机或固话格式
zip|请填写正确的邮编|检查值是否为正确的邮编格式
url|请填写正确的网址|检查值是否为正确的url（包含ip）
area|请选择完整的地区|检查值是否选择了完整的地区
equalto|两次填写不一致|检查两个输入框填写的值是否一致（如密码验证）
oneoftwo|请至少填写一项|检查两个输入框至少有一个有值
onerequired|请至少选择一项|检查多个选项至少选择其中一个

###自定义验证类型

除了上述预定义的验证类型外，还可以自定义，有两种方式可以做到：一是用 html5 中定义的 pattern 属性定义正则匹配，验证器会提取其值并转换成完整匹配，所以 pattern 中不要加入前后的 ^ 和 $；一是用 JS 扩展 validatorMap，加入自定义的验证器名称和验证函数，如下就为一个 JS 自定义验证类型的例子：

    $.extend(validatorMap, {
        'custom_valid_name': ['自定义提示信息', function(element, value, type, parent) {
            //验证体
            //...

            return result;
        }]
    });

###验证过程

首先，在 document.body 一级对 click 事件进行了统一拦截，并匹配到点击的 form 区域，然后根据不同的验证类型进行统一的验证提示，如果通过则**异步**提交到后台进行处理，如果有正确/错误消息，则用 JSON 方式抛出消息提示，然后再用 callback 回调进行下一步操作。如果是非 form 区域，想要验证其中的表单是否符合要求，也可以通过 JS 调用 validator 方法实现。再或者如果是 a 链接，并且其 rel 属性为 _request，也可以被拦截变成异步方式提交，只是默认提交方式为 get。

##表单事务处理 _formplus.js_

包含表单填写/密码检测/表单元素兼容性的相关处理，分别定义并统一处理。

###密码强度检测组件

**函数：**
passwordStrength

**作用：**
在页面中填写密码时检测强度。

**参数：**

- value: 要检测的值
- key: 检测控件的dom
- className: 检测控件的初始样式名

**返回值：**
无

####实例

    <div class="form-row">
      <label for="for_input_password" class="col-2">密码</label>
      <span class="col-5">
        <input type="password" class="input-block auto-password-check-handle" id="for_input_password" placeholder="请输入密码">
        <span class="password-check password-weak">
          <q>安全程度</q>
          <span class="progress">
            <em class="weak">弱</em><em class="good">中</em><em class="strong">强</em>
          </span>
        </span>
      </span>
    </div>

由于此事件由系统统一处理，所以无需额外再写 JS。

###更换验证码

**函数：**
changeVerify

**作用：**
点击验证码图片或特定元素即更换验证码

**参数：**

- element: 要更换验证码的元素
- hasEvent: 是否事先绑定 click 事件

**返回值：**
无

**实例：**

    <div class="form-row verify-code">
      <label class="form-label"><em>*</em>验证码</label>
      <span class="form-act">
        <input type="text" class="verify-input input-st" name="verifycode" id="iptlogin" required="" size="4" maxlength="4">
        <img align="absmiddle" class="auto-change-verify-handle" id="membervocde" src="vcode.html" alt="验证码" width="70" height="35">
        <span class="inline auto-change-verify-handle">看不清？<a href="javascript:void(0);">换一张</a></span>
      </span>
    </div>

此事件也是由系统统一处理的，所以直接写html结构就可以了。

###全选/不选

**函数：**
checkAll

**作用：**
全部选中所有的选择框

**参数：**

- el: 事件源复选框，点击此元素触发全选动作
- elements: 需要被全部选择的复选框

**返回值：**
无

###兼容IE8+的占位符

**对象：**
Placeholder

**作用：**
页面加载时初始化所有带 placeholder 属性的输入框，为不支持 placeholder 的浏览器添加支持

**参数：**
无

**返回值：**
无

**实例：**

    <input type="text" name="" id="" placeholder="it's placeholder">
