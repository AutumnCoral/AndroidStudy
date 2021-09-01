# 架构模式

## MVC/MVP/MVVM 

1,在浏览器输入一个网址访问网站都是GET请求;再FORM表单中，可以通过设置Method指定提交方式为GET或者POST提交方式，默认为GET提交方式。

2,get可以保存在浏览器书签，可以缓存，post不可以

3，表现形式不同 ：

GET请求，请求的数据会附加在URL之后，以?分割URL和传输数据，多个参数用&连接。URL的编码格式采用的是ASCII编码，而不是uniclde，即是说所有的非ASCII字符都要编码之后再传输。

POST请求：POST请求会把请求的数据放置在HTTP请求包的包体中。

4，传输数据的大小

在HTTP规范中，没有对URL的长度和传输的数据大小进行限制。但是在实际开发过程中，对于GET，特定的浏览器和服务器对URL的长度有限制。因此，在使用GET请求时，传输数据会受到URL长度的限制。

对于POST，由于不是URL传值，理论上是不会受限制的，但是实际上各个服务器会规定对POST提交数据大小进行限制，Apache、IIS都有各自的配置


​	5，安全性

POST的安全性比GET的高。

关于参数

考虑参数的位置：get请求的参数位于url中，而post请求的参数位于request body中。

因此，,在浏览器输入一个网址访问网站都是GET请求，

所以这导致了三个问题，
一是get请求的安全性不如post请求；
二是get的参数有长度限制，而post没有；
三是因为RL的编码格式采用的是ASCII编码，get的参数只允许ASCII字符，post没有限制。

关于回退
点击回退或刷新时，post请求会再次提交表单，而get请求不会。
所以post是回退有害的，get回退无害。

关于缓存
get能被缓存，可以收藏为书签，参数保留在浏览器历史中；
post不能被缓存，不可收藏为书签，参数不会保留在浏览器历史中。

关于请求包
get请求只发送一个tcp数据包，即http header和data共同发送给web服务器，服务器响应200 OK.
post请求发送两个tcp数据包，第一次发送http header，如果web服务器予以响应100 continue，则发送第二个数据包data，服务器响应200 OK.,

##  新架构：Jetpack

Jetpack组件主要分为四大类：基础组件（Foundation Components）、架构组件（Architecture Components）、行为组件（Behavior Components）和界面组件（UI Components

组件（UI Components）。具体包括的库分别如下：

- · 基础组件：包括AppCompat、Android KTX、Multidex和Test；
- · 架构组件：包括Data Binding、Lifecycles、LiveData、Navigation、Paging、Room、ViewModel和WorkManager；·
-  行为组件：包括Download Manager、Media&Playback、Permissions、Notifications、Sharing和Slices；· 
- 界面组件：包括Animation&Transitions、Auto, TV&Wear、Emoji、Fragment、Layout和Palette。