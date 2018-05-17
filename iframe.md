---
title: iframe跨域问题
date: 2018-03-11 16:01:06
tags: [javascript, 跨域]
categories: 学习
---

## 一、主域相同
- 子页面和父页面都加上 `document.domain = "子页面域名"`
<!-- more -->
---

## 二、跨全域高度自适应
### 1、postMessage(不兼容ie8-)
  + 父页面
  ```html
  <iframe onload="resizeCrossDomainIframe('iframepage', '子页面域名');" src="父页面")

  function resizeCrossDomainIframe(id, other_domain) {
      var iframe = document.getElementById(id);

      window.addEventListener('message', function(event) {

          if (event.origin !== other_domain) return; // only accept messages from the specified domain
          if (isNaN(event.data)) return; // only accept something which can be parsed as a number

          var height = parseInt(event.data) + 32; // add some extra height to avoid scrollbar
          iframe.height = height + "px";
      }, false);
  }
  ```
  + 子页面
  ```html
  <body onload="parent.postMessage(document.body.scrollHeight, '父页面域名');">
  ```

### 2、使用代理页面
- 父页面
```html
<iframe id="parentPage" name="mainFrame" scrolling="no" src="子页面" frameborder="0" width="100%"></iframe>
```

- 代理页面(与父页面同域名)
``` html
<script>
var b_iframe = window.parent.parent.document.getElementById("parentPage");
var hash_url = window.location.hash;
if (hash_url.indexOf("#") >= 0) {
  var hash_width = hash_url.split("#")[1].split("|")[0] + "px";
  var hash_height = hash_url.split("#")[1].split("|")[1] + "px";
  b_iframe.style.width = hash_width;
  b_iframe.style.height = hash_height;
}
</script>
```

- 子页面
```html
<iframe id="agentPage" height="0" width="0" src="中间页面" style="display:none"></iframe>
<script type="text/javascript">
(function autoHeight() {
    var b_width = Math.max(document.body.scrollWidth, document.body.clientWidth);
    var b_height = Math.max(document.body.scrollHeight, document.body.clientHeight);
    var c_iframe = document.getElementById("agentPage");
    c_iframe.src = c_iframe.src + "#" + b_width + "|" + b_height;
})();
</script>
```
