* 原文链接 : [Leveling Up Your JavaScript](http://developer.telerik.com/featured/leveling-up-your-javascript/)
* 原文作者 : [Raymond Camden](http://developer.telerik.com/author/rcamden/)
* 译文出自 : [掘金翻译计划](https://github.com/xitu/gold-miner)
* 译者 : [Hikerpig](https://github.com/hikerpig)
* 校对者:


JavaScript 是一门入门容易，但是相当难以精通的语言。可现今一些文章总假设你已经精通了这门语言。

我从1995年JavaScript 还以LiveScript 名字出现的时候就开始用它了，但后来逐渐从前端开发撤回服务器的安全怀抱中，直到五年前才重拾。很高兴看到如今的浏览器更加的强大和易于调试。但JavaScript 已经演变得越来越复杂。不过最近我终于得出结论，我并不需要_精通_Javascript，只需要比以前更进一步就好。能成为一个"好"的JavaScript 开发者我便觉欣慰。

以下是我发现的一些_实用_的JavaScript 小技巧: [组织代码](http://developer.telerik.com/featured/leveling-up-your-javascript/#organization); [代码风格检验](http://developer.telerik.com/featured/leveling-up-your-javascript/#linting); [测试](http://developer.telerik.com/featured/leveling-up-your-javascript/#testing); 以及 [使用开发者工具](http://developer.telerik.com/featured/leveling-up-your-javascript/#devtools)。里面有几条对有经验的JavaScript 开发者来说可能很显而易见，但是语言初学者来很容易养成坏习惯。这些技巧提高了我的技术水平，同时也为我的用户创造了更好的体验。_这_难道不是我们最大的目标么。

> 你可在此处[下载](http://developer.telerik.com/wp-content/uploads/2016/01/code.zip)本文的样例代码。

## 组织代码

JavaScript 初学者总是不可避免地在他们的HTML 页面里写上一大坨代码。开始的时候都是很简单的，例如使用jQuery 给一个表单输入自动加上焦点，然后要加上表单验证，然后又要加上一些市场上走俏的模态框组件-就是那些阻止用户往下阅读内容好让他们在Facebook 上给网站点赞的东西。经过这些七七八八的功能迭代后你的一个文件里HTML 标签和JavaScript 都有了几百行。

别再继续这种乱七八糟的方式了。这个技巧太简单了我都不好意思单独把它列出来，但大家还_真的_很难拒绝这种把代码一坨扔上页面的偷懒做法。但请各位务必避之如瘟疫。新写网站时，请新建JavaScript 文件书写交互以及各种客户端功能的代码，然后用script 标签加载它，请养成这个好习惯。

把JavaScript 从HTML 页面中剥离以后（干净多了是不是？），下一个问题就是关于这些代码的组织形式了。这几百行JavaScript 也许功能没啥问题，但是几个月后，一旦你开始想调试或是改点东西，你可能特么找不到某个函数在哪了。

若仅仅从HTML 中剥离出来是不够的，那么下一个解决方案是什么呢？

### 框架！

显然解决方案是框架。把所有东西用AngularJS，或Ember，或React 或其他几百个框架中某一个写一遍。哼哧哼哧地把整个网站重写为一个单页应用，用上MVC 什么的。

或者根本不需要。当然了，别误会我，在编写应用的时候我喜欢用Angular，但是一个"应用"和一个页面的交互复杂度是有区别的。一个用上Ajax 技术的产品目录页和Gmail 也是有区别的 - 起码几十万行代码的区别。那么，如果不走框架这条路的话，还有什么选择呢？

### 设计模式

设计模式是对"这是过去人们解决问题的一个方法"这句话的高级说法。Addy Osmani 写过一本关于此的很好的书，[学习JavaScript 设计模式](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)，可以免费下载阅读。我推荐这本书。但是我对它（以及类似的关于此议题的讨论）有点小看法，因为最后你们写的代码可能变成这样:

    var c = new Car();
    c.startEngine();
    c.drive();
    c.soNotRealistic();

对我来说，设计模式在抽象层面上是有意义的，但是在_实际工作中_，没有什么用。从实际项目中的网页和代码里提取模式，是件很难的事情。

#### 模块

在所有我看过的设计模式中，我觉得模块模式是最简单也是最容易应用到现有代码里的。

纵而览之，模块模式就是一群代码之外加了个包装。你把抽出一群功能相关的代码扔到一个模块里，决定需要暴露的部分，也可以把一个模块里的代码放到不同的文件里。然后建立一个易于在项目之间共享的代码黑匣。

看看这个简单的例子。此处的语法咋看可能有点奇怪，起码我一开始是这样觉得的。我们先从"包装"部分开始看，然后我再解释其余部分。

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zumg7z7gj20kp05ojru.jpg)

模块模式的包装。

只有我一个人被这些括号搞晕了么？我的脑子搞不明白这里是干嘛的，这还是在我懂JavaScript 的前提下。其实这里如果从里往外看，就清晰很多。

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zuncbxnuj20m805lgly.jpg)

模块的内部只是个普通的函数。

从一个简单的函数开始，在里面你需要定义你的模块的实际功能代码。

You begin with a simple function. This is where you will define the methods you can call on your module.

![](http://ww1.sinaimg.cn/large/9b5c8bd8jw1f0zunvmhafj20m805ot94.jpg)

圆括号使得这个函数自动运行。

最后的圆括号会让该函数立即执行。我们在函数里返回了什么，模块就是什么。此时我们这里还是空的。不过此时上图高亮的部分还_不是_合法的JavaScript。那么，怎样让它变得合法呢？

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zuoenvzjj20m805mdg9.jpg)

外边的圆括号开始发功了。

在`function() { }()` 外的圆括号使得此处成为合法JavaScript。你要是不信我，就打开开发者工具的控制台自己输入看看。

这样就是我们一开始看到的。

![](http://ww1.sinaimg.cn/large/9b5c8bd8jw1f0zuotyvzej20m808ngm7.jpg)

返回值被赋给一个变量。

最后一件事是把返回值赋给一个变量。尽管我自己完全懂得这里，但每次我看见这种代码我都得暂停一秒钟来提醒自己这是什么鬼。说来也不怕羞，我在编辑器里存着这段空模块代码随时快手粘贴。

当我们终于征服了这坨诡异的语法之后，究竟真正的模块模式看起来是怎样的？

    var counterModule = (function() {
    	var counter = 0;

    	return {
    		incrementCounter: function () {
    			return counter++;
    		},
    		resetCounter: function () {
    			console.log("counter value prior to reset: " + counter );
    			counter = 0;
    		}
    	};

    }());

这段代码创建了一个叫做`counterModule` 的模块。它有两个函数，`incrementCounter` 和 `resetCounter`。可以这样使用它们：

    console.log(counterModule.getCounter()); //0
    counterModule.incrementCounter();
    console.log(counterModule.getCounter()); //1
    counterModule.resetCounter();
    console.log(counterModule.getCounter()); //0

主要的思想就是把`counterModule` 里的代码好好地封装起来。封装是计算机科学基础概念，将来JavaScript 还会提供更简单的封装方法，不过就现在来说，我觉得模块模式已是个超级简单和使用的组织代码方案。

#### 一个实用的模块案例

吐槽完网上看到的样例（例如上面那个Car的例子）。我们现在需要编写一个符合实际场景需求的简单代码。限于本文篇幅，我会写得尽量简单，但会贴合你在遇到实际web 项目时的情况。

假设你的网游公司愣天堂 (任粉莫喷)，在用户要创建游戏人物的时候需要一个注册页面。你需要一个可以让用户选择名字的表单。构建名字的规则有点诡异：

*   必须以大写字母开头
*   长度不小于2
*   允许空格，但是不能有标点
*   不能有"敏感"词汇

先写下这个超简单的表单。

    <html>
    	<head>

    	</head>

    	<body>

    		<p>Text would be here to describe the rules...</p>

    		<form>
    			<input type="text" placeholder="Identifer">
    			<input type="submit" value="Register Identifer.">
    		</form>
    		<script src="app.js"></script>
    	</body>
    </html>

除了我描述的输入框，表单里还有个提交按钮。然后我加了些有关上面提到的规则的说明，先尽量保持精简。让我们来看看代码。

    var badWords = ["kitten","puppy","beer"];
    function hasBadWords(s) {
    	for(var i=0;i<badwords.length; i++)="" {="" if(s.indexof(badwords[i])="">= 0) return true;
    	}
    	return false;
    }

    function validIdentifier(s) {
    	//是否为空
    	if(s === "") return false;
    	//至少两个字符
    	if(s.length === 1) return false;
    	//必须以大写字母开头
    	if(s.charAt(0) !== s.charAt(0).toUpperCase()) return false;
    	//只允许字母和空格
    	if(/[^a-z ]/i.test(s)) return false;
    	//没有敏感词
    	if(hasBadWords(s)) return false;
    	return true;
    }

    document.getElementById("submitButton").addEventListener("click", function(e) {

    	var identifier = document.getElementById("identifer").value;

    	if(validIdentifier(identifier)) {
    		return true;
    	} else { console.log('false');
    		e.preventDefault();
    		return false;
    	}
    });</badwords.length;>

从代码底部开始，你看到我写了点基本的获取页面元素的代码（没错伙计们这里我没有用jQuery）然后监听button 上的点击事件。拿到用户输入的用户名字段然后传给验证函数。验证的内容不多不少我之前描述的那些。这里代码还没有_太_乱，不过随着之后验证逻辑的增长和页面交互逻辑的增加，代码会越来越难以维护。所以我们把这里重写为模块吧。

首先，创建game.js 文件并在index.html 中使用script 标签引入它。然后把验证逻辑移到一个模块里。

    var gameModule = (function() {

    	var badWords = ["kitten","puppy","beer"];

    	function hasBadWords(s) {
    		for(var i=0;i<badwords.length; i++)="" {="" if(s.indexof(badwords[i])="">= 0) return true;
    		}
    		return false;
    	}

    	function validIdentifier(s) {
    		//is it blank?
    		if(s === "") return false;
    		//must be at least 2 chars
    		if(s.length === 1) return false;
    		//must begin with a capital letter
    		if(s.charAt(0) !== s.charAt(0).toUpperCase()) return false;
    		//only letters and spaces
    		if(/[^a-z ]/i.test(s)) return false;
    		//no bad words!
    		if(hasBadWords(s)) return false;
    		return true;
    	}

    	return {
    		valid:validIdentifier
    	}

    }());</badwords.length;>

现在这里和之前没有翻天覆地的差别，只不过是被封装成了一个有一个`valid` 接口的`gameModule` 变量。接下来我们来看看app.js 文件。


    document.getElementById("submitButton").addEventListener("click", function(e) {

    	var identifier = document.getElementById("identifer").value;

    	if(gameModule.valid(identifier)) {
    		return true;
    	} else { console.log('false');
    		e.preventDefault();
    		return false;
    	}
    });

看看我们的DOM 监听函数里少了多少代码。所有的验证逻辑（两个函数和一个敏感词列表）被安全地移到了模块里后，这里的代码就更好维护了。如果你的编辑器支持，你在此处还能有模块方法名的代码补全。

模块化不是什么高深的东西，但由此在_干净_和_精简_指标上的提升，绝对是好事情。

## 代码检验(Linting)

简单给初闻者解释下，代码检验表示使用最佳实践和一些避免出错的规则对代码进行检查。很高大上对不对？我以前以为只有挑剔过头的开发者才会考虑这个。当然了，我期望自己写出超棒的代码，但我也需要腾出时间玩游戏。就算我的代码够不上某些高大上的完美标准，但它能好好工作我就能满意了。

然而...

记不记得你重命名了个函数然后提醒自己之后一定会改？

记不记得你创建了个有两个形参的函数，其实最后只用了一个？

记不记得你写过多少蠢代码？我说的是那些根本不能工作的，类似我最爱的`fuction` 和`functon`。

代码检验就是这时候站出来帮你的！除了我之外大家都知道，代码检验不只有风格的最佳实践，还包含语法和基本的逻辑检验。还有一个让我从"等我有时间以后一定或做的" 跳到"我会虔诚地遵循" 的原因，那就是几乎所有现代编辑器都默认支持此。我目前用的编辑器（Sublime, Brackets 和Visual Studio Code）都支持代码实时检验和反馈。

举个例子，以下是Visual Studio Code 对我一段很挫的代码的提示。当然了，我是故意写得很挫的。

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zupgeoxdj20m80d1q40.jpg)

Visual Studio Code 代码检验。

上图中，你能看到Visual Studio Code <strike>抱怨</strike>我代码中的几个错误。Visual Studio Code 的代码检验器，和大多数检验器一样，可配置你关心的检验规则以及对其中"错误"（必须修正）和"警告"(别偷懒啊，总要修复的)的定义。

如果你不想安装任何东西，也不想折腾编辑器，另一种好方法是使用[JSHint.com](http://jshint.com)在线检验代码。JSHint 差不多是最流行的检验器，它基于另一个检验其间器JSLint (谁说它们长得像来着？)。JSHint 的诞生一部分是由于JSLint 太过严格。你可以直接在编辑器里或是通过命令行使用JSHint，最简单的体验方法是在它的网站上试试。

![](http://ww1.sinaimg.cn/large/9b5c8bd8jw1f0zuppot76j20m804w0t8.jpg)

JSHint 网站。

乍看可能不太明显，其实左边的代码是在一个实时编辑器里的。右边的是一份对左边代码的检验报告。把它弄出来的最简单方式是在代码里随便写错点什么。我这里把`main` 函数名改成了`main2`。

    function main2() {
      return 'Hello, World!';
    }

    main();

马上，网页就对此给我报了两个错误。注意了，这并不是语法错误。代码在语法上是完全没问题的，但是JSHint 发现了你可能忽视了的问题所在（当然了，这里代码只有5行，但想象下一个大文件里函数定义和调用之间隔了好多行的时候）。

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zuq1qvjvj209t070wei.jpg)

JSHint 错误。

来个实例如何？以下的代码（嗯现在我_是_用了jQuery），我写了点简单的JavaScript 做表单验证。都是些鸡毛蒜皮的东西，不过今天几乎一般的JavaScript 代码做的都是这些事（哦哦当然还有创建弹出框然后问你要不要"赞"这个网站。真特么爱死这些了）。你可以在demo_jshint 文件夹的app_orig.js 里详细查看。

    function validAge(x) {
    	return $.isNumeric(x) && x >= 1;  
    }

    function invalidEmail(e) {
      return e.indexOf("@") == -1;
    }

    $(document).ready(function() {

    	$("#saveForm").on("submit", function(e) {
    		e.preventDefault();

    		var name = $("#name").val();
    		var age = $("#age").val();
    		var email = $("#email").val();

    		badForm = false;

    		if(name == "") badForm = true;
    		if(age == "") badForm = true;
    		if(!$.isNumeric(age) || age <= 0)="" badform="true;" if(email="=" "")="" if(invalidemail(email))="" console.log(badform);="" if(badform)="" alert('bad="" form!');="" else="" {="" do="" something="" on="" good="" }="" });="" });<="" code=""></=>

开始两个辅助验证的函数（对年龄和email）。然后是`document.ready` 代码块里对表单提交的监听。获取表单中三个字段的值，检查是否为空（即无效），若表单无效就弹出警告，否则继续（在我们的例子里，什么也没发生，表单没变化）。

扔到JSHint 上看看发生了啥：

![](http://ww3.sinaimg.cn/large/9b5c8bd8jw1f0zuqkjapdj20b90s5q3x.jpg)

JSHint 对我们样例代码的报错。

哇塞好多东西！看起来是类似的问题出现了多次。我开始用检验器的时候挺常见。我并没有弄出很多种错误，而仅仅是同种错误的重复。第一个非常简单 - 检查相等时使用三等号替代双等号。简单来说就是用更严格的标准检测空字符串。先修复这个(demo_jshint/app_mod1.js)。

    function validAge(x) {
    	return $.isNumeric(x) && x >= 1;  
    }

    function invalidEmail(e) {
      return e.indexOf("@") == -1;
    }

    $(document).ready(function() {

    	$("#saveForm").on("submit", function(e) {
    		e.preventDefault();

    		var name = $("#name").val();
    		var age = $("#age").val();
    		var email = $("#email").val();

    		badForm = false;

    		if(name === "") badForm = true;
    		if(age === "") badForm = true;
    		if(!$.isNumeric(age) || age <= 0)="" badform="true;" if(email="==" "")="" if(invalidemail(email))="" console.log(badform);="" if(badform)="" alert('bad="" form!');="" else="" {="" do="" something="" on="" good="" }="" });="" });<="" code=""></=>

JSHint 报告变成了:

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zur1n2y4j20am0lb0t8.jpg)

JSHint 对我们样例代码的报错。

算是解决了。下一个错误类型是"未声明变量"。看着有点诡异。如果使用jQuery 的话，你知道`$` 是存在的。`badForm` 的问题就更简单点 - 我忘记用`var`声明它了。那我们怎么解决`$`的问题呢？JSHint 提供了对代码规则检验方法的配置。在代码里加上一个注释以后，我们告诉JSHint`$`变量是作为全局变量可以放心使用。接下来我们补上这个注释，并且加上丢失的`var`声明（demo_jshint/app_mod2.js）。

    /* globals $ */
    function validAge(x) {
    	return $.isNumeric(x) && x >= 1;  
    }

    function invalidEmail(e) {
      return e.indexOf("@") == -1;
    }

    $(document).ready(function() {

    	$("#saveForm").on("submit", function(e) {
    		e.preventDefault();

    		var name = $("#name").val();
    		var age = $("#age").val();
    		var email = $("#email").val();

    		var badForm = false;

    		if(name === "") badForm = true;
    		if(age === "") badForm = true;
    		if(!$.isNumeric(age) || age <= 0)="" badform="true;" if(email="==" "")="" if(invalidemail(email))="" console.log(badform);="" if(badform)="" alert('bad="" form!');="" else="" {="" do="" something="" on="" good="" }="" });="" });<="" code=""></=>

JSHint 报告变成了:

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zurgx350j209204gwed.jpg)

JSHint 对我们样例代码的报错。

哇哦快好了！最后一个问题恰好的展示了JSHint 在提示最佳代码风格实践和指出错误以外的用途。这里我忘了写过一个处理年龄验证的函数。你看我创建了`validAge`，但是在表单验证代码区域没使用它。也许我该删了这个函数- 反正也只有一行，但我觉得留下来更好 - 以免以后验证逻辑越来越复杂。以下就是完整的代码了(demo_jshint/app.js)。

    /* globals $ */
    function validAge(x) {
    	return $.isNumeric(x) && x >= 1;  
    }

    function invalidEmail(e) {
      return e.indexOf("@") == -1;
    }

    $(document).ready(function() {

    	$("#saveForm").on("submit", function(e) {
    		e.preventDefault();

    		var name = $("#name").val();
    		var age = $("#age").val();
    		var email = $("#email").val();

    		var badForm = false;

    		if(name === "") badForm = true;
    		if(age === "") badForm = true;
    		if(!validAge(age)) badForm = true;
    		if(email === "") badForm = true;
    		if(invalidEmail(email)) badForm = true;

        	console.log(badForm);
    		if(badForm) alert('Bad Form!');
    		else {
    			//do something on good
    		}
    	});
    });

最终版本"通过"了JSHint 的测试。虽然实际上，并不完美。注意到我两个检验函数一个叫`validAge`一个叫`invalidEmail`，一个返回肯定一个返回否定。更好的做法是保持一致性。还有每次这个验证函数开始，jQuery 需要获取DOM 中的三个元素，其实它们只需要被获取一次。我应该在表单提交回调函数外创建这些变量，每次验证的时候重复使用。如我所言，JSHint 不是完美的，但代码最终版本绝对比第一版要好很多，我的修改也没有花多少时间。

不同用途的代码检验器有JavaScript([JSLint](http://www.jslint.com)和[JSHint](http://www.jshint.com))，HTML([HTMLHint](http://htmlhint.com/)和[W3C Validator](https://validator.w3.org/))和CSS ([CSSLint](http://csslint.net/))。如果编辑器支持，你还能更酷地用Grunt 和Gulp 工具对这些进行自动化。

## 测试

我不写测试。

没错，我就这么放话了。世界不会停止转动。不过其实，在开发客户端项目时，我其实_是_写测试的（好啦实际是我_尝试_去写测试），但是我的主要工作写博客和给写多种功能的样例代码。因为它们只是观念的验证而不需要实际工作，所以我不需要为它们写测试。其实，在我成为布道者和不做"实际"工作之前，我也是敢这么放话的，不写测试的借口和不使用代码检验器一样。不过一些给检验器加分的因素放在测试上也很好用。

首先- 许多编辑器会为你自动生成测试代码。例如在Brackets 中，可以使用[xunit](https://github.com/dschaffe/brackets-xunit) 扩展。借助它你只要在JavaScript 文件上调出右键菜单就能生成测试代码（支持多种流行测试框架格式）。

![](http://ww1.sinaimg.cn/large/9b5c8bd8jw1f0zus4jz8sj20m80hymy4.jpg)

xunit 创建的测试。

该扩展基于现存代码去生成测试代码。生成的测试代码只是个模板，你需要自己去填写具体内容，这避免了一些无聊的重复劳动。

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zuthjkyxj20m80hxjtd.jpg)

xunit 创建的测试。

填满了测试细节以后，该扩展会帮你自动执行测试。都到了这份上了，不写代码基本上就只是懒了。

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zutuzzmij20m80l50we.jpg)

测试报告。

你也许听过TDD(测试驱动开发)。说的是在写具体代码之前先把单元测试写好。本质上是测试主导你的开发。写下代码并看它通过测试的时候，你将欣慰自己没走错路。

You’ll probably have heard of TDD (Test Driven Development). This is the concept of writing unit tests before any actual functionality. Essentially, the idea is that your tests help drive your development. As you write your code and see your tests begin to pass, you have some assurance that your on the right path.

I think that’s a noble idea, but it may be difficult to achieve for everyone. How about starting simpler? Imagine you’ve got a set of existing code that – as far as you know – works just fine. Then you discover a bug. Before fixing the bug, you can create a test to verify the bug, fix it, then use the test to ensure it _stays_ fixed as you work on it in the future. As I said, this isn’t the ideal path, but can be a way to gently ramp up into including testing in all stages of your development.

For our sample code with a bug we’ll use a little function that I wrote that tries to shorten numbers. So for example, 109203 could be simplified as 109K. An even bigger number like 2190290 could be turned into 2M. Let’s look at the code and then I’ll demonstrate the bug.

    var formatterModule = (function() {

    	function fnum(x) {
    		if(isNaN(x)) return x;

    		if(x < 9999) {
    			return x;
    		}

    		if(x < 1000000) {
    			return Math.round(x/1000) + "K";
    		}
    		if(x < 10000000) {
    			return (x/1000000).toFixed(2) + "M";
    		}

    		if(x < 1000000000) {
    			return Math.round((x/1000000)) + "M";
    		}

    		if(x < 1000000000000) {
    			return Math.round((x/1000000000)) + "B";
    		}

    		return "1T+";
    	}

    	return {
    		fnum:fnum
    	}

    }());

Maybe you see the issue right away? Give up? When given 9999 as an input, it returns 10K. Now, that might be a useful shortening, but the code is supposed to treat all numbers below 10K as their original value. It is an easy enough correction, but let’s use this as an opportunity to add a test. For our testing framework we’ll use [Jasmine](http://jasmine.github.io/). Jasmine has a great, easy to understand language for writing tests and a simple way to run them. The quickest way to get started is to download the library. Once you’ve done that and extracted it, you’ll find a file called SpecRunner.html. This file handles loading your code, loading a test, and then running the tests and creating a pretty report. It requires the lib folder from the zip but you can begin by copying both SpecRunner and the lib to someplace on your web server.

Open up SpecRunner.html and on top you’ll see:

    <!-- include source files here... -->
    script tags here...

    <!-- include spec files here... -->
    more script tags here...

Under the first comment you’ll want to remove the existing line and simply add a script tag pointing to the code containing your code. If you’ve got the zip file for this article you can see my code in demo4 in a file called formatter.js. Next you’ll want to add a script tag pointing to the spec, or test. Maybe you haven’t seen Jasmine before, but take a look at the spec. It is _very_ readable, even to the untrained eye.

    describe("It can format numbers nicely", function() {

    	it("takes 9999 and returns 9999", function() {
    		expect(9999).toBe(formatterModule.fnum(9999));
    	});

    });

Basically my test is saying that when 9999 is passed to the library it should get 9999 out again. If you open the SpecRunner.html in your browser you can see it reporting the failure.

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zuu5bbhaj20m80e1q61.jpg)

Report of the failing test.

The fix, is rather simple. Change that conditional using 9999 to 10000:

    if(x < 10000) {
    	return x;
    }

Now when you run the tests you’ll see a much happier picture:

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zuuh4xj8j20m804y74k.jpg)

Report of the passing test.

Looking at the module, you can probably think of a number of related tests that would really flesh out the suite. In general, there’s nothing wrong with going overboard on your testing and trying to cover every possible use of your code possible. Consider the awesome date/time library [Moment.js](http://momentjs.com/). It has – I kid you not – over fifty-seven thousand tests. That’s thousand. You read it right.

Other options for testing JavaScript code include [QUnit](https://qunitjs.com/) and [Mocha](http://mochajs.org/). As with linting you can automate testing with tasks runners like Grunt, and you can even go full stack and test the browser itself with [Selenium](http://www.seleniumhq.org/).

## Browser Developer Tools

The final tool I’ll mention are those within the browser itself – the dev tools. You can find multiple articles, presentations, and videos on this topic so I won’t say much more about it, outside of my belief that, amongst everything I’ve discussed today, this is probably the one thing I’d call **required knowledge** for web developers. It is perfectly fine to write broken code. And it is perfectly fine to not know everything. But browser dev tools at least help you _find_ the broken bits. At that point a solution is typically one Google search away.

If I can add one final piece of advice here, it is that you should not focus on only one browser’s dev tools. I was playing with App Cache a few years ago (yes, I’m a glutton for punishment) and ran into issues with my code working in Chrome. Of course, I had my dev tools open, but it wasn’t helping. On a whim, I opened up the code in Firefox and used their dev tools, and **immediately** I discovered the issue. Firefox simply reported more information about the request compared to Chrome. Running it one time was all I needed to correct the issue. (Ok, that’s a lie. Firefox showed me the problem but it took a bit longer to fix.) If you find yourself stuck, just open another browser and see if the error reporting offers a different perspective.

On the off chance you’ve never actually _seen_ your browser tools in action, here are instructions on how to view them in all the major browsers, as well as the best link to get started for reading more.

### Google Chrome

To open dev tools, click the hamburger menu icon on the upper right of your browser, select “More Tools”, and then “Developer Tools”. You can also open up dev tools using your keyboard. For example, on OSX the combination is `CMD+SHIFT+C`. You can find documentation for Chrome’s dev tools at [“Chrome DevTools Overview”](https://developer.chrome.com/devtools).

### Mozilla Firefox

To open dev tools, click “Tools” in the main menu, then “Web Developer” and “Toggle Tools”. Note that Firefox has a cool toolbar that can be used to issue commands and make opening up dev tools even easier. This can be enabled from the same menu. You can learn more at [Firefox Developer Tools](https://developer.mozilla.org/en-US/docs/Tools).

### Apple Safari (AKA the browser for watching Apple keynotes)

Before you can work with dev tools, you have to enable the “Develop” menu. Go to Safari preferences, then “Advanced”, and click “Show Develop menu in menu bar.” Then you can select the “Develop” menu and use “Show Web Inspector” (or the three items below it) to open dev tools. You can read more about this at [“About Safari Web Inspector”](https://developer.apple.com/library/safari/documentation/AppleApplications/Conceptual/Safari_Developer_Guide/Introduction/Introduction.html).

### Internet Explorer

You can open Internet Explorer’s Dev Tools by clicking the gear icon in the upper right hand of the browser (or by presing F12). You can read more here, [“Using the F12 developer tools”](https://msdn.microsoft.com/library/bg182326%28v=vs.85%29).

## Learning More

Sometimes it seems as if our job as developers is never complete. While writing this article, did you know that thirteen more JavaScript frameworks were released? True story! So here is some final advice on how to learn and how to keep up – as best as possible.

For learning, I focus my attention on the [Mozilla Developer Network](http://developer.mozilla.org) (when you google, try preprending your term with “mdn”), [CodeSchool](http://www.codeschool.com) (a commercial video training company with great content), and [Khan Academy](https://www.khanacademy.org/). I want to specifically call out Mozilla Developer Network (MDN) as I avoided it for years thinking it was a Netscape/Firefox only site. That was pretty dumb.

Another suggestion is to just read code! Many of us have used jQuery, but have you ever actually opened up the file to take a look at how it is built? Reading other people’s code can be a great way to get exposed to other techniques and methods. While it may be scary, I also strongly encourage you to share your own code. Not only will you get the benefit of having an extra pair of eyes (or many thousands of them) look at your code, you may actually help others as well. A few years back I was watching a junior programmer share some code and while he made some fairly typical “noob” mistakes, he also used some techniques that were outright brilliant.

For keeping up on the latest news, I subscribe to the various “weekly” newsletters run by [Cooper Press](http://cooperpress.com). They have an HTML weekly, a JavaScript one, a Node one, a Mobile one, and so forth. It can get overwhelming, but do what you can do. When I see that some new tool has been released that does Foo and I don’t particularly _need_ Foo at the moment, I don’t even try to learn this tool. I just remember, “Hey, there’s a tool that does Foo.” so that when I need it in the future, I can dedicate the time then.

_Header image courtesy of [Lemsipmatt](https://flic.kr/p/5PS638)_
