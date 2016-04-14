* 原文链接 : [Leveling Up Your JavaScript](http://developer.telerik.com/featured/leveling-up-your-javascript/)
* 原文作者 : [Raymond Camden](http://developer.telerik.com/author/rcamden/)
* 译文出自 : [掘金翻译计划](https://github.com/xitu/gold-miner)
* 译者 : [Hikerpig](https://github.com/hikerpig)
* 校对者:


JavaScript 是一门入门容易，但是相当难以精通的语言。可现今一些文章总假设你已经精通了这门语言。

我从1995年JavaScript 还以LiveScript 名字出现的时候就开始用它了，但后来逐渐从前端开发撤回服务器的安全怀抱中，直到五年前才重拾。很高兴看到如今的浏览器更加的强大和易于调试。但JavaScript 已经演变得越来越复杂。不过最近我终于得出结论，我并不需要_精通_Javascript，只需要比以前更进一步就好。能成为一个"好"的JavaScript 开发者我便觉欣慰。

What follows are the tips and techniques that I have found to be useful and – most of all – _practical_ in terms of writing JavaScript: [organizing code](http://developer.telerik.com/featured/leveling-up-your-javascript/#organization); [linting](http://developer.telerik.com/featured/leveling-up-your-javascript/#linting); [testing](http://developer.telerik.com/featured/leveling-up-your-javascript/#testing); and [using browser developer tools](http://developer.telerik.com/featured/leveling-up-your-javascript/#devtools). While some of these may seem obvious to experienced JavaScript developers, it’s very easy to fall into bad habits when you are new to a language. These guidelines have helped me level up my skills and produce better experiences for my users. And _that’s_ our number one goal, right?

以下是我发现的一些_实用_的JavaScript 小技巧: [组织代码](http://developer.telerik.com/featured/leveling-up-your-javascript/#organization); [代码风格检验](http://developer.telerik.com/featured/leveling-up-your-javascript/#linting); [测试](http://developer.telerik.com/featured/leveling-up-your-javascript/#testing); 以及 [使用开发者工具](http://developer.telerik.com/featured/leveling-up-your-javascript/#devtools)。

> 你可在此处[下载](http://developer.telerik.com/wp-content/uploads/2016/01/code.zip)本文的样例代码。

## 组织代码

As a new JavaScript developer begins learning their craft, they will inevitably begin with a large block of code on top of their HTML page. It always starts simple. A simple bit of jQuery to autofocus a form field. Then maybe form validation. Then a little dialog widget that marketing just loves – you know, the ones that stop people from reading content so that they can Facebook Like the site itself. After a few iterations of this you’ve got a few hundred lines of JavaScript within an HTML file that probably has a few hundred lines of tags.

JavaScript 初学者总是不可避免地在他们的HTML 页面里写上一大坨代码。开始的时候都是很简单的，例如使用jQuery 给一个表单输入自动加上焦点，然后要加上表单验证，然后又要加上一些市场上走俏的模态框组件-就是那些阻止用户往下阅读内容好让他们在Facebook 上给网站点赞的东西。经过这些七七八八的功能迭代后你的一个文件里HTML 标签和JavaScript 都有了几百行。

It’s a mess. Stop doing it. It sounds simple, and I’m almost embarrassed to even write it down as a tip, but it is _very_ tempting to just whip up a quick script block on top of your page. Avoid that temptation like the plague please. Make it a habit when you create a new web site to go ahead and create an empty JavaScript file. Include it via the script tag and it will be ready for you when you begin adding interactivity and other client-side features.

别再继续这种乱七八糟的方式了。这个技巧太简单了我都不好意思单独把它列出来，但大家还_真的_很难拒绝这种把代码一坨扔上页面的偷懒做法。但请各位务必避之如瘟疫。新写网站时，请新建JavaScript 文件书写交互以及各种客户端功能的代码，然后用script 标签加载它，请养成这个好习惯。

Once you’ve gotten off the HTML page (doesn’t that feel cleaner?), the next problem you’ll run into is organization of the code itself. Those few hundred lines of JavaScript may work just fine, but the first time you have to debug or modify the code after a few months away you may find yourself wondering just where in the heck a particular function exists.

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

#### A Practical Module Sample

I began this discussion by complaining about the appropriateness of some of the examples I see online (see the Car example). Let’s build a simple example of this that actually matches a real world scenario. I’m going to keep it simple for the sake of this article, but applicable to something you may actually encounter in a real web application.

Your online game company, Lyntendo (don’t sue me), has a user sign up portal where users must create a game identity. You need to build a form where the user can select a name. The rules for identifiers are a bit weird:

*   Identifiers must begin with capital letter.
*   Identifiers must be two or more characters long.
*   Spaces are allowed, but no punctuation.
*   Identifiers can’t include certain “naughty” words.

Let’s mock this up in an incredibly simple form.

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

You can see the form I mentioned as well as a submit button. I’d also include text describing the rules I mentioned above, but I’m keeping it simple for now. Let’s look at the code.

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

    document.getElementById("submitButton").addEventListener("click", function(e) {

    	var identifier = document.getElementById("identifer").value;

    	if(validIdentifier(identifier)) {
    		return true;
    	} else { console.log('false');
    		e.preventDefault();
    		return false;
    	}
    });</badwords.length;>

Starting at the bottom, you can see I’ve got some basic code to get the elements from the page (yes, folks, in this case I didn’t use jQuery) and then listen for click events on the button. I get the value the user used for their proposed identifier and then pass it to my validation. The validation is nothing more than what I described above. The code isn’t _too_ messy, but as my validation rules increase and as I add other interactivity points to my page, it will get harder to work with. Let’s rewrite this as a module.

First, I created a new file called game.js and included it via a script tag in my index.html file. I then moved the contents of my validation logic inside a module.

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

This isn’t terribly different from before, but now it’s packaged into a variable called `gameModule` that has one API, `valid`. Now let’s look at app.js.

    document.getElementById("submitButton").addEventListener("click", function(e) {

    	var identifier = document.getElementById("identifer").value;

    	if(gameModule.valid(identifier)) {
    		return true;
    	} else { console.log('false');
    		e.preventDefault();
    		return false;
    	}
    });

Notice how much less code we have mixed in with our DOM listeners. All the functionality of validation (two functions and a list of bad words) are all now safely put away in the module making the code I have here easier to work with. Depending on your editor, you’ll also get code completion for the methods of your module.

Working with modules isn’t necessarily rocket science, but it is _cleaner_ and _simpler_ and that’s a real good thing!

## Linting

If you aren’t aware of the term, linting refers to checking your code for best practices and other problems. A noble cause, right? As nice as it is, I was always left with the impression that linting was something only fussy developers worried about. Obviously I want my code to be great. I also want time to play video games too. Having my code be less than some Perfect High Ideal(tm) while it still actually worked was perfectly fine by me.

But then…

You know those times when you rename a function and remind yourself that you’re going to fix it later?

You know those times when you define a function to accept two arguments and end up only ever using one?

You know how sometimes you write really stupid code? I mean code that won’t even come close to working. My favorite is `fuction` and `functon`.

Yeah, linting actually helps with that! As I keep saying, while it is probably obvious to everyone but me, the fact that linting was more than just best practices but also syntax and basic logic checking as well was news to me. Another factor also put it over the edge from “Will do when I have more time than I know what to do with” to “I will use this religiously” was the fact that most modern editors have it built in. My current editors (Sublime, Brackets, and Visual Studio Code), all support providing real time feedback on your code.

As an example, here is a report from Visual Studio Code on some code I intentionally wrote poorly. Honestly. I did it on purpose.

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zupgeoxdj20m80d1q40.jpg)

Visual Studio Code linting.

In the figure above, you can Visual Studio Code <strike>complaining</strike>pointing out a few mistakes in my code. Visual Studio Code’s linter, and most linters, have options for what you care about and what is considered an error (“must fix”) versus a warning (“should fix, stop being lazy”).

If you don’t want to install anything or try configuring your editor, a great way to test linting online is at [JSHint.com](http://jshint.com). JSHint is probably the most popular linter and is based on another linter, JSLint (Not confusing at all, honest). JSHint was created partially in response to how strict JSLint could be. While you can use JSHint directly in editors or via the command line, one of the easiest ways to to try it out is on the site itself.

![](http://ww1.sinaimg.cn/large/9b5c8bd8jw1f0zuppot76j20m804w0t8.jpg)

JSHint site.

While it may not be immediately obvious, the code on the left hand side is a live editor. On the right hand side is a live report based on that code. The easiest way to see this is to just introduce a simple error in the code. I began by changing the `main` function to `main2`:

    function main2() {
      return 'Hello, World!';
    }

    main();

Immediately, the site reported two errors from this. Now keep in mind, these aren’t syntax errors. Everything may look valid in the code above, but JSHint notices the problems that you may not (Of course, this is a 5 line block of code, but imagine a larger file with the function and call separated by many lines).

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zuq1qvjvj209t070wei.jpg)

JSHint errors.

How about a real example? In the code below (yep, now I _am_ using jQuery), I’ve written a simple bit of JavaScript to handle form validation. It’s trivial stuff, but probably half the JavaScript written today is doing something like this (Oh, and creating pop up modals asking you to “Like” the site. I love those). You can find this code in the demo_jshint folder as app_orig.js.

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

The code begins with two functions written to help with validation (for age and email). Then we have a `document.ready` block where we listen for the form submission. Values from three fields are fetched, checked if blank (or invalid), and then either an alert is fired that the form is invalid or things carry on (or in our example case, the form just sits there).

Let’s throw this into JSHint and see what we get:

![](http://ww3.sinaimg.cn/large/9b5c8bd8jw1f0zuqkjapdj20b90s5q3x.jpg)

JSHint errors for our demo.

Woah, that’s a lot! However, it looks like it’s actually similar problems that occured multiple times. This was common for me as I began using linters. I typically wasn’t making a lot of unique errors, just a lot of the same error again and again. The first one is easy enough – using triple equals for checking versus double equals. The short story on that it is stricter test for checking that the values are empty strings. Let’s fix that first (demo_jshint/app_mod1.js).

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

And here is the updated report from JSHint:

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zur1n2y4j20am0lb0t8.jpg)

JSHint errors for our demo.

Ok, we’re getting there. The next block is about “undefined variables.” That may seem odd. If you’re using jQuery, you know `<div exists. The issue with `badForm` is simpler – I forgot to `var` scope it. But how do we fix `<div? JSHint provides a way to configure how code is checked for issues. By adding a comment to our code, we can let JSHint know that the `<div variable exists as a global and is safe to use. Let’s add that and fix the missing `var` statement (demo_jshint/app_mod2.js):

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

And the updated report from JSHint:

![](http://ww4.sinaimg.cn/large/9b5c8bd8jw1f0zurgx350j209204gwed.jpg)

JSHint errors for our demo.

Woot! Almost done. This final issue is a perfect example of where JSHint can provide useful information that isn’t an error or best practice. In this case, I simply forgot that I wrote a function to handle age verification. You can see I’ve created `validAge`, but in the form checking area, I don’t use it. Maybe I should kill the function – it’s only one line – but it feels more proper to keep the function – just in case the validation gets more intense later on. Here is the final version of the code (demo_jshint/app.js):

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

This version finally “passes” the JSHint test. To be clear, this code isn’t perfect. Notice how I have a validation function called `validAge` and one called `invalidEmail`. One is positive while the other is negative. It would be better if I were consistent. Also notice how every time validation is run, jQuery is asked to fetch three items from the DOM. They really only needed to be loaded once. I should create those variables outside of the submission block and reuse them every time. As I said, JSHint isn’t perfect, but the final version of the code is definitely better than the first and it didn’t take long to update.

You can find linters for JavaScript ([JSLint](http://www.jslint.com) and [JSHint](http://www.jshint.com)), HTML ([HTMLHint](http://htmlhint.com/) and the [W3C Validator](https://validator.w3.org/)) and CSS ([CSSLint](http://csslint.net/)). Along with editor support, if you’re really fancy, you can also automate it with tools like Grunt and Gulp.

## Testing

I don’t write tests.

There. I said it. The world didn’t end. To be fair, I _do_ write tests (ok, I _try_ to write tests) when working on client projects, but for my main job I tend to do blog posts and demos of various features. I don’t write tests for these as they’re just proof of concepts and not production work. That being said, I can say that even before I became an evangelist and stopped doing “real” work, I often used the same excuses for not linting as for not writing tests. And it turns out that many of the same things that made linting easier are also helping out on the testing side.

First – many editors will actually generate tests for you. For example, in Brackets, you can use the [xunit](https://github.com/dschaffe/brackets-xunit) extension. It lets you right click on a JavaScript  
file and generate a test (in multiple popular testing framework formats).

![](http://ww1.sinaimg.cn/large/9b5c8bd8jw1f0zus4jz8sj20m80hymy4.jpg)

Test being created with xunit.

The extension will do its best to attempt to write a test based on existing code. This test will be a 'stub’ and you’ll need to flesh out some actual data in it, but the important thing is that it gets the grunt work out of your way.

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zuthjkyxj20m80hxjtd.jpg)

Test created with xunit.

Once you begin actually fleshing out those details, the extension will automatically run your tests for you. Honestly, at this point, not writing tests begins to plain look lazy.

![](http://ww2.sinaimg.cn/large/9b5c8bd8jw1f0zutuzzmij20m80l50we.jpg)

Test report.

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
