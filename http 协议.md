# HTTP

## [HTTP 协议](http://www.cnblogs.com/li0803/archive/2008/11/03/1324746.html)
HTTP是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。它于1990年提出，经过几年的使用与发展，得到不断地完善和扩展。目前在WWW中使用的是HTTP/1.0的第六版，HTTP/1.1的规范化工作正在进行之中，而且HTTP-NG(Next Generation of HTTP)的建议已经提出。

#### HTTP 请求组成
 http 请求由四部分组成，分别是：请求行、消息报头、空行、请求正文
 
      GET /562f25980001b1b106000338.jpg HTTP/1.1
      Host    img.mukewang.com
      User-Agent    Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
      Accept    image/webp,image/*,*/*;q=0.8
      Referer    http://www.imooc.com/
      Accept-Encoding    gzip, deflate, sdch
      Accept-Language    zh-CN,zh;q=0.8
      Connection: Keep-Alive

请求行以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本，格式如下：

      Method Request-URI HTTP-Version CRLF  

其中 Method表示请求方法；Request-URI是一个统一资源标识符；HTTP-Version表示请求的HTTP协议版本；CRLF表示回车和换行（除了作为结尾的CRLF外，不允许出现单独的CR或LF字符）。

请求方法（所有方法全为大写）有多种，各个方法的解释如下：
* GET     请求获取Request-URI所标识的资源
* POST    在Request-URI所标识的资源后附加新的数据
* HEAD    请求获取由Request-URI所标识的资源的响应消息报头
* PUT     请求服务器存储一个资源，并用Request-URI作为其标识
* DELETE  请求服务器删除Request-URI所标识的资源
* TRACE   请求服务器回送收到的请求信息，主要用于测试或诊断
* CONNECT 保留将来使用
* OPTIONS 请求查询服务器的性能，或者查询与资源相关的选项和需求

> 一个完整的 [URL](http://www.cnblogs.com/woodyblog/p/6005414.html) 包括以下几部分：   
> 
>   protocal://hostname[:port]/path/[;parameters][?query]#fragment
> 
> * protocol 协议，常用的协议是http
> * hostname 主机地址，可以是域名，也可以是IP地址
> * port 端口 http协议默认端口是：80端口，如果不写默认就是:80端口
> * path 路径 网络资源在服务器中的指定路径
> * parameter 参数 如果要向服务器传入参数，在这部分输入
> * query 查询字符串 如果需要从服务器那里查询内容，在这里编辑
> * fragment 片段 网页中可能会分为不同的片段，如果想访问网页后直接到达指定位置，可以在这部分设置
> 
> 例如：
> 
>       http://www.exapmle.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name

#### HTTP 响应
HTTP响应是由四个部分组成，分别是：状态行、消息报头、空行、响应正文

      HTTP/1.1 200 OK  
      Server: Microsoft-IIS/5.0   
      Date: Thu,08 Mar 200707:17:51 GMT
      Connection: Keep-Alive                                 
      Content-Length: 23330
      Content-Type: text/html
      Expries: Thu,08 Mar 2007 07:16:51 GMT
      Set-Cookie: ASPSESSIONIDQAQBQQQB=BEJCDGKADEDJKLKKAJEOIMMH; path=/
      Cache-control: private

      <html>
            <head></head>
            <body>
                  <!--body goes here-->
            </body>
      </html>

状态行由HTTP协议版本号、状态码、状态消息三部分组成。
状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:
* 1xx：指示信息，表示请求已接收，继续处理
* 2xx：成功，表示请求已被成功接收、理解、接受
* 3xx：重定向，要完成请求必须进行更进一步的操作
* 4xx：客户端错误，请求有语法错误或请求无法实现
* 5xx：服务器端错误，服务器未能实现合法的请求

常见状态代码：
* 200 OK      //客户端请求成功
* 400 Bad Request  //客户端请求有语法错误，不能被服务器所理解
* 401 Unauthorized //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
* 403 Forbidden  //服务器收到请求，但是拒绝提供服务
* 404 Not Found  //请求资源不存在，eg：输入了错误的URL
* 500 Internal Server Error //服务器发生不可预期的错误
* 503 Server Unavailable  //服务器当前不能处理客户端的请求，一段时间后可能恢复正常

## 消息报头
#### Access-Control-Allow-Origin
> Access-Control-Allow-Origin是从Cross Origin Resource Sharing (CORS)中分离出来的。这个header是决定哪些网站可以访问资源，通过定义一个通配符来决定是单一的网站还是所有网站可以访问我们的资源。需要注意的是，如果定义了通配符，那么 Access-Control-Allow-Credentials选项就无效了，而且user-agent的cookies不会在请求里发送。

正确的设置

    * – 通配符允许任何远程资源来访问含有Access-Control-Allow-Origin 的内容。  
    http://www.example.com – 只允许特定站点才能访问(http://[host], 或者 https://[host]) 

通常不正确的设置

    http://example.com, http://web2.example.com – 多个站点是不支持的，只能配置一个站点。  
    *.example.com – 只允许单一的站点  
    http://*.example.com – 同上面一样  


#### Etag
> HTTP 协议规格说明定义ETag为“被请求变量的实体值” （参见 —— 章节 14.19）。 另一种说法是，ETag是一个可以与Web资源关联的记号（token）。典型的Web资源可以一个Web页，但也可能是JSON或XML文档。服务器单独负责判断记号是什么及其含义，并在HTTP响应头中将其传送到客户端，以下是服务器端返回的格式：

        ETag: "50b1c1d4f775c61:df3"  
        
客户端的查询更新格式是这样的：  

      If-None-Match: W/"50b1c1d4f775c61:df3"  
 
 如果ETag没改变，则返回状态304然后不返回，这也和Last-Modified一样。本人测试Etag主要在断点下载时比较有用。 
 
      Last-Modified和Etags如何帮助提高性能?  
        
> 聪明的开发者会把Last-Modified 和ETags请求的http报头一起使用，这样可利用客户端（例如浏览器）的缓存。因为服务器首先产生 Last-Modified/Etag标记，服务器可在稍后使用它来判断页面是否已经被修改。本质上，客户端通过将该记号传回服务器要求服务器验证其（客户端）缓存。    

过程如下: 

       1. 客户端请求一个页面（A）。  
       2. 服务器返回页面A，并在给A加上一个Last-Modified/ETag。  
       3. 客户端展现该页面，并将页面连同Last-Modified/ETag一起缓存。  
       4. 客户再次请求页面A，并将上次请求时服务器返回的Last-Modified/ETag一起传递给服务器。  
       5. 服务器检查该Last-Modified或ETag，并判断出该页面自上次客户端请求之后还未被修改，直接返回响应304和一个空的响应体。

**Request header field Access-Control-Allow-Headers is not allowed by Access-Control-Allow-Headers** 

The server (that the POST request is sent to) needs to include the Access-Control-Allow-Headers header (etc) in its response. Putting them in your request from the client has no effect.This is because it is up to the server to specify that it accepts cross-origin requests (and that it permits the Content-Type request header, and so on), the client cannot decide for itself that a given server should allow CORS.

跨域请求在使用POST方法向服务器发送数据时，如果Content-Type使用application/x-www-form-urlencoded、multipart/form-data或text/plain之外编码格式会由普通请求变为Preflighted请求。另外使用自定头时也会变为Preflighted请求。Preflighted 请求与简单请求不同，Preflighted 请求首先会向服务器发送一个Options请求，以验证是否对指定服务有访问权限，之后再发送实际的请求。所以你检查一下你JAVA后端是怎么处理那个先发送的Options请求的，后端服务器需要设置正确的响应头，现在你浏览器报的是那个Preflighted请求不通过的错误。

 在使用Ajax跨域请求时，如果设置Header的ContentType为application/json,会分两次发送请求。第一次先发送Method为OPTIONS的请求到服务器，这个请求会询问服务器支持哪些请求方法（GET,POST等），支持哪些请求头等等服务器的支持情况。等到这个请求返回后，如果原来我们准备发送的请求符合服务器的规则，那么才会继续发送第二个请求，否则会在Console中报错。

 https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
 
 
 ## URL中特殊字符(+?%#&=/)的处理
* `“+”` url中+号的表示空格 转化 %2B
* `“/”` 用来分割目录和子目录 转化 %2F
* `“?”` 用来分割请求的url和参数 转化 %3F
* `“%”` 用来指定特殊字符 转化 %25
* `“#”` 用来表示书签 转化 %23
* `“&”` 用来表示url中指定的参数间的分割符 转化%26
* `“=”` 用来表示url中指定的参数的值 转化 %3D
* `“”` url中的空格可以用+号或编码 转化%20

 
 
