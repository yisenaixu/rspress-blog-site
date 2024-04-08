# XSS与CSRF

腾讯一面问了下，才意识到没有系统的学习相关完整内容（面试官也推荐系统学一下），所以来总结一下

## XSS(跨站脚本攻击)

**攻击者**在**网站上**注入**恶意脚本**，将一些**隐私数据**像cookie，session发送给攻击者

分为三类，存储型，反射型，基于DOM

### 存储型

#### 攻击流程

用户输入数据（恶意脚本）存储在服务端，浏览器请求服务器，传回恶意脚本并执行，盗取隐私数据传递给攻击者

所以是一种持久型稳定的攻击

#### 产生原因

对输入数据没有进行严格的过滤，前端可以检测输入数据是否含有类似脚本的，进行警告不发送请求，后端可以检测数据拒绝请求数据

### 反射型

与存储型类似，经过服务器，但是不存储在服务器，所以是非持久性攻击

### 基于DOM的XSS攻击

与服务器无关，主要是劫持传输的HTML页面内容，修改页面数据来注入脚本攻击

## 防御XSS

存储型和反射型是服务器的漏洞

### 对数据进行过滤

### HTTPOnly

禁止js访问cookie

### CSP(内容安全策略)

-  选择可以哪些域名发送的脚本可以被执行

## CSRF(跨站请求伪造)

攻击者引诱用户打开网站，利用用户登录状态（cookie）发起跨站请求，正常网站a，引诱用户在a打开b，b发送请求给a的网站接口，cookie的path和domain执行a主机，因此b发送的请求会携带cookie发送到a的服务器，执行恶意操作，整个过程攻击者不知道a的cookie，但是利用用户的浏览器通过b网站发送请求给，盗用了内容。

攻击者需要

- 让用户打开自己的网站
- 自己要攻击的接口地址

### 原理

#### 了解cookie和Session的机制

session是一种服务端验证登陆的方式，此外还有token。

用户网站登录后，网站标记用户产生sessionId,放入cookie中，此后用户发送请求携带cookie，sessionId被验证是否是目标用户。

### 自动发送get请求

通过图片src等方式自动发送请求

```html
<! DOCTYPE html> 
<html>
  <body>
    <h1>黑客的站点: CSRF攻击演示</h1>
	<img src="https://time.geekbang.org/sendcoin?user=hacker&number=100"> 
  </body>
</html>
```

### 自动发送post请求

通过form表单自动发送，提前注入值

```html
<!DOCTYPE html>
<html>
  <body>
	<h1>黑客的站点: CSRF攻击演示</h1>
	<form id= 'hacker-form' action="https://time.geekbang.org/sendcoin" method=POST>
	  <input type="hidden" name="userll" value="hacker" />
	  <input type="hidden" name="numberll" value="100" />
	</form>
	<script> document.getElementById ('hacker-form').submit(); </script> 
  </body>
</html>
```

### 引诱用户点击链接

直接让用户点击恶意链接发送请求

## 防御CSRF

### COOKIE SameSite

- strict 阻止所有第三站点携带cookie
- lax  两种情况携带cookie http方法是安全的(get方法)，因为不修改内容  请求更改顶级URL
- none 不阻止

### 验证请求的来源站点 COOKIE Referer

### CSRFTOKEN 服务器返回一个token，隐藏注入到表单中用于身份验证

![已经嵌入了一个额外的字段csrf-token](https://s2.loli.net/2024/04/09/9Rd5pZtLx6srTXc.png)
