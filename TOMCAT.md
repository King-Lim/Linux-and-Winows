# TOMCAT

## 1 WEB技术

### 1.1 HTTP协议和B/S 结构

操作系统有进程子系统，使用多进程就可以充分利用硬件资源。进程中可以多个线程，每一个线程可以被CPU调度执行，这样就可以让程序并行的执行。这样一台主机就可以作为一个服务器为多个客户端提供计算服务。

客户端和服务端往往处在不同的物理主机上，它们分属不同的进程，这些进程间需要通信。跨主机的进程间通信需要使用网络编程。最常见的网络编程接口是Socket。

Socket称为套接字，本意是插座。也就是说网络通讯需要两端，如果一端被动的接收另一端请求并提供计算和数据的称为服务器端，另一端往往只是发起计算或数据请求，称为客户端。

这种编程模式称为Client/Server编程模式，简称C/S编程。开发的程序也称为C/S程序。C/S编程往往使用传输层协议（TCP/UDP），较为底层，比如：QQ，迅雷, 云音乐, 云盘, foxmail，xshell等

1990年HTTP协议和浏览器诞生。在应用层使用文本跨网络在不同进程间传输数据，最后在浏览器中将 服务器端返回的HTML渲染出来。由此，诞生了网页开发。

网页是存储在WEB服务器端的文本文件，浏览器发起HTTP请求后，到达WEB服务程序后，服务程序根 据URL读取对应的HTML文件，并封装成HTTP响应报文返回给浏览器端。 

起初网页开发主要指的是HTML、CSS等文件制作，目的就是显示文字或图片，通过超级链接跳转到另一 个HTML并显示其内容。 

后来，网景公司意识到让网页动起来很重要，傍着SUN的Java的名气发布了JavaScript语言，可以在浏览器中使用JS引擎执行的脚本语言，可以让网页元素动态变化，网页动起来了。 

为了让网页动起来，微软使用ActiveX技术、SUN的Applet都可以在浏览器中执行代码，但都有安全性问题。能不能直接把内容直接在WEB服务器端组织成HTML，然后把HTML返回给浏览器渲染呢？ 

最早出现了CGI（Common Gateway Interface）通用网关接口，通过浏览器中输入URL直接映射到一个 服务器端的脚本程序执行，这个脚本可以查询数据库并返回结果给浏览器端。这种将用户请求使用程序 动态生成的技术，称为动态网页技术。先后出现了ASP、PHP、JSP等技术，这些技术的使用不同语言编 写的程序都运行在服务器端，所以称为WEB后端编程。有一部分程序员还是要编写HTML、CSS、 JavaScript，这些代码运行在浏览器端，称为WEB前端编程。合起来称为Browser/Server编程，即B/S编程。

### 1.2 前端三大核心技术

#### 1.2.1 HTML

HTML（HyperText Markup Language）超文本标记语言，它不同于一般的编程语言。超文本即超出纯 文本的范畴，例如：描述文本颜色、大小、字体等信息，或使用图片、音频、视频等非文本内容。

HTML由一个个的标签（标记）组成，这些标签各司其职，有的提供网页信息，有的负责文字，有的负 责图片，有的负责网页布局，所以一个HTML文件，是由格式标签和数据组成。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<h1>马哥教育欢迎您</h1>
</body>
</html>
```

超文本需要显示，就得有软件能够呈现超文本定义的排版格式，例如显示：图片、表格，显示字体的大 小、颜色，这个软件就是浏览器。

超文本的诞生是为了解决纯文本不能格式显示的问题，是为了好看，但是只有通过网络才能分享超文本 的内容，所以制定了HTTP协议。

#### 1.2.2 CSS（Cascading Style Sheets）层叠样式表

HTML本身为了格式化显示文本，但是当网页呈现大家面前的时候，需求HTML提供更多样式能力。这使 得HTML变得越来越臃肿。这促使了CSS的诞生。

1994年，W3C成立，CSS设计小组所有成员加入W3C，并努力研发CSS的标准，微软最终加入。 

1996年12月发布CSS 1.0。 

1998年5月发布CSS 2.0。 

CSS 3采用了模块化思想，每个模块都在CSS 2基础上分别增强功能。所以，这些模块是陆续发布的。

不同厂家的浏览器使用的引擎，对CSS的支持不一样，导致网页布局、样式在不同浏览器不一样。因 此，想要保证不同用户使用不同浏览器看到的网页效果一直非常困难。

#### 1.2.3 JavaScript

Javascript 简称JS，是一种动态的弱类型脚本解释性语言，和HTML、CSS并称三大WEB核心技术，得到 了几乎主流浏览器支持。

1994年，网景Netscape公司成立并发布了Netscape Navigator浏览器，占据了很大的市场份额，网景 意识到WEB需要动态，需要一种技术来实现。

1995年9月网景浏览器2发布测试版本发布了LiveScript，随即在12月的测试版就更名为JavaScript。同 时期，微软推出IE并支持JScript、VBScript，与之抗衡。

1997年，网景、微软、SUN、Borland公司和其他组织在ECMA（European Computer Manufacturers  Association 欧洲计算机制造商协会）确定了ECMAScript的本程序设计语言的标准。JavaScript和JScript 都成为ECMAScript标准的实现。

2008年后随着chrome浏览器的V8引擎发布。

V8 JS引擎不是解释执行，而是本地编译，在V8引擎做了很多优化，JS程序在其上运行堪比本地二进制程 序。V8引擎使用C++开发，可以嵌入到任何C++程序中。基于V8引擎，2009年基于服务器javascript的运 行环境Node.js诞生，创建了第一版npm (Node.js包管理器和开源库生态系统), 提供了大量的库供程序员 使用。从此，便可以在服务器端真正大规模使用JavaScript编程了。也就是说 JavaScript 也可以真正称为 服务器端编程语言了，成为目前唯一的前，后端通用的语言。

**同步**

交互式网页，用户提交了请求，就是想看到查询的结果。服务器响应到来后是一个全新的页面内容，哪 怕URL不变，整个网页都需要重新渲染。例如，用户填写注册信息，只是2次密码不一致，提交后，整个 注册页面重新刷新，所有填写项目重新填写(当然有办法让用户减少重填)。这种交互非常不友好。从代价 的角度看，就是为了注册的一点点信息，结果返回了整个网页内容，不但浪费了网络带宽，还需要浏览 器重新渲染网页，太浪费资源了，影响了用户体验和感受。上面这些请求的过程，就是同步过程，用户 发起请求，页面整个刷新，直到服务器端响应的数据到来并重新渲染。

**异步**

1996年微软实现了iframe标签，可以在一个网页使用iframe标签局部异步加载内容。

1999年微软推出异步数据传输的ActiveX插件技术，太笨重了，但是也火了很多年。有一个组件 XMLHttpRequest被大多数浏览器支持。

传统的网页如果需要更新内容，必需重载整个网页面。Ajax的出现，改变这一切，同时极大的促进了 Javascript的发展。Ajax 即"Asynchronous Javascript And XML"（异步 JavaScript 和 XML），是指一种 创建交互式、快速动态网页应用的网页开发技术，最早起源于1998年微软的Outlook Web Access开发 团队。Ajax 通过在后台与服务器进行少量数据交换， 可以使网页实现异步更新。这意味着可以在不重新 加载整个网页的情况下，对网页的某部分进行更新。Javascript 通过调用浏览器内置的WEB API中的 XMLHttpRequest 对象实现Ajax 技术。早期Ajax结合数据格式XML，目前更多的使用JSON。利用AJAX 可实现前后端开发的彻底分离，改变了传统的开发模式。

AJAX是一种技术的组合，技术的重新发现，而不是发明，但是它深远的影响了整个WEB开发。 

参考资料： https://www.w3school.com.cn/ajax/index.asp

## 2 java 基础

### 2.1 WEB架构

#### 2.1.1 web资源和访问

**PC 端或移动端浏览器访问**

从静态服务器请求HTML、CSS、JS等文件发送到浏览器端，浏览器端接收后渲染在浏览器上

从图片服务器请求图片资源显示

从业务服务器访问动态内容，动态内容是请求后有后台服务访问数据库后得到的，最终返回到浏览器端

**手机 App 访问**

内置了HTML和JS文件，不需要从静态WEB服务器下载 JS 或 HTML。为的就是减少文件的发送，现代前 端开发使用的JS文件太多或太大了 有必要就从图片服务器请求图片，从业务服务器请求动态数据

客户需求多样，更多的内容还是需要由业务服务器提供，业务服务器往往都是由一组服务器组成。

#### 2.1.2 后台应用架构

##### 2.1.2.1 单体架构

* 传统架构（单机系统），一个项目一个工程：比如商品、订单、支付、库存、登录、注册等等，统 一部署，一个进程 
* all in one的架构方式，把所有的功能单元放在一个应用里。然后把整个应用部署到一台服务器上。 如果负载能力不行，将整个应用进行水平复制，进行扩展，然后通过负载均衡实现访问。 
* Java实现：JSP、Servlet，打包成一个jar、war部署
* 易于开发和测试:也十分方便部署;当需要扩展时，只需要将war复制多份，然后放到多个服务器 上，再做个负载均衡就可以了。 
* 如果某个功能模块出问题，有可能全站不可访问，修改Bug后、某模块功能修改或升级后，需要停掉整个服务，重新整体重新打包、部署这个应用war包，功能模块相互之间耦合度高,相互影响,不适 合当今互联网业务功能的快速迭代。 
* 特别是对于一个大型应用，我们不可能吧所有内容都放在一个应用里面，我们如何维护、如何分工 合作都是问题。如果项目庞大，管理难度大 
* web应用服务器：开源的tomcat、jetty、glassfish。商用的有weblogic、websphere、Jboss

##### 2.1.2.2 微服务

```http
https://www.martinfowler.com/microservices/
```

* 属于SOA（Service Oriented Architecture）的子集 
* 微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底去掉耦合，每一 个微服务提供单个业务功能，一个服务只做一件事。每个服务都围绕着具体业务进行构建，并且能够被独立地部署到生产环境、类生产环境等 
* 从技术角度讲就是一种小而独立的处理过程，类似与进程的概念，能够自行单独启动或销毁 
* 微服务架构（分布式系统），各个模块/服务，各自独立出来，"让专业的人干专业的事"，独立部 署。分布式系统中，不同的服务可以使用各自独立的数据库。 
* 服务之间采用轻量级的通信机制（通常是基于HTTP的RESTful API）。 
* 微服务设计的思想改变了原有的企业研发团队组织架构。传统的研发组织架构是水平架构，前端、 后端、DBA、测试分别有自己对应的团队，属于水平团队组织架构。而微服务的设计思想对团队的 划分有着一定的影响，使得团队组织架构的划分更倾向于垂直架构，比如用户业务是一个团队来负 责，支付业务是一个团队来负责。但实际上在企业中并不会把团队组织架构拆分得这么绝对，垂直架构只是一种理想的架构 
* 微服务的实现框架有多种，不同的应用架构，部署方式也有不同

##### 2.1.2.3 单体架构和微服务比较

##### 2.1.2.4 微服务的优缺点

**微服务优点：**

* 每个服务足够内聚，足够小，代码容易理解。这样能聚焦一个只当的业务功能或业务需求。 
* 开发简单、开发效率提高，一个服务可能就是专业的只干一件事，微服务能够被小团队单独开发， 这个小团队可以是2到5人的开发人员组成 
* 微服务是松耦合的，是有功能意义的服务，无论是在开发阶段或部署阶段都是独立的。 
* 微服务能使用不同的语言开发 
* 易于和第三方集成，微服务运行容易且灵活的方式集成自动部署，通过持续集成工具，如:  Jenkins、Hudson、Bamboo 
* 微服务易于被一个开发人员理解、修改和维护，这样小团队能够更关注自己的工作成果，无需通过合作才能体现价值
* 微服务允许你利用融合最新技术。微服务只是业务逻辑的代码，不会和HTML/CSS或其他界面组件 混合，即前后端分离 
* 每个微服务都有自己的存储能力，可以有自己的数据库，也可以有统一数据库

**微服务缺点：**

* 微服务把原有的一个项目拆分成多个独立工程，增加了开发、测试、运维、监控等的复杂度 
* 微服务架构需要保证不同服务之间的数据一致性，引入了分布式事务和异步补偿机制，为设计和开 发带来一定挑战 
* 开发人员和运维需要处理分布式系统的复杂性，需要更强的技术能力 
* 微服务适用于复杂的大系统，对于小型应用使用微服务，进行盲目的拆分只会增加其维护和开发成本

##### 2.1.2.5 常见的微服务框架

* Dubbo 
  * 阿里开源贡献给了ASF，目前已经是Apache的顶级项目 
  * 一款高性能的Java RPC服务框架，微服务生态体系中的一个重要组件 
  * 将单体程序分解成多个功能服务模块，模块间使用Dubbo框架提供的高性能RPC通信 
  * 内部协调使用Zookeeper，实现服务注册、服务发现和服务治理

* Spring cloud  
  * 一个完整的微服务解决方案，相当于Dubbo的超集 
  * 微服务框架，将单体应用拆分为粒度更小的单一功能服务 
  * 基于HTTP协议的REST(Representational State Transfer 表述性状态转移）风格实现模块间通信

### 2.2 Java

#### 2.2.1 Java历史

Java原指的是印度尼西亚的爪哇岛，人口众多，盛产咖啡、橡胶等。

Java语言最早是在1991年开始设计的，最初叫Oak项目，它初衷是跑在不同机顶盒设备中的。

1993年网景公司成立。Oak项目组很快他们发现了浏览器和动态网页技术这个巨大的市场，转向WEB方 向。并首先发布了可以让网页动起来的Applet技术（浏览器中嵌入运行Java字节码的技术）。

在1995年，一杯爪哇岛咖啡成就了Java这个名字。

Sun公司第一个Java公开版本1.0发布于1996年。口号是"一次编写，到处运行"(Write once，Run  anywhere)，跨平台运行。

1999年，SUN公司发布了第二代Java平台(Java2)。

2009年4月20日，Oracle甲骨文公司宣布将以每股9.50美元，总计74亿美金收购SUN（计算机系统）公 司。2010年1月成功收购。

2010年，Java创始人之一的 James Gosling 离开了Oracle，去了Google。

#### 2.2.2 java 组成

Java 包含下面部分： 

* 语言、语法规范。关键字,如: if、for、class等 
* 源代码 source code 
* 依赖库，标准库(基础)、第三方库(针对某些应用)。底层代码太难使用且开发效率低，封装成现成的库 
* JVM虚拟机。将源代码编译为中间码即字节码后,再运行在JVM之上

由于各种操作系统ABI不一样，采用编译方式，需要为不同操作系统编译成相应格式的二进制程序才能运 行。 

1995年，Java发布Applet技术，Java程序在后台编译成字节码，发送到浏览器端，在浏览器中运行一个 Applet程序，这段程序是运行在另外一个JVM进程中的。 

但是这种在客户端运行Java代码的技术，会有很大的安全问题。1997年CGI技术发展起来，动态网页技 术开始向后端开发转移，在后端将动态内容组织好，拼成HTML发回到浏览器端。

#### 2.2.3 Java动态网页技术

##### 2.2.3.1 servlet

本质就是一段Java程序

```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
public class HelloWorld extends HttpServlet {
	private String message;
	public void init() throws ServletException
 	{
      	message = "Hello World";
 	}
  	public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
 	{
      	response.setContentType("text/html"); //响应报文内容类型
      	PrintWriter out = response.getWriter();  //构建响应报文内容
      	out.println("<h1>" + message + "</h1>");
      	out.println("<p><a href=http://www.magedu.com>马哥教育</a>欢迎你</p>");
 	}
  
  	public void destroy()
 	{
 	}
}
```

在Servlet中最大的问题是，HTML输出和Java代码混在一起，如果网页布局要调整，Java源代码就需要随之进行调整,对于开发人员来说就是个噩梦。

##### 2.2.3.2 jsp（Java Server Pages）

JSP本质是提供一个HTML模板，也就是在网页中预留以后填充的空，后续将Java程序运行生成的数据对HTML进行填空就可以了。如果网页布局需要调整,JAVA源代码不需要很大的调整

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>jsp例子</title>
</head>
<body>
本行后面的内容是服务器端动态生成字符串，最后拼接在一起
<%
out.println("你的 IP 地址 " + request.getRemoteAddr());
%>
</body>
</html>
```

JSP是基于Servlet实现，JSP将表现和逻辑分离，这样页面开发人员更好的注重页面表现力更好服务客户。

不过最终 JSP 还需要先转换为 Servlet的源代码.java文件（Tomcat中使用Jasper转换），只不过这个转换过程无需人工完成,是通过工具自动实现的,然后再编译成.class文件，最后才可以在JVM中运行。

比如: 浏览器第一次请求test.jsp时, Tomcat服务器会自动将test.jsp转化成test.jsp.java这么一个类,并将 该文件编译成class文件。编译完毕后再运行class文件来响应浏览器的请求。如果以后访问test.jsp就不 再重新编译jsp文件了，直接调用class文件来响应浏览器。后续如果Tomcat检测到JSP页面改动了的话， 会重新编译

JSP类似于PHP和ASP,前端代码和后端JAVA代码混写在一起,需要前端和后端工程师在一起协作才能完成, 无法做到真正的前后端分离开发

在web早期的开发中，通常采用的分为两层，视图层和模型层。

优点：架构简单，比较适合小型项目开发 

缺点：JSP职责不单一，职责过重，不便于维护

##### 2.2.3.3 MVC

如果过度使用jsp技术,jsp中既写有大量的java代码，也有html，甚至还有javascript等，造成难以维护， 难以实现前后端分工协作，后来java 的web开发借鉴了MVC（Model View Controller ）开发模式

MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。是将业务逻辑、 数据、显示分离的方法来组织代码。MVC主要作用是降低了视图与业务逻辑间的双向偶合。

MVC不是一种设计模式，MVC是一种架构模式。当然不同的MVC存在差异。

* Model（模型）：数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或 JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服 务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和 业务。 
* View（视图）：负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。可通过 JSP实现 
* Controller（控制器）：接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的 模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。最终表现为Servlet

最典型的MVC就是JSP + servlet + javabean的模式。

**职责分析：** 

Controller：控制器 

* 取得表单数据 
* 调用业务逻辑 
* 转向指定的页面

Model：模型 

* 业务逻辑 
* 保存数据的状态

View：视图 

* 显示页面

**处理流程** 

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法 
3. 业务处理完毕，返回更新后的数据给servlet 
4. servlet转向到JSP，由JSP来渲染页面 
5. 响应给前端更新后的页面

MVC模式也有以下不足： 

* 每次请求必须经过"控制器->模型->视图"这个流程，用户才能看到最终的展现界面，这个过程似乎有些复杂 
* 实际上视图是依赖于模型的，换句话说，如果没有模型，视图也无法呈现出最终的效果 
* 渲染视图过程是在服务端来完成的，最终呈现给浏览器的是带有模型的视图页面，性能无法得到很好的优化

##### 2.2.3.4 REST

为了使数据展现过程更加直接，并且提供更好的用户体验，对MVC模式进行改进。首先从浏览器发送 AJAX（ Asynchronous JavaScript and XML 异步的 JavaScript 和 XML））请求，然后服务端接受该请求并返回JSON数据返回给浏览器，最后在浏览器中进行界面渲染。改进后的MVC模式如下图所示：

也就是说，我们输入的是AJAX请求，输出的是JSON数据，REST技术实现这样的功能。

REST全称是Representational State Transfer（表述性状态转移），全称是 Resource  Representational State Transfer：通俗来讲就是：资源在网络中以某种表现形式进行状态转移。分解 开来：Resource：资源，即数据；Representational：某种表现形式，比如用JSON，XML，JPEG等； State Transfer：状态变化。通过HTTP动词实现

它是Roy Fielding博士在2000年写的一篇关于软件架构风格的论文，后来国内外许多知名互联网公司纷纷开始采用这种轻量级的Web服务，大家习惯将其称为RESTful Web Services，或简称REST服务。

如果将浏览器这一端视为前端，而服务器那一端视为后端的话，可以将以上改进后的MVC模式简化为以下前后端分离模式，如下图所示：

可见，采用REST风格的架构可以使得前端关注界面展现，后端关注业务逻辑，分工明确，职责清晰。

在设计web接口的时候，REST主要是用于定义接口名，接口名一般是用名次写，不用动词，那怎么表达 “获取”或者“删除”或者“更新”这样的操作呢——用请求类型来区分。

比如，我们有一个friends接口，对于“朋友”我们有增删改查四种操作，怎么定义REST接口？

增加一个朋友，uri: generalcode.cn/v1/friends 接口类型：POST 

删除一个朋友，uri: generalcode.cn/va/friends 接口类型：DELETE 

修改一个朋友，uri: generalcode.cn/va/friends 接口类型：PUT 

查找朋友，uri: generalcode.cn/va/friends 接口类型：GET

上面我们定义的四个接口就是符合REST协议的，请注意，这几个接口都没有动词，只有名词friends，都是通过Http请求的接口类型来判断是什么业务操作。

REST就是一种设计API的模式。最常用的数据格式是JSON。由于JSON能直接被JavaScript读取，所以， 以JSON格式编写的REST风格的API具有简单、易读、易用的特点。

编写API有什么好处呢？由于API就是把Web App的功能全部封装了，所以，通过API操作数据，可以极 大地把前端和后端的代码隔离，使得后端代码易于测试，前端代码编写更简单。离。前端拿到数据只负 责展示和渲染，不对数据做任何处理。后端处理数据并以JSON格式传输出去，定义这样一套统一的接 口，在web，ios，android三端都可以用相同的接口，通过客户端访问API，就可以完成通过浏览器页面 提供的功能，而后端代码基本无需改动。

#### 2.2.4 JDK

##### 2.2.4.1 JDK和JRE

###### 2.2.4.1.1 JDK 和 JRE 关系

**Java SE API**: Java 基础类库开发接口 

**JRE**：Java Runtime Environment缩写，指Java运行时环境， 包含 JVM + Java核心类库 

**JDK**：Java Development Kit，即 Java 语言的软件开发工具包,JDK协议基于 JRL(JavaResearch License) 协议

###### 2.2.4.1.2 JVM 的各种版本

参考链接: 

https://en.wikipedia.org/wiki/List_of_Java_virtual_machines 

https://en.wikipedia.org/wiki/Comparison_of_Java_virtual_machines 

各个公司和组织基于标准规范,开发了不同的JVM版本 

* SUN HotSpot 
* IBM J9VM 
* BEA JRockit 

##### 2.2.4.2 Oracle JDK版本

JDK也就是常说的J2SE，在1999年，正式发布了Java第二代平台，发布了三个版本：

* J2SE：标准版，适用于桌面平台 
* J2EE：企业版，java在企业级开发所有规范的总和，共有13个大的规范,Servlet、Jsp都包含在 JavaEE规范中 
* J2ME：微型版，适用于移动、无线、机顶盒等设备环境

2005年，Java的版本又更名为JavaSE、JavaEE、JavaME

JDK7、JDK8、JDK11是LTS（Long Term Support）

JDK 历史版本 https://en.wikipedia.org/wiki/Java_version_history

JDK 版本使用情况 

数据来源：https://www.baeldung.com/java-in-2019

**收费** 

从2019年1月份开始，Oracle JDK 开始对 Java SE 8 之后的版本开始进行商用收费，确切的说是 8u201/202 之后的版本。如果你用 Java 开发的功能如果是用作商业用途的，如果还不想花钱购买的 话，能免费使用的最新版本是 8u201/202。当然如果是个人客户端或者个人开发者可以免费试用 Oracle  JDK 所有的版本。

**发版方式** 

在 JDK 9 发布之前，Oracle 的发版策略是以特性驱动的，只有重大的特性改变才会发布大版本，比如 JDK 7 到 JDK 8，中间会发多个更新版本。而从 JDK 9 开始变为以时间驱动的方式。发布周期为6个月一 个大版本，比如 JDK 9 到 JDK 10，3个月一次补丁版，3年一个 LTS(长期支持版本)。

##### 2.2.4.3 OpenJDK 

###### 2.2.4.3.1 OpenJDK 介绍

OpenJDK是Sun公司采用GPL v2协议发布的JDK开源版本，于2009年正式发布。

官方网站：https://openjdk.java.net/projects/jdk6/

OpenJDK 7是基于JDK7的beta版开发，但为了也将Java SE 6开源，从OpenJDK7的b20构建反向分支开 发，从中剥离了不符合Java SE 6规范的代码，发布OpenJDK 6。所以OpenJDK6和JDK6没什么关系,只是 API兼容而已

OpenJDK使用GPL v2可以用于商业用途。目前由红帽维护。OpenJDK也有在其基础上的众多发行版，比如阿里的Dragonwell。

OpenJDK使用GPL v2可以用于商业用途。目前由红帽维护。OpenJDK也有在其基础上的众多发行版，比 如阿里的Dragonwell。

###### 2.2.4.3.2 安装 openjdk

* 在CentOS中，可以使用 yum 仓库安装 openjdk

```bash
[root@centos8 ~]#dnf list "*jdk*"
[root@centos8 ~]#dnf -y install java-1.8.0-openjdk.x86_64 java-1.8.0-openjdk-devel
[root@centos8 ~]#java -version
openjdk version "1.8.0_232"
OpenJDK Runtime Environment (build 1.8.0_232-b09)
OpenJDK 64-Bit Server VM (build 25.232-b09, mixed mode)

[root@centos8 ~]#which java
/usr/bin/java
[root@centos8 ~]#ll /usr/bin/java
lrwxrwxrwx 1 root root 22 Feb  8 20:03 /usr/bin/java -> /etc/alternatives/java

[root@centos8 ~]#ll /etc/alternatives/java
lrwxrwxrwx 1 root root 73 Feb  8 20:03 /etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.232.b09-0.el8_0.x86_64/jre/bin/java
[root@centos8 ~]#rpm -qf /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.232.b09-0.el8_0.x86_64/jre/bin/java
java-1.8.0-openjdk-headless-1.8.0.232.b09-0.el8_0.x86_64
```

范例: 安装 java-11-openjdk

```bash
OpenJDK 64-Bit Server VM 18.9 (build 11.0.7+10-LTS, mixed mode, sharing)
[root@centos8 ~]#dnf -y install java-11-openjdk.x86_64
[root@centos8 ~]#java --version
openjdk 11.0.7 2020-04-14 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.7+10-LTS)
```

范例: 编译运行java程序

```bash
[root@centos8 ~]#dnf -y install java-1.8.0-openjdk-devel
[root@centos8 ~]#cat Hello.java
class Hello{  
   public static void main(String[] args)
   {  
       System.out.println("hello, world");  
   }  
}  
#编译成字节码
[root@centos8 ~]#javac Hello.java 
[root@centos8 ~]#ll Hello.*
-rw-r--r-- 1 root root 416 Oct 24 13:00 Hello.class
-rw-r--r-- 1 root root 130 Aug 22 23:38 Hello.java
[root@centos8 ~]#file Hello.class 
Hello.class: compiled Java class data, version 52.0 (Java 1.8)
#运行java程序
[root@centos8 ~]#java Hello 
hello, world
```

* ubuntu 安装 openjdk

```bash
[root@ubuntu1804 ~]#apt update
[root@ubuntu1804 ~]#apt -y install openjdk-8-jdk 
[root@ubuntu1804 ~]#java -version
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (build 1.8.0_242-8u242-b08-0ubuntu3~18.04-b08)
OpenJDK 64-Bit Server VM (build 25.242-b08, mixed mode)

root@ubuntu2004:~# apt -y install openjdk-11-jdk
root@ubuntu2004:~# java -version
openjdk version "11.0.9.1" 2020-11-04
OpenJDK Runtime Environment (build 11.0.9.1+1-Ubuntu-0ubuntu1.20.04)
OpenJDK 64-Bit Server VM (build 11.0.9.1+1-Ubuntu-0ubuntu1.20.04, mixed mode, sharing)
```

##### 2.2.4.4 安装oracle官方 JDK

官方下载链接: 

```bash
#注意需要注册登录后,才能下载JDK
https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
```

###### 2.2.4.4.1 Oracle JDK 的 rpm安装

```bash
#需要登录下载：https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
[root@centos8 ~]#ls -lh jdk-8u241-linux-x64.rpm
-rw-r--r-- 1 root root 171M Feb  8 18:29 jdk-8u241-linux-x64.rpm
#安装jdk，无相关依赖包
[root@centos8 ~]#dnf -y install jdk-8u241-linux-x64.rpm
[root@centos8 ~]#java -version
java version "1.8.0_241"
Java(TM) SE Runtime Environment (build 1.8.0_241-b07)
Java HotSpot(TM) 64-Bit Server VM (build 25.241-b07, mixed mode)
#初始化环境变量
[root@centos8 ~]#vim /etc/profile.d/jdk.sh
[root@centos8 ~]#cat /etc/profile.d/jdk.sh
export JAVA_HOME=/usr/java/default
export PATH=$JAVA_HOME/bin:$PATH
#以下两项非必须项
export JRE_HOME=$JAVA_HOME/jre   
export CLASSPATH=$JAVA_HOME/lib/:$JRE_HOME/lib/ 

[root@centos8 ~]#. /etc/profile.d/jdk.sh

#查看jdk信息
[root@centos8 ~]#which java
/usr/bin/java
[root@centos8 ~]#ll /usr/bin/java
lrwxrwxrwx 1 root root 22 Feb  8 18:35 /usr/bin/java -> /etc/alternatives/java
[root@centos8 ~]#ll /etc/alternatives/java
lrwxrwxrwx 1 root root 41 Feb  8 18:35 /etc/alternatives/java -> /usr/java/jdk1.8.0_241-amd64/jre/bin/java

[root@centos8 ~]#rpm -q --scripts jdk-8u241-linux-x64.rpm

#查看到安装目录为/user/java下
[root@centos8 ~]#rpm -ql jdk-8u241-linux-x64.rpm |less
warning: jdk-8u241-linux-x64.rpm: Header V3 RSA/SHA256 Signature, key ID ec551f03: NOKEY
/usr
/usr/java
/usr/java/jdk1.8.0_241-amd64
/usr/java/jdk1.8.0_241-amd64/.java
/usr/java/jdk1.8.0_241-amd64/.java/.systemPrefs
/usr/java/jdk1.8.0_241-amd64/.java/.systemPrefs/.system.lock
/usr/java/jdk1.8.0_241-amd64/.java/.systemPrefs/.systemRootModFile
/usr/java/jdk1.8.0_241-amd64/.java/init.d
......

[root@centos8 ~]#ll /usr/java/
total 0
lrwxrwxrwx 1 root root  16 Feb  8 18:35 default -> /usr/java/latest
drwxr-xr-x 8 root root 258 Feb  8 18:35 jdk1.8.0_241-amd64
lrwxrwxrwx 1 root root  28 Feb  8 18:35 latest -> /usr/java/jdk1.8.0_241-amd64
```

###### 2.2.4.4.2 Oracle JDK的二进制文件安装

```bash
#下载安装包：https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
[root@centos8 ~]#tar xvf jdk-8u241-linux-x64.tar.gz -C /usr/local/
[root@centos8 ~]#cd /usr/local/
[root@centos8 ~]#ln -s jdk1.8.0_241/ jdk
#初始化环境变量
[root@centos8 ~]#vim /etc/profile.d/jdk.sh
[root@centos8 ~]#cat /etc/profile.d/jdk.sh 
export JAVA_HOME=/usr/local/jdk
export PATH=$PATH:$JAVA_HOME/bin
#以下两项非必须项
#export JRE_HOME=$JAVA_HOME/jre 
#export CLASSPATH=$JAVA_HOME/lib/:$JRE_HOME/lib/
[root@centos8 ~]#. /etc/profile.d/jdk.sh

#注意:JAVA_HOME变量必须设置,否则tomcat启动时会出下面错误
[root@centos8 ~]#catalina.sh 
Neither the JAVA_HOME nor the JRE_HOME environment variable is defined At least one of these environment variable is needed to run this program
[root@centos8 ~]#startup.sh 
Neither the JAVA_HOME nor the JRE_HOME environment variable is defined At least one of these environment variable is needed to run this program

#验证安装
[root@centos8 ~]#java -version
java version "1.8.0_241"
Java(TM) SE Runtime Environment (build 1.8.0_241-b07)
Java HotSpot(TM) 64-Bit Server VM (build 25.241-b07, mixed mode)
[root@centos8 ~]#which java
/usr/local/jdk/bin/java
```

###### 2.3.4.4.3 一键安装二进制的JDK

```bash
[root@ubuntu1804 ~]#cat install_jdk.sh
#!/bin/bash
#********************************************************************
DIR=`pwd`
JDK_FILE="jdk-8u281-linux-x64.tar.gz"
JDK_DIR="/usr/local"
color () {
    RES_COL=60
    MOVE_TO_COL="echo -en \\033[${RES_COL}G"
    SETCOLOR_SUCCESS="echo -en \\033[1;32m"
    SETCOLOR_FAILURE="echo -en \\033[1;31m"
    SETCOLOR_WARNING="echo -en \\033[1;33m"
    SETCOLOR_NORMAL="echo -en \E[0m"
    echo -n "$2" && $MOVE_TO_COL
    echo -n "["
    if [ $1 = "success" -o $1 = "0" ] ;then
        ${SETCOLOR_SUCCESS}
        echo -n $" OK "    
    elif [ $1 = "failure" -o $1 = "1" ] ;then
        ${SETCOLOR_FAILURE}
        echo -n $"FAILED"
    else
        ${SETCOLOR_WARNING}
        echo -n $"WARNING"
    fi
    ${SETCOLOR_NORMAL}
    echo -n "]"
    echo                                                                         
                                                     
}
install_jdk(){
if ! [  -f "$DIR/$JDK_FILE" ];then
 color 1  "$JDK_FILE 文件不存在"
 exit; 
elif [ -d $JDK_DIR/jdk ];then
       color 1  "JDK 已经安装"
 exit
else
       [ -d "$JDK_DIR" ] || mkdir -pv $JDK_DIR
fi
tar xvf $DIR/$JDK_FILE  -C $JDK_DIR
cd  $JDK_DIR && ln -s jdk1.8.* jdk 
cat > /etc/profile.d/jdk.sh <<EOF
export JAVA_HOME=$JDK_DIR/jdk
export JRE_HOME=\$JAVA_HOME/jre
export CLASSPATH=\$JAVA_HOME/lib/:\$JRE_HOME/lib/
export PATH=\$PATH:\$JAVA_HOME/bin
EOF
. /etc/profile.d/jdk.sh
java -version && color 0  "JDK 安装完成" || { color 1  "JDK 安装失败" ; exit; }
}

install_jdk
```

## 3 Tomcat 基础功能

### 3.1 tomcat历史和介绍

#### 3.1.1 WEB应用服务器

web 应用服务器的使用 

数据来源：https://www.baeldung.com/java-in-2019

* 商用：IBM WebSphere、Oracle WebLogic（原属于BEA公司）、Oracle Oc4j、RedHat JBoss等 
* 开源：Tomcat、Jetty、Resin、Glassfish

#### 3.1.2 Tomcat 介绍

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服务器，在中小型系统和 并发访问用户不是很多的场合下被普遍使用，Tomcat 具有处理HTML页面的功能，它还是一个Servlet 和JSP容器

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服务器，在中小型系统和 并发访问用户不是很多的场合下被普遍使用，Tomcat 具有处理HTML页面的功能，它还是一个Servlet 和JSP容器

Tomcat 仅仅实现了Java EE规范中与Servlet、JSP相关的类库，是JavaEE不完整实现。

著名图书出版商O'Reilly约稿该项目成员Davidson希望使用一个公猫作为封面，但是公猫已经被使用， 书出版后封面是一只雪豹。

1999年发布初始版本是Tomcat 3.0，实现了Servlet 2.2 和 JSP 1.1规范。 

Tomcat 4.x发布时，内建了Catalina（Servlet容器）和 Jasper（JSP engine）等 

当前 Tomcat 的正式版本已经更新到 9.0.x 版本，但当前企业中主流版本为 8.x 和 7.x  

官网： http://tomcat.apache.org/ 

官网文档: https://tomcat.apache.org/tomcat-8.5-doc/index.html 

帮助文档: 

https://cwiki.apache.org/confluence/display/tomcat/ 

https://cwiki.apache.org/confluence/display/tomcat/FAQ

#### 3.1.3 Tomcat 各版本区别

官方文档：https://tomcat.apache.org/whichversion.html

### 3.2 安装 Tomcat

#### 3.2.1 基于包安装 Tomcat

##### 3.2.1.1 CentOS 包安装

CentOS 8 包仓库中目前还没有提供tomcat相关包

```bash
[root@centos8 ~]#yum list tomcat
Last metadata expiration check: 1:25:35 ago on Wed 15 Jul 2020 09:01:28 AM CST.
Error: No matching Packages to list
```

CentOS 7 yum仓库源中自带的Tomcat 7.0版本安装，此方式安装tomcat版本较低，不推荐

范例：在CentOS 7 上安装 tomcat

```bash
[root@centos7 ~]#yum list tomcat*
[root@centos7 ~]#yum -y install tomcat tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp
[root@centos7 ~]#systemctl enable --now tomcat
Created symlink from /etc/systemd/system/multi-user.target.wants/tomcat.service to /usr/lib/systemd/system/tomcat.service.

[root@centos7 ~]#ss -ntl

[root@centos7 ~]#getent passwd tomcat
tomcat:x:53:53:Apache Tomcat:/usr/share/tomcat:/sbin/nologin
[root@centos7 ~]#ps aux|grep tomcat
tomcat     1328  0.4 11.2 2298188 112004 ?     Ssl  21:32   0:11 
/usr/lib/jvm/jre/bin/java -classpath
/usr/share/tomcat/bin/bootstrap.jar:/usr/share/tomcat/bin/tomcat-juli.jar:/usr/share/java/commons-daemon.jar -Dcatalina.base=/usr/share/tomcat -
Dcatalina.home=/usr/share/tomcat -Djava.endorsed.dirs= -
Djava.io.tmpdir=/var/cachetomcat/temp -
Djava.util.logging.config.file=/usr/share/tomcat/conf/logging.properties -
Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager 
org.apache.catalina.startup.Bootstrap start
root       1521  0.0  0.0 112712   960 pts/0   R+   22:15   0:00 grep --color=auto tomcat
```

打开浏览器访问：http://tomcat:8080/

##### 3.2.1.2 Ubuntu 包安装 tomcat

范例: Ubuntu安装 tomcat

```bash
[root@ubuntu1804 ~]#apt -y install tomcat8 tomcat8-admin tomcat8-docs
```

#### 3.2.2 二进制安装 Tomcat

CentOS 7 的yum源的tomcat版本老旧,而CentOS8 yum源里无tomcat 

目前比较主流的Tomcat是8.5.X版本,推荐从Apache官网下载二进制tomcat包进行安装,此为生产常用方式

##### 3.2.2.1 下载并安装

**注意: 安装tomcat 前必须先部署JDK**

官方和镜像站点下载: 

```http
https://tomcat.apache.org/download-80.cgi
https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/
```

```bash
#官网或镜像网站下载：
[root@centos8 ~]#wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat8/v8.5.50/bin/apache-tomcat-8.5.50.tar.gz
[root@centos8 ~]#tar xf apache-tomcat-8.5.50.tar.gz -C /usr/local/
[root@centos8 ~]#cd /usr/local/
[root@centos8 local]#ln -s apache-tomcat-8.5.50/ tomcat
#指定PATH变量
[root@centos8 ~]#echo 'PATH=/usr/local/tomcat/bin:$PATH' > 
/etc/profile.d/tomcat.sh
[root@centos8 ~]#. /etc/profile.d/tomcat.sh
[root@centos8 ~]#echo $PATH
/usr/local/tomcat/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/local/jdk/bin:/root/bin
#查看当前变量设置和命令用法
[root@centos8 ~]#catalina.sh
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:       /usr/local/jdk
Using CLASSPATH:       
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Usage: catalina.sh ( commands ... )
commands:
 debug             Start Catalina in a debugger
 debug -security   Debug Catalina with a security manager
 jpda start       Start Catalina under JPDA debugger
 run               Start Catalina in the current window
 run -security     Start in the current window with security manager
 start             Start Catalina in a separate window
 start -security   Start in a separate window with security manager
 stop             Stop Catalina, waiting up to 5 seconds for the process to end
 stop n           Stop Catalina, waiting up to n seconds for the process to end
 stop -force       Stop Catalina, wait up to 5 seconds and then use kill -KILL if still running
 stop n -force     Stop Catalina, wait up to n seconds and then use kill -KILL if still running
 configtest       Run a basic syntax check on server.xml - check exit code for result
 version           What version of tomcat are you running?
Note: Waiting for the process to end and use of the -force option require that $CATALINA_PID is defined

#查看环境变量和版本信息
[root@centos8 ~]#catalina.sh version
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:       /usr/local/jdk/jre
Using CLASSPATH:       
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Server version: Apache Tomcat/8.5.59
Server built:   Oct 6 2020 16:57:18 UTC
Server number:  8.5.59.0
OS Name:       Linux
OS Version:     4.18.0-193.el8.x86_64
Architecture:   amd64
JVM Version:    1.8.0_261-b12
JVM Vendor:     Oracle Corporation

#启动tomcat 
[root@centos8 ~]#startup.sh 
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:       /usr/local/jdk/jre
Using CLASSPATH:       
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar Tomcat started.

#查看端口
[root@centos8 ~]#ss -ntl

#查看进程是以root启动的
[root@centos8 ~]#ps aux|grep tomcat
root      12994 34.1  9.4 2155140 76912 pts/0   Sl   22:38   0:02 
/usr/local/jdk/jre/bin/java -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -Dignore.endorsed.dirs= -classpath
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar -Dcatalina.base=/usr/local/tomcat -Dcatalina.home=/usr/local/tomcat -Djava.io.tmpdir=/usr/local/tomcat/temp org.apache.catalina.startup.Bootstrap start
root      13039  0.0  0.1  12108  1076 pts/0   R+   22:38   0:00 grep --color=auto tomcat

#关闭tomcat
[root@centos8 ~]#shutdown.sh
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:       /usr/local/jdk/jre
Using CLASSPATH:       
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar

#或者以下也可以,指定10s后停止,默认5s
[root@centos8 ~]#catalina.sh stop 10

[root@centos8 ~]#ss -ntl

#再次用不同方式启动tomcat 
[root@centos8 ~]#catalina.sh start
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:       /usr/local/jdk/jre
Using CLASSPATH:       
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar Tomcat started.
[root@centos8 ~]#ss -ntl

#再次用不同方式关闭tomcat 
[root@centos8 ~]#catalina.sh stop
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:       /usr/local/jdk/jre
Using CLASSPATH:       
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
```

打开浏览器访问：http://tomcat:8080/，正常可以看到以下界面

扩展知识：tomcat 和 catalina 关系

```http
Tomcat的servlet容器在4.X版本中被Craig McClanahan(Apache Struts项目的创始人,也是Tomcat 的 Catalina 的架构师)重新设计为Catalina.即Catalina就是servlet容器。

Tomcat的核心分为3个部分: 
（1）Web容器:处理静态页面；
（2）JSP容器:把jsp页面翻译成一般的 servlet
（3）catalina: 是一个servlet容器,用于处理servlet

Catalina是美国西海岸靠近洛杉矶22英里的一个小岛，因为其风景秀丽而著名，曾被评为全美最漂亮的小岛。Servlet运行模块的最早开发者Craig McClanahan因为喜欢Catalina岛,故以Catalina命名他所开这个模块，另外在开发的早期阶段，Tomcat是被搭建在一个叫Avalon的服务器框架上，而Avalon则是Catalina岛上的一个小镇的名字，于是想一个与小镇名字相关联的单词也是自然而然。设计者估计是想把tomcat设计成最美的轻量级容器吧。
```

##### 3.2.2.2 配置 tomcat自启动的 service 文件

```bash
#创建tomcat专用帐户
[root@centos8 ~]#useradd -r -s /sbin/nologin tomcat

#准备service文件中相关环境文件
[root@centos8 ~]#vim /usr/local/tomcat/conf/tomcat.conf
[root@centos8 ~]#cat /usr/local/tomcat/conf/tomcat.conf
#两个变量至少设置一项才能启动 tomcat
JAVA_HOME=/usr/local/jdk
#JRE_HOME=/usr/local/jdk/jre 
#如果不指定上面变量,/var/log/messages文件中会出现下面无法启动错误提示
Mar 15 14:30:09 centos8 startup.sh[1530]: Neither the JAVA_HOME nor the JRE_HOME environment variable is defined
Mar 15 14:30:09 centos8 startup.sh[1530]: At least one of these environment variable is needed to run this program

[root@centos8 ~]#chown -R tomcat.tomcat /usr/local/tomcat/

#创建tomcat.service文件
[root@centos8 ~]#vim /lib/systemd/system/tomcat.service
[root@centos8 ~]#cat /lib/systemd/system/tomcat.service 
[Unit]
Description=Tomcat
#After=syslog.target network.target remote-fs.target nss-lookup.target
After=syslog.target network.target 
[Service]
Type=forking
EnvironmentFile=/usr/local/tomcat/conf/tomcat.conf
ExecStart=/usr/local/tomcat/bin/startup.sh
ExecStop=/usr/local/tomcat/bin/shutdown.sh
PrivateTmp=true
User=tomcat
Group=tomcat
[Install]
WantedBy=multi-user.target

[root@centos8 ~]#systemctl daemon-reload
[root@centos8 ~]#systemctl enable --now tomcat
Created symlink /etc/systemd/system/multi-user.target.wants/tomcat.service → 
/usr/lib/systemd/system/tomcat.service.
[root@centos8 ~]#systemctl status tomcat
● tomcat.service - Tomcat
   Loaded: loaded (/usr/lib/systemd/system/tomcat.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2020-02-08 23:37:02 CST; 5s ago
 Process: 14312 ExecStart=/usr/local/tomcat/bin/startup.sh (code=exited, status=0/SUCCESS)
 Main PID: 14320 (java)
   Tasks: 43 (limit: 4895)
   Memory: 64.2M
   CGroup: /system.slice/tomcat.service
           └─14320 /usr/local/jdk/jre/bin/java -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging>
Feb 08 23:37:02 centos8.localdomain systemd[1]: Starting Tomcat...
Feb 08 23:37:02 centos8.localdomain systemd[1]: Started Tomcat.

#查看日志
[root@centos8 ~]#tail /var/log/messages 
Mar 15 14:32:13 centos8 systemd[1]: Reloading.
Mar 15 14:32:23 centos8 systemd[1]: Starting Tomcat...
Mar 15 14:32:23 centos8 startup.sh[1575]: Tomcat started.
Mar 15 14:32:23 centos8 systemd[1]: Started Tomcat.
```

#### 3.2.3 实战案例： 一键安装 tomcat 脚本

```bash
[root@centos8 ~]#cat install_tomcat_for_centos.sh 
#!/bin/bash
#********************************************************************
. /etc/init.d/functions
DIR=`pwd`
JDK_FILE="jdk-8u251-linux-x64.tar.gz"
TOMCAT_FILE="apache-tomcat-8.5.57.tar.gz"
JDK_DIR="/usr/local"
TOMCAT_DIR="/usr/local"
install_jdk(){
if ! [  -f "$DIR/$JDK_FILE" ];then
   action "$JDK_FILE 文件不存在" false
   exit; 
elif [ -d $JDK_DIR/jdk ];then
   action "JDK 已经安装" false
   exit
else
   [ -d "$JDK_DIR" ] || mkdir -pv $JDK_DIR
fi
tar xvf $DIR/$JDK_FILE  -C $JDK_DIR
cd  $JDK_DIR && ln -s jdk1.8.* jdk 

cat > /etc/profile.d/jdk.sh <<EOF
export JAVA_HOME=$JDK_DIR/jdk
export JRE_HOME=\$JAVA_HOME/jre
export CLASSPATH=\$JAVA_HOME/lib/:\$JRE_HOME/lib/
export PATH=\$PATH:\$JAVA_HOME/bin
EOF

. /etc/profile.d/jdk.sh
java -version && action "JDK 安装完成" || { action "JDK 安装失败" false ; exit; }
}
install_tomcat(){
if ! [ -f "$DIR/$TOMCAT_FILE" ];then
   action "$TOMCAT_FILE 文件不存在" false
   exit; 
elif [ -d $TOMCAT_DIR/tomcat ];then
   action "TOMCAT 已经安装" false
   exit
else
 [ -d "$TOMCAT_DIR" ] || mkdir -pv $TOMCAT_DIR
fi
tar xf $DIR/$TOMCAT_FILE -C $TOMCAT_DIR
cd  $TOMCAT_DIR && ln -s apache-tomcat-*/ tomcat
echo "PATH=$TOMCAT_DIR/tomcat/bin:"'$PATH' > /etc/profile.d/tomcat.sh
id tomcat &> /dev/null || useradd -r -s /sbin/nologin tomcat
cat > $TOMCAT_DIR/tomcat/conf/tomcat.conf <<EOF
JAVA_HOME=$JDK_DIR/jdk
EOF
chown -R tomcat.tomcat $TOMCAT_DIR/tomcat/

cat > /lib/systemd/system/tomcat.service <<EOF
[Unit]
Description=Tomcat
#After=syslog.target network.target remote-fs.target nss-lookup.target
After=syslog.target network.target 
[Service]
Type=forking
EnvironmentFile=$TOMCAT_DIR/tomcat/conf/tomcat.conf
ExecStart=$TOMCAT_DIR/tomcat/bin/startup.sh
ExecStop=$TOMCAT_DIR/tomcat/bin/shutdown.sh
RestartSec=3
PrivateTmp=true
User=tomcat
Group=tomcat
[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable --now tomcat.service
systemctl is-active tomcat.service &> /dev/null && action "TOMCAT 安装完成" || { 
action "TOMCAT 安装失败" false ; exit; }
}
install_jdk 
install_tomcat
```

#### 3.2.4 实战案例： 一键安装 tomcat 脚本

```bash
[root@ubuntu1804 ~]#cat install_tomcat.sh 
#!/bin/bash
#
#********************************************************************
#Author: wangxiaochun
#QQ: 29308620
#Date: 2021-03-15
#FileName： install_tomcat.sh
#URL: http://www.wangxiaochun.com
#Description： The test script
#Copyright (C): 2021 All rights reserved
#********************************************************************
DIR=`pwd`
JDK_FILE="jdk-8u281-linux-x64.tar.gz"
TOMCAT_FILE="apache-tomcat-8.5.64.tar.gz"
JDK_DIR="/usr/local"
TOMCAT_DIR="/usr/local"
color () {
    RES_COL=60
    MOVE_TO_COL="echo -en \\033[${RES_COL}G"
    SETCOLOR_SUCCESS="echo -en \\033[1;32m"
    SETCOLOR_FAILURE="echo -en \\033[1;31m"
    SETCOLOR_WARNING="echo -en \\033[1;33m"
    SETCOLOR_NORMAL="echo -en \E[0m"
    echo -n "$2" && $MOVE_TO_COL
    echo -n "["
    if [ $1 = "success" -o $1 = "0" ] ;then
        ${SETCOLOR_SUCCESS}
        echo -n $" OK "    
    elif [ $1 = "failure" -o $1 = "1" ] ;then
        ${SETCOLOR_FAILURE}
        echo -n $"FAILED"
    else
        ${SETCOLOR_WARNING}
        echo -n $"WARNING"
    fi
    ${SETCOLOR_NORMAL}
    echo -n "]"
    echo                                                                         
                                                     
}
install_jdk(){
if ! [  -f "$DIR/$JDK_FILE" ];then
   color 1 "$JDK_FILE 文件不存在"
    exit; 
elif [ -d $JDK_DIR/jdk ];then
   color 1  "JDK 已经安装"
    exit
else
   [ -d "$JDK_DIR" ] || mkdir -pv $JDK_DIR
fi
tar xvf $DIR/$JDK_FILE  -C $JDK_DIR
cd  $JDK_DIR && ln -s jdk1.8.* jdk 
cat > /etc/profile.d/jdk.sh <<EOF
export JAVA_HOME=$JDK_DIR/jdk
export JRE_HOME=\$JAVA_HOME/jre
export CLASSPATH=\$JAVA_HOME/lib/:\$JRE_HOME/lib/
export PATH=\$PATH:\$JAVA_HOME/bin
EOF
. /etc/profile.d/jdk.sh
java -version && color 0 "JDK 安装完成" || { color 1  "JDK 安装失败" ; exit; }
}
install_tomcat(){
if ! [ -f "$DIR/$TOMCAT_FILE" ];then
   color 1 "$TOMCAT_FILE 文件不存在"
    exit; 
elif [ -d $TOMCAT_DIR/tomcat ];then
   color 1 "TOMCAT 已经安装"
    exit
else
   [ -d "$TOMCAT_DIR" ] || mkdir -pv $TOMCAT_DIR
fi
tar xf $DIR/$TOMCAT_FILE -C $TOMCAT_DIR
cd  $TOMCAT_DIR && ln -s apache-tomcat-*/ tomcat
echo "PATH=$TOMCAT_DIR/tomcat/bin:"'$PATH' > /etc/profile.d/tomcat.sh
id tomcat &> /dev/null || useradd -r -s /sbin/nologin tomcat
cat > $TOMCAT_DIR/tomcat/conf/tomcat.conf <<EOF
JAVA_HOME=$JDK_DIR/jdk
EOF
chown -R tomcat.tomcat $TOMCAT_DIR/tomcat/
cat > /lib/systemd/system/tomcat.service <<EOF
[Unit]
Description=Tomcat
#After=syslog.target network.target remote-fs.target nss-lookup.target
After=syslog.target network.target 
[Service]
Type=forking
EnvironmentFile=$TOMCAT_DIR/tomcat/conf/tomcat.conf
ExecStart=$TOMCAT_DIR/tomcat/bin/startup.sh
ExecStop=$TOMCAT_DIR/tomcat/bin/shutdown.sh
RestartSec=3
PrivateTmp=true
User=tomcat
Group=tomcat
[Install]
WantedBy=multi-user.target
EOF
systemctl daemon-reload
systemctl enable --now tomcat.service &> /dev/null
systemctl is-active tomcat.service &> /dev/null && color 0 "TOMCAT 安装完成" || { 
color 1 "TOMCAT 安装失败" ; exit; }
}
install_jdk 
install_tomcat
```

### 3.3 tomcat的文件结构和组成

#### 3.3.1 目录结构

| 目录    | 说明                              |
| ------- | --------------------------------- |
| bin     | 服务启动、停止等相关程序和文件    |
| conf    | 配置文件                          |
| lib     | 库目录                            |
| logs    | 日志目录                          |
| webapps | 应用程序，应用部署目录            |
| work    | jsp编译后的结果，建议提前预热访问 |

范例：查看tomcat相关目录和文件

```bash
[root@centos8 tomcat]#pwd
/usr/local/tomcat
[root@centos8 tomcat]#ls
bin conf lib logs README.md RUNNING.txt webapps
BUILDING.txt CONTRIBUTING.md LICENSE NOTICE RELEASE-NOTES temp  work
[root@centos8 tomcat]#ls bin
bootstrap.jar	ciphers.sh	daemon.sh	shutdown.bat	tomcat-native.tar.gz	catalina.bat       commons-daemon.jar	digest.bat	shutdown.sh   tool-wrapper.bat
catalina.sh         commons-daemon-native.tar.gz digest.sh         startup.bat   
  tool-wrapper.sh
catalina-tasks.xml configtest.bat               setclasspath.bat startup.sh   
    version.bat
ciphers.bat         configtest.sh                 setclasspath.sh   tomcat-juli.jar version.sh
[root@centos8 tomcat]#ls conf
Catalina             context.xml           logging.properties tomcat-users.xml
catalina.policy     jaspic-providers.xml server.xml         tomcat-users.xsd
catalina.properties jaspic-providers.xsd tomcat.conf         web.xml
[root@centos8 tomcat]#ls lib
annotations-api.jar       ecj-4.6.3.jar   servlet-api.jar     tomcat-i18n-fr.jar 
    tomcat-jni.jar
catalina-ant.jar         el-api.jar     tomcat-api.jar     tomcat-i18n-ja.jar 
    tomcat-util.jar
catalina-ha.jar           jasper-el.jar   tomcat-coyote.jar   tomcat-i18n-ko.jar 
    tomcat-util-scan.jar
catalina.jar             jasper.jar     tomcat-dbcp.jar     tomcat-i18n-ru.jar 
    tomcat-websocket.jar
catalina-storeconfig.jar jaspic-api.jar tomcat-i18n-de.jar tomcat-i18n-zhCN.jar websocket-api.jar
catalina-tribes.jar       jsp-api.jar     tomcat-i18n-es.jar tomcat-jdbc.jar
[root@centos8 tomcat]#ls logs
catalina.2020-02-09.log host-manager.2020-02-09.log localhost_access_log.2020-
02-09.txt
catalina.out             localhost.2020-02-09.log     manager.2020-02-09.log
[root@centos8 tomcat]#ls webapps/
docs examples host-manager manager ROOT
[root@centos8 tomcat]#ls work/
Catalina
[root@centos8 tomcat]#ls work/Catalina/
localhost
[root@centos8 tomcat]#ls work/Catalina/localhost/
docs examples host-manager manager ROOT
[root@centos8 tomcat]#ll -i work/Catalina/localhost/
total 0
68039883 drwxr-x--- 2 tomcat tomcat 6 Feb  9 11:02 docs
135579640 drwxr-x--- 2 tomcat tomcat 6 Feb  9 11:02 examples
202681358 drwxr-x--- 2 tomcat tomcat 6 Feb  9 11:02 host-manager
   571365 drwxr-x--- 2 tomcat tomcat 6 Feb  9 11:02 manager
   571364 drwxr-x--- 2 tomcat tomcat 6 Feb  9 11:02 ROOT
[root@centos8 tomcat]#ll -i webapps/
total 4
202681088 drwxr-x--- 15 tomcat tomcat 4096 Feb  9 11:02 docs
202681094 drwxr-x---  6 tomcat tomcat   83 Feb  9 11:02 examples
   571165 drwxr-x---  5 tomcat tomcat   87 Feb  9 11:02 host-manager
68039687 drwxr-x---  5 tomcat tomcat  103 Feb  9 11:02 manager
68039663 drwxr-x---  3 tomcat tomcat  283 Feb  9 11:02 ROOT
[root@centos8 tomcat]#tree work/Catalina/localhost/
work/Catalina/localhost/
├── docs
├── examples
├── host-manager
├── manager
└── ROOT
5 directories, 0 files
[root@centos8 tomcat]#curl http://10.0.0.8:8080/
#当访问过后，work目录中生成新文件
[root@centos8 tomcat]#tree work/Catalina/localhost/
work/Catalina/localhost/
├── docs
├── examples
├── host-manager
├── manager
└── ROOT
   └── org
       └── apache
           └── jsp
               ├── index_jsp.class #字节码文件
               └── index_jsp.java  #servlet文件
8 directories, 2 files
#tomcat会自动的将jsp文件生成java源文件，再编译成class文件
[root@centos8 tomcat]#less 
work/Catalina/localhost/ROOT/org/apache/jsp/index_jsp.java
/*
 * Generated by the Jasper component of Apache Tomcat
 * Version: Apache Tomcat/8.5.50
 * Generated at: 2020-02-09 03:20:20 UTC
 * Note: The last modified time of this file was set to
 *       the last modified time of the source file after
 *       generation to assist with modification tracking.
 */
package org.apache.jsp;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.jsp.*;
public final class index_jsp extends org.apache.jasper.runtime.HttpJspBase
   implements org.apache.jasper.runtime.JspSourceDependent,
                 org.apache.jasper.runtime.JspSourceImports {
 private static final javax.servlet.jsp.JspFactory _jspxFactory =
         javax.servlet.jsp.JspFactory.getDefaultFactory();
 private static java.util.Map<java.lang.String,java.lang.Long> 
_jspx_dependants;
```

#### 3.3.2 配置文件

##### 3.3.2.1 配置文件说明

官方帮助文档：http://tomcat.apache.org/tomcat-8.5-doc/index.html

在tomcat安装目录下的 conf 子目录中，有以下的 tomcat 的配置文件

| 文件名              | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| server.xml          | 主配置文件                                                   |
| web.xml             | 每个webapp只有部署后才能被访问，它的部署方式通常由web.xml进行定义，其存放位置为web-inf/目录中；此文件为所有的webapps提供默认部署相关配置，每个web应用也可以可以使用专用配置文件，来覆盖全局文件 |
| context.xml         | 用于定义所有web应用均需加载的Context配置，此文件为所有的webapps提供默认配置，每个web应用也可以是用自己的专用配置，它通常由专用的配置文件context.xml来定义，其存放位置为WEB-INF/目录中，覆盖全局的文件 |
| tomcat-users.xml    | 用户认证的账号和密码文件                                     |
| catalina.policy     | 当使用security选项启动Tomcat时，用于为Tomcat设置安全策略     |
| cataline.properties | Tomcat环境变量的配置，用于设定类加载器路径，以及一些与JVM调优相关参数 |
| logging.properties  | Tomcat日志系统相关的配置，可以修改日志级别和日志路径等       |

**注意：配置文件大小写敏感**

范例：查看配置文件

```bash
[root@centos8 conf]#pwd
/usr/local/tomcat/conf
[root@centos8 conf]#ls
Catalina             context.xml           logging.properties tomcat-users.xml
catalina.policy     jaspic-providers.xml server.xml         tomcat-users.xsd
catalina.properties jaspic-providers.xsd tomcat.conf         web.xml
[root@centos8 conf]#wc -l server.xml web.xml context.xml tomcat-users.xml 
catalina.policy catalina.properties logging.properties 
   167 server.xml
  4726 web.xml
    30 context.xml
    44 tomcat-users.xml
   271 catalina.policy
   214 catalina.properties
    75 logging.properties
  5527 total
[root@centos8 conf]#
```

范例: 主要配置文件内容

```bash
[root@centos8 ~]#grep -v '\-\-' /usr/local/tomcat/conf/server.xml 
[root@centos8 ~]#grep -v '\-\-' /usr/local/tomcat/conf/context.xml 
[root@centos8 ~]#grep -v '\-\-' /usr/local/tomcat/conf/tomcat-users.xml 
[root@centos8 ~]#cat /usr/local/tomcat/conf/tomcat.conf
JAVA_HOME=/usr/local/jdk
```

##### 3.3.2.2 日志文件 

参考文档: https://cwiki.apache.org/confluence/display/TOMCAT/Logging 

日志格式: https://tomcat.apache.org/tomcat-9.0-doc/config/valve.html#Access_Logging

```bash
%a - Remote IP address
%A - Local IP address
%b - Bytes sent, excluding HTTP headers, or '-' if zero
%B - Bytes sent, excluding HTTP headers
%h - Remote host name (or IP address if enableLookups for the connector is 
false)
%H - Request protocol
%l - Remote logical username from identd (always returns '-')
%m - Request method (GET, POST, etc.)
%p - Local port on which this request was received. See also %{xxx}p below.
%q - Query string (prepended with a '?' if it exists)
%r - First line of the request (method and request URI)
%s - HTTP status code of the response
%S - User session ID
%t - Date and time, in Common Log Format
%u - Remote user that was authenticated (if any), else '-'
%U - Requested URL path
%v - Local server name
%D - Time taken to process the request in millis. Note: In httpd %D is 
microseconds. Behaviour will be aligned to httpd in Tomcat 10 onwards.
%T - Time taken to process the request, in seconds. Note: This value has 
millisecond resolution whereas in httpd it has second resolution. Behaviour will 
be align to httpd in Tomcat 10 onwards.
%F - Time taken to commit the response, in millis
%I - Current request thread name (can compare later with stacktraces)
%X - Connection status when response is completed:
X = Connection aborted before the response completed.
+ = Connection may be kept alive after the response is sent.
- = Connection will be closed after the response is sent.
There is also support to write information incoming or outgoing headers, cookies, 
session or request attributes and special timestamp formats. It is modeled after 
the Apache HTTP Server log configuration syntax. Each of them can be used 
multiple times with different xxx keys:
%{xxx}i write value of incoming header with name xxx
%{xxx}o write value of outgoing header with name xxx
%{xxx}c write value of cookie with name xxx
%{xxx}r write value of ServletRequest attribute with name xxx
%{xxx}s write value of HttpSession attribute with name xxx
%{xxx}p write local (server) port (xxx==local) or remote (client) port 
(xxx=remote)
%{xxx}t write timestamp at the end of the request formatted using the enhanced 
SimpleDateFormat pattern xxx
```

范例: tomcat中的日志文件

```
[root@tomcat ~]#cat /usr/local/tomcat/conf/logging.properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements. See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License. You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
handlers = 1catalina.org.apache.juli.AsyncFileHandler, 
2localhost.org.apache.juli.AsyncFileHandler, 
3manager.org.apache.juli.AsyncFileHandler, 4hostmanager.org.apache.juli.AsyncFileHandler, java.util.logging.ConsoleHandler
.handlers = 1catalina.org.apache.juli.AsyncFileHandler, 
java.util.logging.ConsoleHandler
############################################################
# Handler specific properties.
# Describes specific configuration info for Handlers.
############################################################
1catalina.org.apache.juli.AsyncFileHandler.level = FINE
1catalina.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
1catalina.org.apache.juli.AsyncFileHandler.prefix = catalina.
1catalina.org.apache.juli.AsyncFileHandler.encoding = UTF-8
2localhost.org.apache.juli.AsyncFileHandler.level = FINE
2localhost.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
2localhost.org.apache.juli.AsyncFileHandler.prefix = localhost.
2localhost.org.apache.juli.AsyncFileHandler.encoding = UTF-8
3manager.org.apache.juli.AsyncFileHandler.level = FINE
3manager.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
3manager.org.apache.juli.AsyncFileHandler.prefix = manager.
3manager.org.apache.juli.AsyncFileHandler.encoding = UTF-8
4host-manager.org.apache.juli.AsyncFileHandler.level = FINE
4host-manager.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
4host-manager.org.apache.juli.AsyncFileHandler.prefix = host-manager.
4host-manager.org.apache.juli.AsyncFileHandler.encoding = UTF-8
java.util.logging.ConsoleHandler.level = FINE
java.util.logging.ConsoleHandler.formatter = org.apache.juli.OneLineFormatter
java.util.logging.ConsoleHandler.encoding = UTF-8
############################################################
# Facility specific properties.
# Provides extra control for each logger.
############################################################
org.apache.catalina.core.ContainerBase.[Catalina].[localhost].level = INFO
org.apache.catalina.core.ContainerBase.[Catalina].[localhost].handlers =
2localhost.org.apache.juli.AsyncFileHandler
org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/manager].level =
INFO
org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/manager].handlers 
= 3manager.org.apache.juli.AsyncFileHandler
org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/hostmanager].level = INFO
org.apache.catalina.core.ContainerBase.[Catalina].[localhost].[/hostmanager].handlers = 4host-manager.org.apache.juli.AsyncFileHandler
# For example, set the org.apache.catalina.util.LifecycleBase logger to log
# each component that extends LifecycleBase changing state:
#org.apache.catalina.util.LifecycleBase.level = FINE
# To see debug messages in TldLocationsCache, uncomment the following line:
#org.apache.jasper.compiler.TldLocationsCache.level = FINE
# To see debug messages for HTTP/2 handling, uncomment the following line:
#org.apache.coyote.http2.level = FINE
# To see debug messages for WebSocket handling, uncomment the following line:
#org.apache.tomcat.websocket.level = FINE

[root@centos8 ~]#ls /usr/local/tomcat/logs/ -1
catalina.2020-07-14.log  #tomcat服务日志
catalina.out               #tomcat服务日志
host-manager.2020-07-14.log   #host manager管理日志
localhost.2020-07-14.log       #默认主机日志
localhost_access_log.2020-07-14.txt  ##默认主机访问日志
manager.2020-07-14.log        #manager 管理日志
```

范例: tomcat的访问日志格式

```bash
#查看访问日志格式
[root@centos8 ~]#tail /usr/local/tomcat/conf/server.xml 
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
       <Valve className="org.apache.catalina.valves.AccessLogValve"
directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />   #说明: &quot;在html
中表示双引号"符号
     </Host>
   </Engine>
 </Service>
</Server>
#查看访问日志
[root@centos8 ~]#tail /usr/local/tomcat/logs/localhost_access_log.2020-07-14.txt
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET / HTTP/1.1" 200 11215
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /tomcat.css HTTP/1.1" 200 5581
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /tomcat.png HTTP/1.1" 200 5103
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /bg-nav.png HTTP/1.1" 200 1401
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /bg-upper.png HTTP/1.1" 200 3103
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /asf-logo-wide.svg HTTP/1.1" 200 27235
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /bg-middle.png HTTP/1.1" 200 1918
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /bg-button.png HTTP/1.1" 200 713
10.0.0.1 - - [14/Jul/2020:09:08:46 +0800] "GET /favicon.ico HTTP/1.1" 200 21630
```

#### 3.3.3 组件

##### 3.3.3.1 组件分层和分类 

**顶级组件** 

Server，代表整个Tomcat容器，一台主机可以启动多tomcat实例，需要确保端口不要产生冲突 

**服务类组件** 

Service，实现组织Engine和Connector，建立两者之间关联关系, service 里面只能包含一个Engine 

**连接器组件** 

Connector，有HTTP（默认端口8080/tcp）、HTTPS（默认端口8443/tcp）、AJP（默认端口 8009/tcp）协议的连接器，AJP（Apache Jserv protocol）是一种基于TCP的二进制通讯协议。 

**容器类** 

Engine、Host（虚拟主机）、Context(上下文件,解决路径映射)都是容器类组件，可以嵌入其它组件， 内部配置如何运行应用程序。 

**内嵌类** 

可以内嵌到其他组件内，valve、logger、realm、loader、manager等。以logger举例，在不同容器组 件内分别定义。 

**集群类组件** 

listener、cluster

##### 3.3.3.2 Tomcat 内部组成

| 名称      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| Server    | 服务器，Tomcat运行的进程实例，一个Server中可以有多个service，但通常就一个 |
| Service   | 服务，用来组织Engine和Connector的对应关系，一个service中只有一个Engine |
| Connector | 连接器，负责客户端的HTTP、HTTPS、AJP等协议连接。一个Connector只属于某一个Engine |
| Engine    | 即引擎，用来响应并处理用户请求。一个Engine上可以绑定多个Connector |
| Host      | 即虚拟主机，可以实现多虚拟主机，例如使用不同的主机头区分     |
| Context   | 应用的上下文，配置特定url路径映射和目录的映射关系：url=> directory |

每一个组件都由一个Java“类”实现，这些组件大体可分为以下几个类型：

```
顶级组件：Server
服务类组件：Service
连接器组件：http, https, ajp（apache jserv protocol）
容器类：Engine, Host, Context
被嵌套类：valve, logger, realm, loader, manager, ...
集群类组件：listener, cluster, ...
```

范例: 查看类

```
[root@centos8 ~]#grep className /usr/local/tomcat/conf/server.xml 
```

##### 3.3.3.3 核心组件

* Tomcat启动一个Server进程。可以启动多个Server，即tomcat的多实例, 但一般只启动一个 
* 创建一个Service提供服务。可以创建多个Service，但一般也只创建一个
  * 每个Service中，是Engine和其连接器Connector的关联配置
* 可以为这个Service提供多个连接器Connector，这些Connector使用了不同的协议，绑定了不同的 端口。其作用就是处理来自客户端的不同的连接请求或响应
* Service 内部还定义了Engine，引擎才是真正的处理请求的入口，其内部定义多个虚拟主机Host 
  * Engine对请求头做了分析，将请求发送给相应的虚拟主机 
  * 如果没有匹配，数据就发往Engine上的defaultHost缺省虚拟主机 
  * Engine上的缺省虚拟主机可以修改 
* Host 定义虚拟主机，虚拟主机有name名称，通过名称匹配 
* Context 定义应用程序单独的路径映射和配置

范例：多个组件关系 conf/server.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Server port="8005" shutdown="SHUTDOWN">
  <Service name="Catalina">
    <Connector port="8080" protocol="HTTP/1.1"connectionTimeout="20000"
               redirectPort="8443" />
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
    <Engine name="Catalina" defaultHost="localhost">
     <Host name="localhost"  appBase="webapps" unpackWARs="true"
autoDeploy="true">
         <Context >
         <Context />
     </Host>
    </Engine>
  </Service>
</Server>
```

##### 3.3.3.4 tomcat 处理请求过程

假设来自客户的请求为：http://localhost:8080/test/index.jsp

* 浏览器端的请求被发送到服务端端口8080，Tomcat进程监听在此端口上。通过侦听的HTTP/1.1  Connector获得此请求。 
* Connector把该请求交给它所在的Service的Engine来处理，并等待Engine的响应 
* Engine获得请求localhost:8080/test/index.jsp，遍历它所有虚拟主机Host 
* Engine匹配到名为localhost的Host。如果匹配不到,就把请求交给该Engine中的defaultHost处理 
* localhost Host获得请求/test/index.jsp，匹配它所拥有的所有Context 
* Host匹配到路径为/test的Context 
* path=/test的Context获得请求index.jsp，在它的mapping table中寻找对应的servlet 
* Context匹配到URL PATTERN为 *.jsp 的servlet，对应于JspServlet类构造HttpServletRequest对象和HttpServletResponse对象，作为参数调用JspServlet的doGet或doPost方法。 
* Context把执行完了之后的HttpServletResponse对象返回给Host 
* Host把HttpServletResponse对象返回给Engine 
* Engine把HttpServletResponse对象返回给Connector 
* Connector把HttpServletResponse对象返回给浏览器端

### 3.4 应用部署

#### 3.4.1 tomcat的根目录结构

Tomcat中默认网站根目录是$CATALINA_BASE/webapps/ 

在Tomcat中部署主站应用程序和其他应用程序，和之前WEB服务程序不同。

**nginx**

假设在nginx中部署2个网站应用eshop、forum，假设网站根目录是/data/nginx/html，那么部署可以是这样的。 

eshop解压缩所有文件放到 /data/nginx/html/目录下,forum的文件放 

在 /data/nginx/html/forum/下。

最终网站链接有以下对应关系

```http
http://localhost/ 对应于eshop的应用，即 /data/nginx/html/
http://localhost/forum/ 对应于forum的应用，即/data/nginx/html/forum/
```

**Tomcat**

Tomcat中默认网站根目录是$CATALINA_BASE/webapps/ 

在Tomcat的webapps目录中，有个非常特殊的目录ROOT，它就是网站默认根目录。 

将eshop解压后的文件放到这个$CATALINA_BASE/webapps/ROOT中。 

bbs解压后文件都放在$CATALINA_BASE/webapps/forum目录下。 

$CATALINA_BASE/webapps下面的每个目录都对应一个Web应用,即WebApp 

最终网站链接有以下对应关系

```
http://localhost/ 对应于eshop的应用WebApp，即$CATALINA_BASE/webapps/ROOT/目录,
http://localhost/forum/ 对应于forum的应用WebApp，即$CATALINA_BASE/webapps/forum/
```

如果同时存在$CATALINA_BASE /webapps/ROOT/forum ，仍以 $CATALINA_BASE/webapps/forum/ 优先生效 

每一个虚拟主机都可以使用appBase指令配置自己的站点目录，使用appBase目录下的ROOT目录作为主站目录。

范例: 主页目录和编码

```bash
[root@centos8 ~]#cat /usr/local/tomcat/webapps/ROOT/index.html 
<h1>马哥教育</h1>

[root@centos8 ~]#curl 10.0.0.8:8080/index.html -I
HTTP/1.1 200
Accept-Ranges: bytes
ETag: W/"22-1594212097000"
Last-Modified: Wed, 08 Jul 2020 12:41:37 GMT
Content-Type: text/html  #tomcat无指定编码,浏览器自动识别为GBK,可能会导致乱码
Content-Length: 22
Date: Wed, 08 Jul 2020 13:06:03 GMT

#httpd服务器默认指定编码为UTF-8,因为服务器本身不会出现乱码
#nginx服务器默认在响应头部没有批定编码,也会出现乱码
[root@centos8 ~]#curl 10.0.0.18/index.html -I
HTTP/1.1 200 OK
Date: Wed, 08 Jul 2020 13:07:57 GMT
Server: Apache/2.4.37 (centos)
Last-Modified: Wed, 08 Jul 2020 12:59:55 GMT
ETag: "16-5a9edaf39d274"
Accept-Ranges: bytes
Content-Length: 22
Content-Type: text/html; charset=UTF-8

#浏览器的设置默认不是UTF-8,可能会导致乱码
```

```bash
#修改网页指定编码
[root@centos8 ~]#cat /usr/local/tomcat/webapps/ROOT/index.html
<html>
<head>
<meta http-equiv=Content-Type content="text/html;charset=utf-8">
<title>tomcat</title>
</head>
<h1>马哥教育</h1>
```

#### 3.4.2 JSP WebApp目录结构

$CATALINA_BASE/webapps下面的每个目录对应的WebApp,可能有以下子目录,但下面子目录是非必须的

* 主页配置：默认按以下顺序查找主页文件 index.html，index.htm、index.jsp 
* WEB-INF/：当前目录WebApp的私有资源路径，通常存储当前应用使用的web.xml和context.xml 配置文件 
* META-INF/：类似于WEB-INF，也是私有资源的配置信息，和WEB-INF/目录一样浏览器无法访问 
* classes/：类文件，当前webapp需要的类 
* lib/：当前应用依赖的jar包

#### 3.4.3 主页设置

##### 3.4.3.1 全局配置实现修改默认主页文件

默认情况下 tomcat 会在$CATALINA_BASE/webapps/ROOT/目录下按以下次序查找文件,找到第一个则进行显示

* index.html 
* index.htm 
* index.jsp

可以通过修改 $CATALINA_BASE/conf/web.xml 中的下面 标签 内容修改默认页文件

范例: 修改默认主页文件

```bash
[root@centos8 tomcat]#pwd
/usr/local/tomcat
[root@centos8 tomcat]#echo '<h1>www.magedu.org</h1>' > webapps/ROOT/index.html
[root@centos8 tomcat]#curl http://127.0.0.1:8080/
<h1>www.magedu.org</h1>
[root@centos8 tomcat]#tail conf/web.xml
 <!-- here, so be sure to include any of the default values that you wish  -->
 <!-- to use within your application.                                       -->
   <welcome-file-list>
       <welcome-file>index.html</welcome-file>
       <welcome-file>index.htm</welcome-file>
       <welcome-file>index.jsp</welcome-file>
   </welcome-file-list>
</web-app>
[root@centos8 tomcat]#vim conf/web.xml
[root@centos8 tomcat]#tail conf/web.xml
 <!-- here, so be sure to include any of the default values that you wish  -->
 <!-- to use within your application.                                       -->
   <welcome-file-list>
       <welcome-file>index.jsp</welcome-file>
       <welcome-file>index.htm</welcome-file>
       <welcome-file>index.html</welcome-file>
   </welcome-file-list>
</web-app>
[root@centos8 tomcat]#systemctl restart tomcat
[root@centos8 tomcat]#curl http://127.0.0.1:8080/
```

##### 3.4.3.2 WebApp的专用配置文件

将上面主配置文件conf/web.xml中的 标签 内容，复制 到/usr/local/tomcat/webapps/ROOT/WEB-INF/web.xml中，如下所示:

范例: 针对主站点根目录设置专用配置文件

```bash
[root@centos8 tomcat]#vim webapps/ROOT/WEB-INF/web.xml 
[root@centos8 tomcat]#cat webapps/ROOT/WEB-INF/web.xml 
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                     http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
  version="3.1"
  metadata-complete="true">
 <display-name>Welcome to Tomcat</display-name>
 <description>
     Welcome to Tomcat
 </description>
   <welcome-file-list>
       <welcome-file>index.html</welcome-file>   #修改三个文件的顺序
       <welcome-file>index.htm</welcome-file>
       <welcome-file>index.jsp</welcome-file>
   </welcome-file-list>
</web-app>
#配置修改后，无需重启tomcat服务，即可观察首页变化
[root@centos8 tomcat]#curl http://127.0.0.1:8080/
```

范例: 针对特定APP目录设置专用配置文件

```bash
[root@centos8 tomcat]#cp -a webapps/ROOT/WEB-INF/   webapps/magedu/
[root@centos8 tomcat]#echo /usr/local/tomcat/webapps/magedu/test.html > 
webapps/magedu/test.html
[root@centos8 tomcat]#tree webapps/magedu/
webapps/magedu/
├── index.htm
├── index.html
├── test.html
└── WEB-INF
   └── web.xml
1 directory, 4 files
[root@centos8 tomcat]#cat webapps/magedu/WEB-INF/web.xml
......
 <description>
     Welcome to Tomcat
 </description>
   <welcome-file-list>
   <welcome-file>test.html</welcome-file>  #修改默认页面文件的顺序
   <welcome-file>index.htm</welcome-file>
   <welcome-file>index.html</welcome-file>
   </welcome-file-list>
</web-app>
#注意修改属性
[root@centos8 tomcat]#chown -R tomcat.tomcat webapps/magedu/
[root@centos7 ~]#curl http://www.magedu.org:8080/magedu/
/usr/local/tomcat/webapps/magedu/test.html
```

**配置规则：** 

* webApp的专有配置优先于系统的全局配置
* 修改系统的全局配置文件，需要重新启动服务生效 
* 修改 webApp的专有配置，无需重启即可生效

#### 3.4.4 应用部署实现

##### 3.4.4.1 WebApp应用的归档格式

* war：WebApp打包,类zip格式文件,通常包括一个应用的所有资源,比如jsp,html,配置文件等 
* .jar：EJB类文件的打包压缩类zip格式文件，,包括很多的class文件, 网景公司发明 
* .rar：资源适配器类打包文件，目前已不常用 
* .ear：企业级WebApp打包，目前已不常用

传统应用开发测试后，通常打包为war格式，这种文件部署到Tomcat的webapps目录下，并默认会自动解包展开和部署上线。

```bash
#conf/server.xml中文件配置
<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
```

##### 3.4.4.2 部署方式

* 部署Deploy：将webapp的源文件放置到目标目录，通过web.xml和context.xml文件中配置的路径就可以访问该webapp，通过类加载器加载其特有的类和依赖的类到JVM上，即：最终用户可以通过浏览器访问该应用 
  * 自动部署：Tomcat一旦发现多了一个web应用APP.war包，默认会自动把它解压缩，加载并启动起来 
  * 手动部署 
    * 冷部署：将webapp放到指定目录，才去启动Tomcat服务 
    * 热部署：Tomcat服务不停止，需要依赖manager、ant脚本、tcd（tomcat client  deployer）等工具 
* 反部署undeploy：停止webapp运行，并从JVM上清除已经加载的类，从Tomcat应用目录中移除部署的文件 
* 启动start：是webapp能够访问 
* 停止stop：webapp不能访问，不能提供服务，但是JVM并不清除它

##### 3.4.4.3 部署WebApp的目录结构

常见开发项目目录组成

```bash
#目录结构一般由开发用工具自动生成，以下模拟生成相关目录
mkdir projects/myapp/{WEB-INF,META-INF,classes,lib} -pv
mkdir: 已创建目录 "projects"
mkdir: 已创建目录 "projects/myapp"
mkdir: 已创建目录 "projects/myapp/WEB-INF"
mkdir: 已创建目录 "projects/myapp/META-INF"
mkdir: 已创建目录 "projects/myapp/classes"
mkdir: 已创建目录 "projects/myapp/lib"
#常见应用首页，内容就用前面的test.jsp内部
vi projects/myapp/index.jsp 
#手动复制项目目录到webapps目录下去
cp -r projects/myapp/ /usr/local/tomcat/webapps/
#注意权限和属性
chown -R tomcat.tomcat /usr/local/tomcat/webapps/myapp
#访问http://YourIP:8080/myapp/
```

##### 3.4.4.4 实战案例：手动的应用部署

###### 3.4.4.4.1 部署主页目录下的应用WebApp

```bash
[root@centos8 tomcat]#vim webapps/ROOT/test.jsp
[root@centos8 tomcat]#cat webapps/ROOT/test.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
<%
out.println("hello jsp");
%>
<br>
<%=request.getRequestURL()%>
</body>
</html>
[root@centos8 tomcat]#curl http://127.0.0.1:8080/test.jsp
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
hello jsp
<br>
http://127.0.0.1:8080/test.jsp
</body>
</html>
[root@centos8 tomcat]#tree work/Catalina/localhost/ROOT/
work/Catalina/localhost/ROOT/
└── org
   └── apache
       └── jsp
           ├── test_jsp.class
           └── test_jsp.java
3 directories, 2 files
[root@centos8 tomcat]#
```

###### 3.4.4.4.2 部署一个子目录的应用WebApp

```bash
[root@centos8 tomcat]#pwd
/usr/local/tomcat
[root@centos8 tomcat]#mkdir webapps/app1/
#利用之前实验的文件生成新应用
[root@centos8 tomcat]#cp -p webapps/ROOT/test.jsp webapps/app1/
[root@centos8 tomcat]#chown -R tomcat.tomcat webapps/app1/
[root@centos8 tomcat]#tree webapps/app1/
webapps/testapp1/
└── test.jsp
0 directories, 1 file
[root@centos8 tomcat]#curl http://127.0.0.1:8080/app1/test.jsp
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
hello jsp
<br>
<%=request.getRequestURL()%>
</body>
</html>
[root@centos8 tomcat]#tree work/Catalina/localhost/app1/
work/Catalina/localhost/app1/
└── org
   └── apache
       └── jsp
           ├── test_jsp.class
           └── test_jsp.java
3 directories, 2 files
[root@centos8 tomcat]#
#删除应用
[root@centos8 tomcat]#rm -rf webapps/app1/
[root@centos8 tomcat]#ls webapps/
docs examples host-manager manager ROOT
[root@centos8 tomcat]#ls work/Catalina/localhost/
docs examples host-manager manager ROOT
```

##### 3.4.4.5 实战案例：自动的应用部署war包

###### 3.4.4.5.1 制作应用的war包文件

```bash
[root@centos8 ~]#ls /data/app2/
test.html test.jsp
[root@centos8 ~]#cat /data/app2/test.html 
<h1>This is test html </h1>
[root@centos8 ~]#cat /data/app2/test.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
<%
out.println("test jsp");
%>
<br>
<%=request.getRequestURL()%>
</body>
</html>
[root@centos8 ~]#cd /data/app2/
#生成war包文件app2.war,此文件的名称决定了tomcat子目录的名称
[root@centos8 app2]#jar cvf /data/app2.war *
added manifest
adding: test.html(in = 28) (out= 27)(deflated 3%)
adding: test.jsp(in = 329) (out= 275)(deflated 16%)
[root@centos8 app2]#cd /data/app2/
[root@centos8 app2]#ls
test.html test.jsp
[root@centos8 app2]#cd /data/
[root@centos8 data]#ls
app2 app2.war
[root@centos8 data]#file app2.war
app2.war: Java archive data (JAR)
[root@centos8 data]#chown tomcat.tomcat /data/app2.war
```

###### 3.4.4.5.2 自动应用部署上面的war包

```bash
[root@centos8 tomcat]#pwd
/usr/local/tomcat
[root@centos8 tomcat]#ls webapps/
docs examples host-manager manager ROOT
[root@centos8 tomcat]#ls work/Catalina/localhost/
docs examples host-manager manager ROOT
[root@centos8 tomcat]#cp -p /data/app2.war webapps/
#再次查看，tomcat将app2.war自动解压缩
[root@centos8 tomcat]#ll webapps/
total 8
drwxr-x--- 15 tomcat tomcat 4096 Feb  9 11:02 docs
drwxr-x---  6 tomcat tomcat   83 Feb  9 11:02 examples
drwxr-x---  5 tomcat tomcat   87 Feb  9 11:02 host-manager
drwxr-x---  5 tomcat tomcat  103 Feb  9 11:02 manager
drwxr-x---  3 tomcat tomcat  300 Feb  9 19:59 ROOT
drwxr-x---  3 tomcat tomcat   55 Feb  9 20:14 app2
-rw-r--r--  1 tomcat tomcat  862 Feb  9 20:05 app2.war
[root@centos8 tomcat]#ll webapps/app2
total 8
drwxr-x--- 2 tomcat tomcat  44 Feb  9 20:14 META-INF
-rw-r----- 1 tomcat tomcat  28 Feb  9 20:03 test.html
-rw-r----- 1 tomcat tomcat 329 Aug 30 02:30 test.jsp
#work目录会自动生成对应的app2的子目录，但目录内无内容
[root@centos8 tomcat]#tree work/Catalina/localhost/app2/
work/Catalina/localhost/app2/
0 directories, 0 files
#访问jsp文件后，tomcat会自动将jsp转换和编译生成work目录下对应的java和class文件
[root@centos8 tomcat]#curl http://127.0.0.1:8080/app2/test.jsp
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
hello jsp
</body>
</html>
[root@centos8 tomcat]#tree work/Catalina/localhost/app2/
work/Catalina/localhost/app2/
└── org
   └── apache
       └── jsp
           ├── test_jsp.class
           └── test_jsp.java
3 directories, 2 files
[root@centos8 tomcat]#curl http://127.0.0.1:8080/app2/test.html
<h1>This is test html </h1>
[root@centos8 tomcat]#tree work/Catalina/localhost/app2/
work/Catalina/localhost/estapp2/
└── org
   └── apache
       └── jsp
           ├── test_jsp.class
           └── test_jsp.java
3 directories, 2 files
#自动删除（反部署）
#[root@centos8 tomcat]#rm -f webapps/app2.war  
[root@centos8 tomcat]#ls webapps/
docs examples host-manager manager ROOT app2
#过几秒再查看，发现app2目录也随之删除
[root@centos8 tomcat]#ls webapps/
docs examples host-manager manager ROOT
[root@centos8 tomcat]#ls webapps/
docs examples host-manager manager ROOT
[root@centos8 tomcat]#ls work/Catalina/localhost/
docs examples host-manager manager ROOT
```

##### 3.4.4.6 实站案例: 部署基于JAVA的博客系统 JPress

JPress 是一个使用Java开发的类似WordPress的产品的建站神器，目前已有超过10万+网站使用JPress  搭建，其中包括多个政府机构，200+上市公司，中科院、红十字会等。

开源协议: LGPL-3.0 

官方网站: http://www.jpress.io/

```bash
[root@centos8 ~]#cd /usr/local/tomcat/webapps/
[root@centos8 webapps]#ls
docs examples host-manager jpress-v3.2.1.war manager ROOT
#自动解压缩生成jpress-v3.2.1目录
[root@centos8 webapps]#ls
docs examples host-manager jpress-v3.2.1 jpress-v3.2.1.war manager ROOT
#生成软链接
[root@centos8 webapps]#ln -s jpress-v3.2.1 jpress
[root@centos8 webapps]#ls
docs examples host-manager jpress jpress-v3.2.1 jpress-v3.2.1.war manager 
ROOT
[root@centos8 webapps]#
#准备数据库和用户授权
[root@centos8 ~]#yum -y install mysql-server
[root@centos8 ~]#;systemctl enable --now mysqld
[root@centos8 ~]#mysql 
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.17 Source distribution
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> create user jpress@'10.0.0.%' identified by '123456';
mysql> create database jpress;
mysql> grant all on jpress.* to jpress@'10.0.0.%';
```

上传的图片存放路径

```bash
[root@centos8 webapps]#tree jpress/attachment/
jpress/attachment/
└── 20210110
   └── 2169440a4e75459d8a420f4b245565d4.jpg
1 directory, 1 file
```

##### 3.4.4.7 基于WEB的管理Server status和Manager APP实现应用部署

tomcat 提供了基于WEB的管理页面,默认由 tomcat-admin-webapps.noarch包提供相关文件

###### 3.4.4.7.1 实现WEB的管理Server status和Manager APP

打开浏览器可以访问tomcat管理的默认管理页面，点击下图两个按钮都会出现下面提示403的错误提示

默认的管理页面被禁用，启用方法如下 

* 修改conf/conf/tomcat-users.xml

```bash
[root@centos8 tomcat]#ls conf/
Catalina             context.xml           logging.properties tomcat-users.xml
catalina.policy     jaspic-providers.xml server.xml         tomcat-users.xsd
catalina.properties jaspic-providers.xsd tomcat.conf         web.xml
#查看配置信息
[root@centos8 tomcat]#cat conf/server.xml 
<GlobalNamingResources>
   <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
   <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />  #由此文件指定授权用户信息
 </GlobalNamingResources>
```

用户认证，配置文件是conf/tomcat-users.xml。打开tomcat-users.xml，我们需要一个角色managergui。

```bash
[root@centos8 tomcat]#vim conf/tomcat-users.xml 
<tomcat-users xmlns="http://tomcat.apache.org/xml"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
#加下面两行，指定用户和密码
 <role rolename="manager-gui"/>
 <user username="admin" password="123456" roles="manager-gui"/>
</tomcat-users>
#修改全局配置文件需要重启服务生效
[root@centos8 tomcat]#systemctl restart tomcat
```

* 修改webapps/manager/META-INF/context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context antiResourceLocking="false" privileged="true" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.
(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreve
ntionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```

查看正则表达式就知道是本地访问了，由于当前访问地址是192.168.x.x，可以修改正则表达式为

```
allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|192\.168\.\d+\.\d+"
```

范例：

```bash
[root@centos8 tomcat]#vim webapps/manager/META-INF/context.xml
<Context antiResourceLocking="false" privileged="true" >
 <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|10\.0\.0\.\d+" />
 <Manager sessionAttributeValueClassNameFilter="java\.lang\.
(?:Boolean|Integer|Long|Number|String)|org\.apach
e\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.
(?:Linked)?HashMap"/>
</Context> 
#修改WebApp的配置无需重启服务即可生效
```

* 再次通过浏览器访问两个按钮Server Status和Manager App，可以看到以下管理界面，输入前面 的用户和密码进行登录

###### 3.4.4.7.2 基于WEB应用程序管理器实现APP的部署

Web 应用程序管理界面可以实现以下功能 

Applications 应用程序管理，可以启动、停止、重加载、反部署、清理过期session 

Deploy 可以热部署，也可以部署war文件。

**方式1: 指定目录部署软件**

```bash
[root@centos8 ~]#mkdir -p /data/myapp/
[root@centos8 ~]#echo /data/myapp/index.html > /data/myapp/index.html
#按下面信息添写,实现下面链接的访问
http://10.0.0.8:8080/test1/
```

```bash
Context Path (required): 指定通过浏览器访问的虚拟目录
WAR or Directory URL：指定真正存放文件的实际磁盘目录路径
```

```bash
#自动将/data/myapp目录下的数据复制到webapps/test1下面
[root@centos8 ~]#tree /usr/local/tomcat/webapps/test1/
/usr/local/tomcat/webapps/test1/
└── index.html
0 directories, 1 file
[root@centos8 ~]#cat /usr/local/tomcat/webapps/test1/index.html 
/data/myapp/index.html
```

**方式2: 部署war包文件**

#### 3.4.5 常见配置详解

##### 3.4.5.1 端口8005/tcp 安全配置管理

在conf/server.xml 有以下内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Server port="8005" shutdown="SHUTDOWN">
  <Service name="Catalina">
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
    <Engine name="Catalina" defaultHost="localhost">
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
      </Host>
    </Engine>
  </Service>
</Server>
```

```xml
<Server port="8005" shutdown="SHUTDOWN">
```

8005是Tomcat的管理端口，默认监听在127.0.0.1上。无需验证就可发送SHUTDOWN (大小写敏感)这个 字符串，tomcat接收到后就会关闭此Server。 

此管理功能建议禁用，可将SHUTDOWN改为一串猜不出的字符串实现 

或者port修改成 0, 会使用随机端口,如:36913 

port设为-1等无效端口,将关闭此功能 

此行不能被注释,否则无法启动tomcat服务

范例:

```xml
<Server port="8005" shutdown="44ba3c71d57f494992641b258b965f28">
```

范例：修改8005/tcp端口管理命令

```
[root@centos8 ~]#ss -ntl
State       Recv-Q       Send-Q                     Local Address:Port           Peer Address:Port   
LISTEN      0             128                              0.0.0.0:22           0.0.0.0:*         
LISTEN      0             100                                   *:8080           *:*         
LISTEN      0             128                                 [::]:22           [::]:*         
LISTEN      0             1                     [::ffff:127.0.0.1]:8005          *:*            LISTEN      0             100                                   *:8009            *:*          
[root@centos8 ~]#telnet 127.0.0.1 8005
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
SHUTDOWN #执行命令关闭tomcat
Connection closed by foreign host.
[root@centos8 ~]#ss -ntl
State       Recv-Q       Send-Q                 Local Address:Port             Peer Address:Port     
LISTEN       0             128                          0.0.0.0:22               0.0.0.0:*         
LISTEN       0             128                             [::]:22               [::]:* 
[root@centos8 tomcat]#vim conf/server.xml 
<Server port="8005" shutdown="magedu">
[root@centos8 tomcat]#systemctl start tomcat
[root@centos8 tomcat]#telnet 127.0.0.1 8005
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
SHUTDOWN
Connection closed by foreign host.
[root@centos8 tomcat]#ss -ntl
State       Recv-Q       Send-Q                     Local Address:Port           Peer Address:Port   
LISTEN      0             128                              0.0.0.0:22           0.0.0.0:* 
LISTEN      0             100                                   *:8080         *:*         
LISTEN      0             128                                 [::]:22           [::]:*         
LISTEN      0             1                     [::ffff:127.0.0.1]:8005             *:*         
LISTEN      0             100                                   *:8009            *:*  
[root@centos8 tomcat]#telnet 127.0.0.1 8005
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
magedu
Connection closed by foreign host.
[root@centos8 tomcat]#ss -ntl
State       Recv-Q       Send-Q                 Local Address:Port             Peer Address:Port     
LISTEN       0             128                          0.0.0.0:22                0.0.0.0:*         
LISTEN       0             128                             [::]:22                [::]:*     
[root@centos8 tomcat]#
```

##### 3.4.5.2 显示指定的http服务器版本信息

默认不显示tomcat的http的Server头信息, 可以指定tomcat的http的Server头信息为相应的值

```xml
#conf/server.xml
<Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"
redirectPort="8443" Server="SOME STRING"/> 
```

范例:

```bash
[root@centos8 ~]#curl -I 127.0.0.1:8080 
HTTP/1.1 200
Content-Type: text/html;charset=UTF-8
Transfer-Encoding: chunked
Date: Fri, 17 Jul 2020 08:32:52 GMT
#修改配置,指定想显示的tomcat版本
[root@centos8 ~]#vim /usr/local/tomcat/conf/server.xml 
......
 <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" Server="WangServer"/> 
......
[root@centos8 ~]#systemctl restart tomcat
[root@centos8 ~]#curl 127.0.0.1:8080 -I
HTTP/1.1 200
Content-Type: text/html;charset=UTF-8
Transfer-Encoding: chunked
Date: Fri, 17 Jul 2020 08:34:17 GMT
Server: WangServer
```

##### 3.4.5.3 其它配置

conf/server.xml中可以配置service，connector, Engine,Host等 

* service配置

一般情况下，一个Server实例配置一个Service，name属性相当于该Service的ID。

```xml
<Service name="Catalina">
```

* 连接器配置

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

redirectPort，如果访问HTTPS协议，自动转向这个连接器。但大多数时候，Tomcat并不会开启 HTTPS，因为Tomcat往往部署在内部，HTTPS性能较差

* 引擎配置

```xml
<Engine name="Catalina" defaultHost="localhost">
```

* defaultHost 配置

defaultHost指向内部定义某虚拟主机。缺省虚拟主机可以改动，默认localhost。

```xml
<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
```

##### 3.4.5.4 多虚拟主机配置

###### 3.4.5.4.1 多虚拟主机配置说明

* name 必须是主机名，用主机名来匹配 
* appBase 当前主机的网页根目录，是相对于 $CATALINA_HOME ，也可以使用绝对路径 
* unpackWARs 是否自动解压war格式 
* autoDeploy 热部署，自动加载并运行应用

虚拟主机配置过程

* 再添加和配置一个新的虚拟主机，并将myapp部署到/data/webapps目录下

```xml
vim conf/server.xml  
#在文件最后面增加下面内容
<Host name="web1.magedu.org" appBase="/data/webapps/" unpackWARs="True"
autoDeploy="false">
#虚拟主机专有访问日志
<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
prefix="web1_access_log" suffix=".txt" pattern="%h %l %u %t &quot;%r&quot; %s 
%b" />
</Host>
#以下行是自带的不需要修改
</Engine>
</Service>
</Server>   
#或者如果不加日志也可以用下面简化写法
<Host name="web1.magedu.org" appBase="/data/webapps/" unpackWARs="True"
autoDeploy="false"/>
```

* 准备虚拟主机的数据目录

```bash
常见虚拟主机根目录
# mkdir /data/webapps/ROOT -pv
# chown -R tomcat.tomcat /data/webapps
# echo web1.magedu.org > /data/webapps/ROOT/index.html
```

* 测试

刚才在虚拟主机中主机名定义node1.magedu.org，所以需要主机在本机手动配置一个域名解析。 

如果是windows，修改在C:\Windows\System32\drivers\etc下的hosts文件，需要管理员权限。 

使用http://web1.magedu.org:8080/访问查看

###### 3.4.5.4.2 实战案例：tomcat实现多虚拟主机

```bash
[root@centos8 tomcat]#pwd
/usr/local/tomcat
[root@centos8 tomcat]#vim conf/server.xml 
[root@centos8 tomcat]#tail conf/server.xml
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
     </Host>
#添加了以下四行
     <Host  name="node1.magedu.org"  appBase="/data/webapps1">
     </Host>
     <Host  name="node2.magedu.org"  appBase="/data/webapps2">
     </Host>
   </Engine>
 </Service>
</Server>
#对每个虚拟主机，准备数据
[root@centos8 ~]#mkdir /data/webapps{1,2}/ROOT -pv 
mkdir: created directory '/data/webapps1/ROOT'
mkdir: created directory '/data/webapps2/ROOT'
[root@centos8 ~]#cat /data/webapps1/ROOT/index.jsp 
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
<br>
<%=request.getRequestURL()%>
</body>
</html>
[root@centos8 ~]#cat /data/webapps2/ROOT/index.jsp 
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
<br>
<%=request.getRequestURL()%>
</body>
</html>
[root@centos8 ~]#

#设置权限
[root@centos8 ~]#chown -R tomcat.tomcat /data/webapps{1,2}/
#准备虚拟主机的名称解析
[root@centos8 ~]#cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
centos8.localdomain
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.0.0.8 node1.magedu.org node2.magedu.org
[root@centos8 ~]#curl http://node1.magedu.org:8080/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
http://node1.magedu.org:8080/
</body>
</html>
[root@centos8 ~]#curl http://node2.magedu.org:8080/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
http://node2.magedu.org:8080/
</body>
</html>
```

###### 3.4.5.4.3 实战案例：修改tomcat实现多虚拟主机的端口为80

```bash
 [root@centos8 ~]#vim /usr/local/tomcat/conf/server.xml
 <Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
               
#注意: 因为以tomcat用户运行,不能直接使用1024以下的端口,需要修改tomcat的运行身份,否则会出现下
面错误
[root@centos8 ~]#tail -f /usr/local/tomcat/logs/catalina.out
 Caused by: java.net.SocketException: Permission denied
[root@centos8 ~]#vim /lib/systemd/system/tomcat.service 
[Service]
.....
#User=tomcat
#Group=tomcat
[root@centos8 ~]#systemctl daemon-reload
[root@centos8 ~]#systemctl restart tomcat
```

##### 3.4.5.5 基于web方式的Host Manager虚拟主机管理

可以通过tomcat的管理页面点下面Host Manager按钮进入管理虚拟主机的页面

默认Host Manager 管理页被禁用，会出现下面提示，解决方法类似于3.4.4.6

###### 3.4.5.5.1 允许本机访问

配置如下

```xml
[root@centos8 tomcat]#vim conf/tomcat-users.xml 
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
    <role rolename="manager-gui"/> #3.4.4.6添加的内容
    <role rolename="admin-gui" />   #添加新的role 
    <user username="admin" password="123456" roles="manager-gui,admin-gui"/> #再
加新role
    
</tomcat-users>
[root@centos8 tomcat]#systemctl restart tomcat
```

重启Tomcat后，点击"Host Manager"按钮

###### 3.4.5.5.2 允许远程主机访问

但通过远程访问地址仍无法访问Host Manager管理页面

默认无法通过网络远程访问Host Manager管理页面，默认会出现以下界面

```bash
[root@centos8 tomcat]#vim webapps/host-manager/META-INF/context.xml 
<Context antiResourceLocking="false" privileged="true" >
 <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|10\.0\.0\.\d+" />
 <Manager sessionAttributeValueClassNameFilter="java\.lang\.
(?:Boolean|Integer|Long|Number|String)|org\.apach
e\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.
(?:Linked)?HashMap"/>
</Context>
```

无需重启服务，直接访问，输入前面的用户和密码，即可登录成功

###### 3.4.5.5.3 创建新的虚拟主机

可以管理虚拟主机

```bash
#创建虚拟主机前,必须先创建相关目录,否则创建虚拟机不成功
[root@centos8 ~]#mkdir /data/node1/ROOT/
[root@centos8 ~]#echo node1.magedu.org > /data/node1/ROOT/index.html
[root@centos8 ~]#chown -R tomcat.tomcat /data/node1/
```

##### 3.4.5.5 Context 配置 

###### 3.4.5.5.1 Centext 配置方式

Context作用： 

* 路径映射：将url映射至指定路径，而非使用appBase下的物理目录，实现虚拟目录功能 
* 应用独立配置，例如单独配置应用日志、单独配置应用访问控制

```xml
#映射指定路径
<Context path="/test" docBase="/data/test" reloadable="true" />
#映射站点的根目录
<Context path="/" docBase="/data/website" reloadable="true" />
#还可以添加日志等独立的配置
<Context path="/test" docBase="/data/test" reloadable="true" >
  <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_test_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
</Context>
```

说明： 

* path：指的是访问的URL路径，如果path与appBase下面的子目录同名，context的docBase路径优先更高 
* docBase：可以是磁盘文件的绝对路径，也可以是相对路径（相对于Host的appBase） 
* reloadable：true表示如果WEB-INF/classes或META-INF/lib目录下.class文件有改动，就会将WEB 应用重新加载。生产环境中，建议使用false来禁用。

Centext实现过程 

* 将~/projects/myapp/下面的项目文件复制到/data/下，可以修改一下index.jsp 区别一下

```bash
# cp -r ~/projects/myapp /data/myapp-v1
# vim /data/myappv1/index.jsp 
# cd /data
# ln -sv myapp-v1 test
```

**注意：**这里特别使用了软链接，原因方便后期版升级或回滚，如是是版本升级，需要将软链接指向 myappv2，重新启动。如果新版上线后，出现问题，重新修改软链接到上一个版本的目录，并重启，就 可以实现回滚

* 修改conf/server.xml设置context

Tomcat的配置文件server.xml中修改如下，重启Tomcat生效

```xml
     <Host name="node1.magedu.com"  appBase="/data/webapps"
            unpackWARs="true" autoDeploy="true" >
        <Context path="/test" docBase="/data/test" reloadable="true" />
     </Host>
```

* 测试 

  使用http://node1.magedu.com:8080/test/

###### 3.4.5.5.2 Valve组件

valve(阀门)组件可以定义日志

```xml
<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
 	prefix="localhost_access_log" suffix=".txt"
 	pattern="%h %l %u %t &quot;%r&quot; %s %b" />
```

**valve存在多种类型：**

```
定义访问日志：org.apache.catalina.valves.AccessLogValve
定义访问控制：org.apache.catalina.valves.RemoteAddrValve 
```

示例:

```xml
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
deny="10\.0\.0\.\d+"/>
```

###### 3.4.5.5.3 实战案例

范例：虚拟主机上利用context实现虚拟目录

```bash
#在前面范例的基础上实现，继续创建node1.magedu.org虚拟主机下的物理子目录
[root@centos8 ~]#mkdir /data/webapps1/app1/
[root@centos8 ~]#echo /data/webapps1/app1/index.html > 
/data/webapps1/app1/index.html
[root@centos8 ~]#curl http://node1.magedu.org:8080/app1/
/data/webapps1/app1/index.html
#利用context实现node1.magedu.org虚拟主机下的虚拟子目录
[root@centos8 tomcat]#vim conf/server.xml
[root@centos8 tomcat]#tail conf/server.xml
     </Host>
     <Host  name="node1.magedu.org"  appBase="/data/webapps1">
      #加下面六行
     <Context path="/app1" docBase="/data/app1" reloadable="true" >
     <Valve className="org.apache.catalina.valves.AccessLogValve"
directory="logs"
               prefix="node1.magedu.org_app1" suffix=".log"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
       <Valve className="org.apache.catalina.valves.RemoteAddrValve"
deny="10\.0\.0\.7"/>
       </Context>
      
     </Host>
     <Host  name="node2.magedu.org"  appBase="/data/webapps2">
     </Host>
   </Engine>
 </Service>
</Server>
[root@centos8 tomcat]#systemctl restart tomcat
#因数据没有准备好，出现下面错误
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
curl: (7) Failed to connect to node1.magedu.org port 8080: Connection refused
#准备数据目录
[root@centos8 tomcat]#mkdir /data/app1-v1
[root@centos8 tomcat]#echo /data/app1-v1/index.html > /data/app1-v1/index.html
[root@centos8 tomcat]#ln -s /data/app1-v1/ /data/app1
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
curl: (7) Failed to connect to node1.magedu.org port 8080: Connection refused

#数据目录准备好，还需要重新启动服务，才能访问
[root@centos8 tomcat]#systemctl restart tomcat
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
/data/app1-v1/index.html
[root@centos7 ~]#curl -I http://node1.magedu.org:8080/app1/
HTTP/1.1 403
Content-Type: text/html;charset=utf-8
Content-Language: en
Transfer-Encoding: chunked
Date: Tue, 14 Jul 2020 06:48:54 GMT
#可以看到此目录单独的访问日志
[root@centos8 ~]#cat /usr/local/tomcat/logs/node1.magedu.org_app1.2020-07-14.log
10.0.0.8 - - [14/Jul/2020:14:36:01 +0800] "GET /app1/ HTTP/1.1" 200 330
10.0.0.7 - - [14/Jul/2020:14:48:07 +0800] "GET /app1/ HTTP/1.1" 403 618
```

范例: 基于前面环境,实现软件升级和回滚功能

```bash
#升级版本
[root@centos8 tomcat]#mkdir /data/app1-v2
[root@centos8 tomcat]#echo /data/app1-v2/index.html > /data/app1-v2/index.html
[root@centos8 tomcat]#rm -f /data/app1
#删除软链接，仍然可以访问旧版本
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
/data/app1-v1/index.html
#重新服务后，出现错误
[root@centos8 tomcat]#systemctl restart tomcat
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
curl: (7) Failed to connect to node1.magedu.org port 8080: Connection refused
#新建软链接，指向新版，仍需重启服务才生效
[root@centos8 tomcat]#ln -s /data/app1-v2/ /data/app1
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
curl: (7) Failed to connect to node1.magedu.org port 8080: Connection refused
[root@centos8 tomcat]#systemctl restart tomcat
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
/data/app1-v2/index.html
#软件降级或回滚
[root@centos8 tomcat]#rm -f /data/app1
[root@centos8 tomcat]#ln -s /data/app1-v1/ /data/app1
[root@centos8 tomcat]#systemctl restart tomcat
[root@centos8 tomcat]#curl http://node1.magedu.org:8080/app1/
/data/app1-v1/index.html
```

## 4 结合反向代理实现tomcat部署

### 4.1 常见部署方式介绍

* standalone模式，Tomcat单独运行，直接接受用户的请求，不推荐。 
* 反向代理，单机运行，提供了一个Nginx作为反向代理，可以做到静态由nginx提供响应，动态jsp 代理给Tomcat 
  * LNMT：Linux + Nginx + MySQL + Tomcat 
  * LAMT：Linux + Apache（Httpd）+ MySQL + Tomcat 
* 前置一台Nginx，给多台Tomcat实例做反向代理和负载均衡调度，Tomcat上部署的纯动态页面更适合 
  * LNMT：Linux + Nginx + MySQL + Tomcat 
* 多级代理 
  * LNNMT：Linux + Nginx + Nginx + MySQL + Tomcat

### 4.2 利用 nginx 反向代理实现全部转发置指定同一个虚拟主 机

#### 4.2.1 配置说明

利用nginx反向代理功能，实现上图的代理功能，将用户请求全部转发至指定的同一个tomcat主机

利用nginx指令proxy_pass 可以向后端服务器转发请求报文,并且在转发时会保留客户端的请求报文中的 host首部

```bash
#从yum源安装nginx
#yum install nginx -y
#vim /etc/nginx/nginx.conf
#全部反向代理测试
location / {
    # proxy_pass http://127.0.0.1:8080; # 不管什么请求，都会访问后面的localhost虚拟主机
    proxy_pass http://node1.magedu.com:8080; # 此项将用户访问全部请求转发到node1的虚拟主机上
    #proxy_pass http://node2.magedu.com:8080; #此项将用户访问全部请求转发到node2的虚拟主机上
    
    #以上两项都需要修改nginx服务器的/etc/hosts,实现node1.magedu.com和node2.magedu.com到IP的解析    
}
#nginx -t
#systemctl restart nginx
#说明: proxy_pass http://FQDN/ 中的FQDN 决定转发至后端哪个虚拟主机,而与用户请求的URL无关
#如果转到后端的哪个服务器由用户请求决定,可以向后端服务转发请求的主机头实现,示例: proxy_set_header Host $http_host; 
```

#### 4.2.2 实战案例

环境说明:

```http
一台主机,实现nginx和tomcat
tomcat上有两个虚拟主机node1和node2
```

```bash
#先按3.4.5.4介绍方式在同一下主机上建立两个tomcat虚拟主机，node1.magedu.org和
node2.magedu.org
#修改/etc/hosts文件，实现名称解析
[root@centos8 ~]#vim /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 centos8.localdomain
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6 10.0.0.100 node1.magedu.org node2.magedu.org
#安装nginx
[root@centos8 ~]#yum -y install nginx
#修改nginx.conf配置文件
[root@centos8 ~]#vim /etc/nginx/nginx.conf
......
#修改location / 此行，添加以下内容
location / {
           #proxy_pass http://127.0.0.1:8080;
           proxy_pass http://node1.magedu.org:8080;                               
        }
......
[root@centos8 ~]#systemctl enable --now nginx
#先别访问node1,node2和IP都可以看到一样的node1的虚拟主机页面
[root@centos8 ~]#curl http://node1.magedu.org/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node1.magedu.org
</body>
</html>
[root@centos8 ~]#curl http://node2.magedu.org/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node2.magedu.org
</body>
</html>
[root@centos8 ~]#curl http://127.0.0.1/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node1.magedu.org
</body>
</html>
[root@centos8 ~]#curl http://10.0.0.100/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node1.magedu.org
</body>
</html>
[root@centos8 ~]#systemctl restart nginx
#再次修改nginx.conf配置文件
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node1.magedu.org
</body>
</html>
[root@centos8 ~]#curl http://node2.magedu.org/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node2.magedu.org
</body>
</html>
[root@centos8 ~]#curl http://127.0.0.1/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node1.magedu.org
</body>
</html>
[root@centos8 ~]#curl http://10.0.0.100/
<!DOCTYPE html>
<html>
<head>
   <meta charset="utf-8">
   <title>jsp例子</title>
</head>
<body>
后面的内容是服务器端动态生成字符串，最后拼接在一起
node1.magedu.org
</body>
</html>
[root@centos8 ~]#systemctl restart nginx
#再次修改nginx.conf配置文件
```

### 4.3 利用nginx实现动静分离代理

#### 4.3.1 配置说明

可以利用nginx实现动静分离

```bash
vim nginx.conf
root /usr/share/nginx/html;
#下面行可不加
#location / {
#   root /data/webapps/ROOT;
#   index index.html;
#}
# ~* 不区分大小写
location ~* \.jsp$ {
    proxy_pass http://node1.magedu.com:8080; #注意: 8080后不要加/,需要在nginx服务器修
改 /etc/hosts
}
```

以上设置，可以将jsp的请求反向代理到tomcat，而其它文件仍由nginx处理，从而实现所谓动静分离。 但由于jsp文件中实际上是由静态资源和动态组成，所以无法彻底实现动静分离。实际上Tomcat不太适 合做动静分离，用它来管理程序的图片不好做动静分离部署

#### 4.3.2 实战案例

```bash
#准备三个不同的资源文件
[root@centos8 ~]#echo /usr/local/tomcat/webapps/ROOT/test.html > 
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#echo /usr/local/tomcat/webapps/ROOT/test.jsp > 
/usr/local/tomcat/webapps/ROOT/test.jsp
[root@centos8 ~]#echo /usr/share/nginx/html/test.html > 
/usr/share/nginx/html/test.html
[root@centos8 ~]#vim /etc/nginx/nginx.conf
......
root         /usr/share/nginx/html;
#location / {
#
#}
location ~* \.jsp$ {
   proxy_pass http://127.0.0.1:8080;                                             
        }
......
[root@centos8 ~]#systemctl restart nginx
#访问test.html，观察结果都一样访问的是nginx自身的资源
[root@centos8 ~]#curl http://node1.magedu.org/test.html
[root@centos8 ~]#curl http://node2.magedu.org/test.html
[root@centos8 ~]#curl http://127.0.0.1/test.html
[root@centos8 ~]#curl http://10.0.0.8/test.html
/usr/share/nginx/html/test.html
#访问test.jsp，观察结果都一样访问的是tomcat的默认主机资源
[root@centos8 ~]#curl http://node1.magedu.org/test.jsp
[root@centos8 ~]#curl http://node2.magedu.org/test.jsp
[root@centos8 ~]#curl http://127.0.0.1/test.jsp
[root@centos8 ~]#curl http://10.0.0.8/test.jsp
/usr/local/tomcat/webapps/ROOT/test.jsp
```

### 4.4 利用httpd实现基于http协议的反向代理至后端 Tomcat服务器

#### 4.4.1 配置说明

httpd也提供了反向代理功能，所以可以实现对tomcat的反向代理功能

范例：查看代理相关模块

```bash
[root@centos8 ~]#httpd -M|grep proxy
AH00558: httpd: Could not reliably determine the server's fully qualified domain 
name, using centos8.localdomain. Set the 'ServerName' directive globally to 
suppress this message
 proxy_module (shared)
 proxy_ajp_module (shared)      #ajp
 proxy_balancer_module (shared)
 proxy_connect_module (shared)
 proxy_express_module (shared)
 proxy_fcgi_module (shared)
 proxy_fdpass_module (shared)
 proxy_ftp_module (shared) 
 proxy_http_module (shared)    #http
 proxy_hcheck_module (shared)
 proxy_scgi_module (shared)
 proxy_uwsgi_module (shared)
 proxy_wstunnel_module (shared)
 proxy_http2_module (shared)
```

**proxy_http_module模块代理配置**

```xml
vim /etc/httpd/conf.d/http-tomcat.conf
<VirtualHost *:80>
   ServerName       node1.magedu.org
   ProxyRequests     Off
   ProxyPass   / http://127.0.0.1:8080/
   ProxyPassReverse / http://127.0.0.1:8080/
   ProxyPreserveHost On
   ProxyVia         On
</VirtualHost>
```

* ProxyRequests：Off 关闭正向代理功能,即启动反向代理 
* ProxyPass：反向代理指令,指向后端服务器 
* ProxyPassReverse：当反向代理时,返回给客户端的报文需将之重写个别后端主机的response头, 如:Location,Content-Location,URI 
* ProxyPreserveHost：On时让反向代理保留原请求的Host首部转发给后端服务器，off 时则删除 host首部后再转发至后面端服务器, 这将导致只能转发到后端的默认虚拟主机 
* ProxyVia：On开启。反向代理的响应报文中提供一个response的via首部，默认值off

**说明: 关于ProxyPreserveHost**

```bash
#分别访问下面不同链接
http://httpd服务IP/
http://node1.magedu.org/
http://node1.magedu.org/index.jsp
#以上3个URL看到了不同的页面，说明ProxyPreserveHost On起了作用
#设置ProxyPreserveHost Off再看效果，说明什么？
```

#### 4.4.2 实战案例

范例：基于httpd实现反向代理后端的tomcat

```bash
#对不同的虚拟主机生成页面文件
[root@centos8 ~]#echo /usr/local/tomcat/webapps/ROOT/test.html > 
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#echo /data/node1/ROOT/test.html > /data/node1/ROOT/test.html
[root@centos8 ~]#echo /data/node2/ROOT/test.html > /data/node2/ROOT/test.html
#提前准备好tomcat的两个虚拟主机的配置
[root@centos8 ~]#tail /usr/local/tomcat/conf/server.xml
     </Host>
       <Host name="node1.magedu.org"  appBase="/data/node1/" unpackWARs="true"
autoDeploy="true">
       </Host>
       <Host name="node2.magedu.org"  appBase="/data/node2/" unpackWARs="true"
autoDeploy="true">
       </Host>
   </Engine>
 </Service>
</Server>
#修改httpd配置
[root@centos8 ~]#yum -y install httpd
[root@centos8 ~]#vim /etc/httpd/conf.d/tomcat.conf 
[root@centos8 ~]#cat /etc/httpd/conf.d/tomcat.conf 
<VirtualHost *:80>
   ServerName       node1.magedu.org
   ProxyRequests     Off
   ProxyVia         On
   ProxyPreserveHost On
   ProxyPass / http://127.0.0.1:8080/
   ProxyPassReverse   / http://127.0.0.1:8080/
</VirtualHost>
[root@centos8 ~]#systemctl restart httpd
#用下面不同URL访问，可以看不同结果
[root@centos8 ~]#curl http://node1.magedu.org/test.html
/data/node1/ROOT/test.html
[root@centos8 ~]#curl http://node2.magedu.org/test.html
/data/node2/ROOT/test.html
[root@centos8 ~]#curl http://127.0.0.1/test.html
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#curl http://10.0.0.8/test.html
/usr/local/tomcat/webapps/ROOT/test.html
#修改配置
[root@centos8 ~]#vim /etc/httpd/conf.d/tomcat.conf
#只修改下面一行
ProxyPreserveHost Off                                                             
     
[root@centos8 ~]#systemctl restart httpd
              
#再次用用下面不同URL访问，可以看相同结果
[root@centos8 ~]#curl http://node1.magedu.org/test.html
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#curl http://node2.magedu.org/test.html
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#curl http://10.0.0.8/test.html
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#curl http://127.0.0.1/test.html
/usr/local/tomcat/webapps/ROOT/test.html
```

### 4.5 利用 httpd 实现基于AJP协议的反向代理至后端 Tomcat服务器

#### 4.5.1 AJP 协议说明 

AJP（Apache JServ Protocol）是定向包协议，是一个二进制的TCP传输协议，相比HTTP这种纯文本的 协议来说，效率和性能更高，也做了很多优化。但是浏览器并不能直接支持AJP13协议，只支持HTTP协 议。所以实际情况是，通过Apache的proxy_ajp模块进行反向代理，暴露成http协议给客户端访问

#### 4.5.2 启用和禁用 AJP

注意: Tomcat/8.5.51之后版本基于安全需求默认禁用AJP协议 

范例: Tomcat/8.5.51之后版启用支持AJP协议

```bash
[root@centos8 tomcat]#vim conf/server.xml
#取消前面的注释,并修改下面行,修改address和secretRequired
<Connector protocol="AJP/1.3"  address="0.0.0.0" port="8009"
redirectPort="8443" secretRequired="" />
[root@centos8 tomcat]#systemctl restart tomcat
[root@centos8 tomcat]#ss -ntl
State         Recv-Q       Send-Q                   Local Address:Port         Peer Address:Port     
LISTEN        0             128                             0.0.0.0:22           0.0.0.0:*       
LISTEN        0             100                           127.0.0.1:25           0.0.0.0:*       
LISTEN        0             128                               [::]:22           [::]:*       
LISTEN        0             100                               [::1]:25           [::]:*       
LISTEN        0             1                   [::ffff:127.0.0.1]:8005         *:*       
LISTEN        0             100                 [::ffff:127.0.0.1]:8009         *:*       
LISTEN        0             100                                   *:8080         *:*       
LISTEN        0             128                                   *:80           *:* 
```

注意: secretRequired="" 必须加上,否则出现以下错误提示

```bash
[root@centos8 tomcat]#cat logs/catalina.log 
Caused by: java.lang.IllegalArgumentException: The AJP Connector is configured with secretRequired="true" but the secret attribute is either null or "". This combination is not valid.
```

除httpd外，其它支持AJP代理的服务器非常少，比如Nginx就不支持AJP，所以目前一般都禁用AJP协议端口

范例：禁用AJP协议

```
#Tomcat/8.5.50版本之前默认支持AJP协议
[root@centos8 ~]#ss -ntl
State       Recv-Q       Send-Q                     Local Address:Port           Peer Address:Port   
LISTEN      0             128                              0.0.0.0:22           0.0.0.0:*         
LISTEN      0             100                                   *:8080         *:*         
LISTEN      0             128                                   *:80           *:*         
LISTEN      0             128                                 [::]:22           [::]:*      
LISTEN      0             1                     [::ffff:127.0.0.1]:8005         *:*         
LISTEN      0             100                                   *:8009         *:*
#配置tomcat配置文件，删除下面一行
[root@centos8 ~]#vim /usr/local/tomcat/conf/server.xml 
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />    
[root@centos8 ~]#systemctl restart tomcat
[root@centos8 ~]#ss -ntl
State       Recv-Q       Send-Q                     Local Address:Port           Peer Address:Port   
LISTEN      0             128                              0.0.0.0:22           0.0.0.0:*         
LISTEN      0             100                                   *:8080         *:*         
LISTEN      0             128                                   *:80           *:*         
LISTEN      0             128                                 [::]:22           [::]:*         
LISTEN      0             1                     [::ffff:127.0.0.1]:8005         *:*               
```

#### 4.5.3 httpd 实现 AJP 反向代理

##### 4.5.3.1 配置说明

相对来讲，AJP协议基于二进制比使用HTTP协议的连接器效率高些。 

proxy_ajp_module模块代理配置

```xml
<VirtualHost *:80>
   ServerName       node1.magedu.org
   ProxyRequests     Off
   ProxyVia         On
   ProxyPreserveHost On
   ProxyPass       / ajp://127.0.0.1:8009/
</VirtualHost>
```

查看Server Status可以看到确实使用的是ajp连接了。

##### 4.5.3.2 实战案例

范例：启用httpd的AJP反向代理功能

```bash
[root@centos8 ~]#vim /etc/httpd/conf.d/tomcat.conf 
[root@centos8 ~]#cat /etc/httpd/conf.d/tomcat.conf
<VirtualHost *:80>
   ServerName       node1.magedu.org
   ProxyRequests     Off
   ProxyVia         On   #此项对AJP无效
   ProxyPreserveHost On   #此项对AJP无效
   ProxyPass       / ajp://127.0.0.1:8009/
</VirtualHost>
[root@centos8 ~]#systemctl restart httpd
#再次用用下面不同URL访问，可以看以下结果
[root@centos8 ~]#curl http://node1.magedu.org/test.html
/data/node1/ROOT/test.html
[root@centos8 ~]#curl http://node2.magedu.org/test.html
/data/node2/ROOT/test.html
[root@centos8 ~]#curl http://10.0.0.8/test.html
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#curl http://127.0.0.1/test.html
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#vim /etc/httpd/conf.d/tomcat.conf 
#只修改下面一行,关闭向后端转发请求的host首部
 ProxyPreserveHost Off  
#再次用用下面不同URL访问，可以看到和上面一样的结果，说明AJP协议和Http不同，自动转发所有首部信息
[root@centos8 ~]#curl http://node1.magedu.org/test.html
/data/node1/ROOT/test.html
[root@centos8 ~]#curl http://node2.magedu.org/test.html
/data/node2/ROOT/test.html
[root@centos8 ~]#curl http://10.0.0.8/test.html
/usr/local/tomcat/webapps/ROOT/test.html
[root@centos8 ~]#curl http://127.0.0.1/test.html
/usr/local/tomcat/webapps/ROOT/test.html
```

可以通过status页面看到下面AJP的信息

```bash
#用iptables禁用AJP的访问
[root@centos8 ~]#iptables -A INPUT -p tcp --dport 8009 -j REJECT
[root@centos8 ~]#curl http://node1.magedu.org/test.html
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>503 Service Unavailable</title>
</head><body>
<h1>Service Unavailable</h1>
<p>The server is temporarily unable to service your
request due to maintenance downtime or capacity
problems. Please try again later.</p>
</body></html>
```

### 4.6 实现 tomcat 负载均衡

动态服务器的问题，往往就是并发能力太弱，往往需要多台动态服务器一起提供服务。如何把并发的压 力分摊，这就需要调度，采用一定的调度策略，将请求分发给不同的服务器，这就是Load Balance负载 均衡。 

当单机Tomcat，演化出多机多级部署的时候，一个问题便凸显出来，这就是Session。而这个问题的由 来，都是由于HTTP协议在设计之初没有想到未来的发展。

#### 4.6.1 HTTP的无状态，有连接和短连接

* 无状态：指的是服务器端无法知道2次请求之间的联系，即使是前后2次请求来自同一个浏览器，也 没有任何数据能够判断出是同一个浏览器的请求。后来可以通过cookie、session机制来判断。 
  * 浏览器端第一次HTTP请求服务器端时，在服务器端使用session这种技术，就可以在服务器端 产生一个随机值即SessionID发给浏览器端，浏览器端收到后会保持这个SessionID在Cookie 当中，这个Cookie值一般不能持久存储，浏览器关闭就消失。浏览器在每一次提交HTTP请求 的时候会把这个SessionID传给服务器端，服务器端就可以通过比对知道是谁了 
  * Session通常会保存在服务器端内存中，如果没有持久化，则易丢失 
  * Session会定时过期。过期后浏览器如果再访问，服务端发现没有此ID，将给浏览器端重新发新的SessionID 
  * 更换浏览器也将重新获得新的SessionID 
* 有连接：是因为它基于TCP协议，是面向连接的，需要3次握手、4次断开。 
* 短连接：Http 1.1之前，都是一个请求一个连接，而Tcp的连接创建销毁成本高，对服务器有很大 的影响。所以，自Http 1.1开始，支持keep-alive，默认也开启，一个连接打开后，会保持一段时 间（可设置），浏览器再访问该服务器就使用这个Tcp连接，减轻了服务器压力，提高了效率。

服务器端如果故障，即使Session被持久化了，但是服务没有恢复前都不能使用这些SessionID。 

如果使用HAProxy或者Nginx等做负载均衡器，调度到了不同的Tomcat上，那么也会出现找不到 SessionID的情况。

#### 4.6.2 会话保持方式

##### 4.6.2.1 session sticky会话黏性

Session绑定 

* nginx：source ip, cookie 
* HAProxy：source ip, cookie

优点：简单易配置 

缺点：如果目标服务器故障后，如果没有做sessoin持久化，就会丢失session,此方式生产很少使用

##### 4.6.2.2 Session 复制集群

Tomcat自己的提供的多播集群，通过多播将任何一台的session同步到其它节点。 

缺点 

* Tomcat的同步节点不宜过多，互相即时通信同步session需要太多带宽 
* 每一台都拥有全部session，内存损耗太多

##### 4.6.2.3 Session Server 

session 共享服务器，使用memcached、redis做共享的Session服务器，此为推荐方式

#### 4.6.3 负载均衡规划和准备

##### 4.6.3.1 负载均衡主机和网络地址规划

| IP         | 主机名           | 服务    | 软件          |
| ---------- | ---------------- | ------- | ------------- |
| 10.0.0.100 | proxy.magedu.org | 调度器  | Nginx、Httpd  |
| 10.0.0.101 | t1.magedu.org    | Tomcat1 | jdk8、Tomcat8 |
| 10.0.0.102 | t2.magedu.org    | Tomcat2 | jdk8、Tomcat8 |

```bash
#只需在10.0.0.100的nginx主机上实现域名解析
vim /etc/hosts 
#添加以下三行
10.0.0.100 proxy.magedu.org proxy
10.0.0.101 t1.magedu.org t1
10.0.0.102 t2.magedu.org t2
```

##### 4.6.3.2 负载均衡tomcat主机准备

修改tomcat的虚拟机主机为自定义的主机名,并设为默认的虚拟主机 

t1虚拟主机配置conf/server.xml

```xml
<Engine name="Catalina" defaultHost="t1.magedu.org">
    <Host name="t1.magedu.org" appBase="/data/webapps" autoDeploy="true" >
    </Host>
</Engine>
```

t2虚拟主机配置conf/server.xml

```xml
<Engine name="Catalina" defaultHost="t2.magedu.org">
    <Host name="t2.magedu.org" appBase="/data/webapps" autoDeploy="true" >
    </Host>
</Engine>
```

##### 4.6.3.3 准备负载均衡规划测试用的jsp文件

在t1和 t2节点创建相同的文件/data/webapps/ROOT/index.jsp

```bash
#项目路径配置
mkdir -pv /data/webapps/ROOT

#编写测试jsp文件，内容在下面
vim /data/webapps/ROOT/index.jsp
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<div>On <%=request.getServerName() %></div>
<div><%=request.getLocalAddr() + ":" + request.getLocalPort() %></div>
<div>SessionID = <span style="color:blue"><%=session.getId() %></span></div>
<%=new Date()%>
</body>
</html>

#设置权限
chown -R tomcat.tomcat /data/webapps/
```

#### 4.6.4 Nginx 实现后端 tomcat 的负载均衡调度

##### 4.6.4.1 Nginx 实现后端 tomcat 的负载均衡

nginx 配置如下

```bash
vim /etc/nginx/nginx.conf 
#在http块中加以下内容
#注意名称不要用下划线
upstream tomcat-server {
        #ip_hash; # 先禁用看看轮询，之后开启开黏性
        #hash $cookie_JSESSIONID; # 先禁用看看轮询，之后开启开黏性
        server t1.magedu.org:8080;
        server t2.magedu.org:8080;
}
    
server {
        location ~* \.(jsp|do)$ {
       proxy_pass http://tomcat-server;
       }
}
```

测试 http://proxy.magedu.com/index.jsp，可以看到轮询调度效果,每次刷新后端主机和SessionID都会变化

```xml
[root@proxy ~]#curl http://proxy.magedu.org/index.jsp
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On tomcat-server</div>
<div>10.0.0.101:8080</div>
<div>SessionID = <span 
style="color:blue">2E4BFA5135497EA3628F1EBDAE62493E</span></div>
Thu Jul 09 17:58:06 CST 2020
</body>
</html>
[root@proxy ~]#curl http://proxy.magedu.org/index.jsp
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On tomcat-server</div>
<div>10.0.0.102:8080</div>
<div>SessionID = <span 
style="color:blue">C5CC437BC05EE5A8620822CB07E71B7C</span></div>
Thu Jul 09 17:58:07 CST 2020
</body>
</html>
```

使用抓包wireshark工具可以看到下面信息

##### 4.6.4.2 实现 session 黏性

在upstream中使用ip_hash指令，使用客户端IP地址Hash。

```bash
[root@proxy ~]#vim /etc/nginx/nginx.conf
#只添加ip_hash;这一行
upstream tomcat-server {
       ip_hash; #启动源地址hash  
        #hash $cookie_JSESSIONID   #启动基于cookie的hash
       server t1.magedu.org:8080;
       server t2.magedu.org:8080;
}
```

配置完reload nginx服务。curl 测试一下看看效果。

```bash
#用curl访问每次都调度到10.0.0.102主机上，但因为curl每次请求不会自动携带之前获取的cookie,所有SessionID每次都在变化
[root@proxy ~]#curl http://proxy.magedu.org/index.jsp
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On tomcat-server</div>
<div>10.0.0.102:8080</div>
<div>SessionID = <span 
style="color:blue">C471641C26865B08B2FDA970BE7C71A6</span></div>
Thu Jul 09 18:02:48 CST 2020
</body>
</html>
[root@proxy ~]#curl http://proxy.magedu.org/index.jsp
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On tomcat-server</div>
<div>10.0.0.102:8080</div>
<div>SessionID = <span 
style="color:blue">3F61232DFD791A94D60D0D2E9561309A</span></div>
Thu Jul 09 18:02:52 CST 2020
</body>
</html>
```

通过图形浏览器看到主机不变，sessionID不变

#### 4.6.5 Httpd 实现后端tomcat的负载均衡调度

和nginx一样, httpd 也支持负载均衡调度功能

##### 4.6.5.1 httpd 的负载均衡配置说明

使用 httpd -M 可以看到 proxy_balancer_module，用它来实现负载均衡。 

官方帮助: http://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html

| 方式         | 依赖模块                                      |
| ------------ | --------------------------------------------- |
| http负载均衡 | mod_proxy、mod_proxy_http、mod_proxy_balancer |
| ajp负载均衡  | mod_proxy、mod_proxy_ajp、mod_proxy_balancer  |

**负载均衡配置说明**

```bash
#配置代理到balancer
ProxyPass [path] !|url [key=value [key=value ...]]
#Balancer成员
BalancerMember [balancerurl] url [key=value [key=value ...]]
#设置Balancer或参数
ProxySet url key=value [key=value ...]
```

ProxyPass 和 BalancerMember 指令参数

| 参数  | 缺省值 | 说明                                                         |
| ----- | ------ | ------------------------------------------------------------ |
| min   | 0      | 连接池最小容量                                               |
| max   | 1-n    | 连接池最大容量                                               |
| retry | 60     | Apache请求发送到后端服务器错误后等待的时间秒数。0表示立即重试 |

Balancer 参数

|     参数      |   缺省值   |                             说明                             |
| :-----------: | :--------: | :----------------------------------------------------------: |
|  loadfactor   |            |        定义负载均衡后端服务器权重，取值范围: 1 - 100         |
|   lbmethod    | byrequests | 负载均衡调度方法。 byrequests 基于权重的统计请求个数进行调度、bytrafficz 执行基于权重的流量计数调度、bybusyness 通过考量每个后端服务器当前负载进行调度 |
|  maxattempts  |     1      | 放弃接受请求前,实现故障转移的次数，默认为1，其最大值不应该大于总的节点数 |
|  nofailover   |    off     | On 表示不允许故障转移, 如果后端服务器没有Session副本可以 设置为On、Off 表示故障可以转移 |
| stickysession |            | 调度器的sticky session名字，根据web后台编程语言不同，可以设置为JSESSIONID或PHPSESSIONID |

ProxySet指令也可以使用上面的参数。

##### 4.6.5.2 启用 httpd 的负载均衡

在 tomcat 的配置中Engine使用jvmRoute属性,通过此项可得知SessionID是在哪个tomcat生成

```xml
#t1的conf/server.xml配置如下：
<Engine name="Catalina" defaultHost="t1.magedu.org" jvmRoute="Tomcat1">
<Host name="t1.magedu.org" appBase="/data/webapps" autoDeploy="true">           
        
</Host>
    
#t2的conf/server.xml配置如下：
<Engine name="Catalina" defaultHost="t2.magedu.org" jvmRoute="Tomcat2">
<Host name="t2.magedu.org" appBase="/data/webapps" autoDeploy="true">           
      
</Host>
```

这样设置后 SessionID 就变成了以下形式： 

SessionID = 9C949FA4AFCBE9337F5F0669548BD4DF.Tomcat1 

httpd配置如下

```bash
[root@proxy ~]#vim /etc/httpd/conf.d/tomcat.conf 
[root@proxy ~]#cat /etc/httpd/conf.d/tomcat.conf
<Proxy balancer://tomcat-server>
   BalancerMember http://t1.magedu.org:8080 loadfactor=1
   BalancerMember http://t2.magedu.org:8080 loadfactor=2
</Proxy>
<VirtualHost *:80>
   ServerName       proxy.magedu.org
   ProxyRequests     Off
   ProxyVia         On
   ProxyPreserveHost On  #off时不向后端转发原请求host首部，而转发采用BalancerMember指向名称为首部
   ProxyPass       / balancer://tomcat-server/
   ProxyPassReverse / balancer://tomcat-server/
</VirtualHost>
#开启httpd负载均衡的状态页
<Location /balancer-manager>
 SetHandler balancer-manager
 ProxyPass !
 Require all granted
</Location>
```

loadfactor设置为1:2，便于观察。观察调度的结果是轮询的。

使用抓包wireshark工具可以看到下面信息

##### 4.6.5.3 实现 session 黏性

官方文档：http://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html

```
%{BALANCER_WORKER_ROUTE}e   The route of the worker chosen.
```

范例: 

```bash
[root@proxy ~]#vim /etc/httpd/conf.d/tomcat.conf 
#添加此行,在cookie 添加 ROUTEID的定义
Header add Set-Cookie "ROUTEID=.%{BALANCER_WORKER_ROUTE}e; path=/"
env=BALANCER_ROUTE_CHANGED  
<Proxy balancer://tomcat-server>
   BalancerMember http://t1.magedu.org:8080 loadfactor=1 route=T1 #修改行,指定后端服务器对应的ROUTEID
   BalancerMember http://t2.magedu.org:8080 loadfactor=1 route=T2 #修改行
   ProxySet stickysession=ROUTEID    #添加此行,指定用cookie中的ROUTEID值做为调度条件
</Proxy>
<VirtualHost *:80>
   ServerName       proxy.magedu.org
   ProxyRequests     Off
   ProxyVia         On
   ProxyPreserveHost On
   ProxyPass       / balancer://tomcat-server/
   ProxyPassReverse / balancer://tomcat-server/
</VirtualHost>
#开启httpd负载均衡的状态页
<Location /balancer-manager>
 SetHandler balancer-manager
 ProxyPass !
 Require all granted
</Location>
```

用浏览器访问发现Session不变了，一直找的同一个Tomcat服务器

```bash
#观察结果
[root@proxy ~]#curl http://proxy.magedu.org/index.jsp #轮询
[root@proxy ~]#curl -b "ROUTEID=.T1" http://proxy.magedu.org/index.jsp #固定调度到t1
[root@proxy ~]#curl -b "ROUTEID=.T2" http://proxy.magedu.org/index.jsp
[root@centos7 ~]#curl http://proxy.magedu.org/index.jsp -I
HTTP/1.1 200
Date: Wed, 15 Jul 2020 00:55:05 GMT
Server: Apache/2.4.37 (centos)
Content-Type: text/html;charset=ISO-8859-1
Transfer-Encoding: chunked
Set-Cookie: JSESSIONID=214D58236E1BD3095814B86A65C46C8A.Tomcat1; Path=/; 
HttpOnly
Via: 1.1 proxy.magedu.org
Set-Cookie: ROUTEID=.T1; path=/
[root@centos7 ~]#curl -b "JSESSIONID=214D58236E1BD3095814B86A65C46C8A.Tomcat1; ROUTEID=.T1" http://proxy.magedu.org/index.jsp
```

关闭tomcat2的tomcat服务，再查看，转向另一台tomcat服务器

```bash
[root@t2 ~]#systemctl stop tomcat
```

重新启动 tomcat2 的tomcat服务，再查看保持不变

范例:

```bash
[root@client ~]#curl -I www.magedu.org
HTTP/1.1 200
Date: Sun, 10 Jan 2021 09:58:49 GMT
Server: Apache/2.4.37 (centos)
Content-Type: text/html;charset=ISO-8859-1
Transfer-Encoding: chunked
Set-Cookie: JSESSIONID=9E1A624ABD2A0ABBD256EFB14B12D555.Tomcat2; Path=/; 
HttpOnly
Via: 1.1 proxy.magedu.org
Set-Cookie: ROUTEID=.T2; path=/
[root@client ~]#curl -b "ROUTEID=.T1" www.magedu.org
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
       <title>tomcat test</title>
       </head>
       <body>
       <h1>t1.magedu.org</h1>
       <div>On www.magedu.org</div>
       <div>10.0.0.101:8080</div>
       <div>SessionID = <span 
style="color:blue">A13BFA537739734CFDC12D59EA56710A.Tomcat1</span></div>
       Sun Jan 10 18:01:33 CST 2021
       </body>
       </html>
```

##### 4.6.5.4 实现 AJP 协议的负载均衡

在上面基础上修改httpd的配置文件

```bash
#在t1和t2的tomcat-8.5.51以上版本的需启用AJP
[root@centos8 tomcat]#vim conf/server.xml
#取消前面的注释,并修改下面行
<Connector protocol="AJP/1.3"  address="0.0.0.0" port="8009"
redirectPort="8443" secretRequired="" />
[root@proxy ~]#cat /etc/httpd/conf.d/tomcat.conf 
#Header add Set-Cookie "ROUTEID=.%{BALANCER_WORKER_ROUTE}e; path=/" 
env=BALANCER_ROUTE_CHANGED #注释此行
<Proxy balancer://tomcat-server>
   BalancerMember ajp://t1.magedu.org:8009 loadfactor=1 route=T1   #修改此行
   BalancerMember ajp://t2.magedu.org:8009 loadfactor=1 route=T2   #修改此行
    #ProxySet stickysession=ROUTEID   #先注释此行
</Proxy>
<VirtualHost *:80>
   ServerName       proxy.magedu.org
   ProxyRequests     Off
   ProxyVia         On
   ProxyPreserveHost On
   ProxyPass       / balancer://tomcat-server/
   ProxyPassReverse / balancer://tomcat-server/
</VirtualHost>
<Location /balancer-manager>
   SetHandler balancer-manager
   ProxyPass !
   Require all granted
</Location>       
#ProxySet stickysession=ROUTEID先禁用,可以看到不断轮询的切换效果
```

开启ProxySet后，发现Session不变了，一直找的同一个Tomcat服务器。

```bash
[root@proxy ~]#vim /etc/httpd/conf.d/tomcat.conf 
<Proxy balancer://tomcat-server>
   BalancerMember ajp://t1.magedu.org:8009 loadfactor=1 route=T1   
   BalancerMember ajp://t2.magedu.org:8009 loadfactor=2 route=T2   
   ProxySet stickysession=ROUTEID    #取消此行注释,只修改此行
</Proxy>
                                                          
[root@proxy ~]#systemctl restart httpd
```

## 5 Tomcat Session Replication Cluster

Tomcat 官方实现了 Session 的复制集群,将每个Tomcat的Session进行相互的复制同步,从而保证所有 Tomcat都有相同的Session信息.

### 5.1 配置说明

官方文档：

```http
https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html
https://tomcat.apache.org/tomcat-8.5-doc/cluster-howto.html
```

说明

```xml
<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster" channelSendOptions="8">
 <Manager className="org.apache.catalina.ha.session.DeltaManager"
   expireSessionsOnShutdown="false"
   notifyListenersOnReplication="true"/>
 <Channel className="org.apache.catalina.tribes.group.GroupChannel">
 <Membership className="org.apache.catalina.tribes.membership.McastService"
 address="228.0.0.4"         #指定的多播地址
 port="45564"   #45564/UDP
 frequency="500"             #间隔500ms发送
 dropTime="3000"/>           #故障阈值3s
 <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
  address="auto"   #监听地址,此项建议修改为当前主机的IP
  port="4000" #监听端口
  autoBind="100" #如果端口冲突,自动绑定其它端口,范围是4000-4100
  selectorTimeout="5000"        #自动绑定超时时长5s
  maxThreads="6"/>
 <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
 <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
 </Sender>
<Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
<Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatchIntercep
tor"/>
</Channel>
 <Valve className="org.apache.catalina.ha.tcp.ReplicationValve" filter=""/>
 <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
 <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"
 tempDir="/tmp/war-temp/"
 deployDir="/tmp/war-deploy/"
 watchDir="/tmp/war-listen/"
 watchEnabled="false"/>
 <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>
</Cluster>
#注意:tomcat7的官方文档此处有错误
http://tomcat.apache.org/tomcat-7.0-doc/cluster-howto.html
......
<ClusterListener className="org.apache.catalina.ha.session.JvmRouteSessionIDBinderListener">
         <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener">
         </Cluster>
```

配置说明

* Cluster 集群配置 
* Manager 会话管理器配置 
* Channel 信道配置 
  * Membership 成员判定。使用什么多播地址、端口多少、间隔时长ms、超时时长ms。同一 个多播地址和端口认为同属一个组。使用时修改这个多播地址，以防冲突 
  * Receiver 接收器，多线程接收多个其他节点的心跳、会话信息。默认会从4000到4100依次尝 试可用端口 
    * address="auto"，auto可能绑定到127.0.0.1上，所以一定要改为当前主机可用的IP 
  * Sender 多线程发送器，内部使用了tcp连接池。 
  * Interceptor 拦截器
* Valve  
  * ReplicationValve 检测哪些请求需要检测Session，Session数据是否有了变化，需要启动复制过程
* ClusterListener 
  * ClusterSessionListener 集群session侦听器

使用  <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>

添加到<Engine>  所有虚拟主机都可以启用Session复制 

添加到<Host>  ，该虚拟主机可以启用Session复制 

最后，在应用程序内部启用了才可以使用

### 5.2 实战案例: 实现 Tomcat Session 集群

环境准备： 

* 时间同步，确保NTP或Chrony服务正常运行 
* 防火墙规则

| IP         | 主机名           | 服务    |               |
| ---------- | ---------------- | ------- | ------------- |
| 10.0.0.100 | proxy.magedu.org | 调度器  | Nginx、Httpd  |
| 10.0.0.101 | t1.magedu.org    | Tomcat1 | jdk8、Tomcat8 |
| 10.0.0.102 | t2.magedu.org    | Tomcat2 | jdk8、Tomcat8 |

#### 5.2.1 在 proxy 主机设置 httpd (或nginx)实现后端tomcat主机轮询

```bash
[root@proxy ~]#cat /etc/httpd/conf.d/tomcat.conf
<Proxy balancer://tomcat-server>
   BalancerMember http://t1.magedu.org:8080 loadfactor=1
   BalancerMember http://t2.magedu.org:8080 loadfactor=1
</Proxy>
<VirtualHost *:80>
   ServerName       proxy.magedu.org
   ProxyRequests     Off
   ProxyVia         On
   ProxyPreserveHost On  
   ProxyPass       / balancer://tomcat-server/
   ProxyPassReverse / balancer://tomcat-server/
</VirtualHost>
[root@proxy ~]#systemctl restart httpd
```

#### 5.2.2 在所有后端tomcat主机上修改conf/server.xml

本次把多播复制的配置放到t1.magedu.org和t2.magedu.org虚拟主机里面， 即Host块中。 

特别注意修改Receiver的address属性为一个本机可对外的IP地址。

##### 5.2.2.1 修改 t1 主机的 conf/server.xml

```bash
#将5.1 内容复制到conf/server.xml的Host块内或Engine块(针对所有主机)
[root@t1 ~]#vim /usr/local/tomcat/conf/server.xml 
[root@t1 ~]#cat /usr/local/tomcat/conf/server.xml
.....以上省略.....
   <Host name="t1.magedu.org"  appBase="/data/webapps" unpackWARs="true"
autoDeploy="true">
###################在<Host> </host>块中间加下面一段内容##############################
     <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"
 channelSendOptions="8">
 <Manager className="org.apache.catalina.ha.session.DeltaManager"
   expireSessionsOnShutdown="false"
   notifyListenersOnReplication="true"/>
 <Channel className="org.apache.catalina.tribes.group.GroupChannel">
 <Membership className="org.apache.catalina.tribes.membership.McastService"
 address="230.100.100.100"   #指定不冲突的多播地址
 port="45564"
 frequency="500"
 dropTime="3000"/>
  <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
  address="10.0.0.101"          #指定网卡的IP
  port="4000"
  autoBind="100"
  selectorTimeout="5000"
  maxThreads="6"/>
 <Sender 
className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
 <Transport 
className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
 </Sender>
<Interceptor 
className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
<Interceptor 
className="org.apache.catalina.tribes.group.interceptors.MessageDispatchIntercep
tor"/>
</Channel>
 <Valve className="org.apache.catalina.ha.tcp.ReplicationValve"
 filter=""/>
 <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
 <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"
 tempDir="/tmp/war-temp/"
 deployDir="/tmp/war-deploy/"
 watchDir="/tmp/war-listen/"
 watchEnabled="false"/>
 <ClusterListener 
className="org.apache.catalina.ha.session.ClusterSessionListener"/>
</Cluster>
#######################################以上内容是新加的#################################
     </Host>
   </Engine>
 </Service>
</Server>
[root@t1 ~]#systemctl restart tomcat

[root@t1 ~]#ss -ntl
State           Recv-Q   Send-Q Local     Address:Port Peer       Address:Port           
LISTEN           0         128               0.0.0.0:22             0.0.0.0:*              
LISTEN           0         100             127.0.0.1:25             0.0.0.0:*              
LISTEN           0         128                [::]:22                 [::]:*             
LISTEN           0         100                [::1]:25                [::]:*       
LISTEN           0         50           [::ffff:10.0.0.101]:4000       *:*      
LISTEN           0         1             ::ffff:127.0.0.1]:8005        *:*   
LISTEN           0         100                *:8009                   *:* 
LISTEN           0         100                *:8080                   *:* 
```

简化说明

t1的conf/server.xml中，如下

```xml
<Host name="t1.magedu.com" appBase="/data/webapps" autoDeploy="true" >
   #其他略去
    <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
  address="10.0.0.101"  #只改此行
  port="4000"
  autoBind="100"
  selectorTimeout="5000"
  maxThreads="6"/>
```

##### 5.2.2.2 修改 t2 主机的 conf/server.xml

```bash
[root@t2 ~]#vim /usr/local/tomcat/conf/server.xml 
[root@t2 ~]#cat /usr/local/tomcat/conf/server.xml
.....以上省略.....
   <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
        <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->
        <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
        <Valve className="org.apache.catalina.valves.AccessLogValve"
directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      </Host>
          <Host name="t2.magedu.org" appBase="/data/webapps" autoDeploy="true" >
              
###################在<Host> </host>块中间加下面一段内容##############################
     <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"
 channelSendOptions="8">
  <Manager className="org.apache.catalina.ha.session.DeltaManager"
   expireSessionsOnShutdown="false"
   notifyListenersOnReplication="true"/>
  <Channel className="org.apache.catalina.tribes.group.GroupChannel">
 <Membership className="org.apache.catalina.tribes.membership.McastService"
 address="230.100.100.100"
 port="45564"
 frequency="500"
 dropTime="3000"/>
 <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
  address="10.0.0.102"            #此行指定当前主机的IP,其它和T1节点配置相
同
  port="4000"
  autoBind="100"
  selectorTimeout="5000"
  maxThreads="6"/>
 <Sender
className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
  <Transport
className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
 </Sender>
<Interceptor
className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
<Interceptor
className="org.apache.catalina.tribes.group.interceptors.MessageDispatchIntercep
tor"/>
</Channel>
  <Valve className="org.apache.catalina.ha.tcp.ReplicationValve"
 filter=""/>
  <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
  <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"
 tempDir="/tmp/war-temp/"
 deployDir="/tmp/war-deploy/"
 watchDir="/tmp/war-listen/"
 watchEnabled="false"/>
  <ClusterListener
className="org.apache.catalina.ha.session.ClusterSessionListener"/>
</Cluster>
#######################################以上内容是新加的#################################
              </Host>
    </Engine>
  </Service>
</Server>
[root@t2 ~]#systemctl restart tomcat

[root@t2 ~]#ss -ntl
State           Recv-Q   Send-Q       Local Address:Port   Peer Address:Port  
LISTEN           0       128               0.0.0.0:22           0.0.0.0:*       
LISTEN           0       100             127.0.0.1:25           0.0.0.0:*       
LISTEN           0       128                   [::]:22             [::]:*       
LISTEN           0       100                 [::1]:25             [::]:*       
LISTEN           0       50     [::ffff:10.0.0.102]:4000               *:*       
LISTEN           0       1       [::ffff:127.0.0.1]:8005               *:*       
LISTEN           0       100                     *:8009               *:*       
LISTEN           0       100                     *:8080               *:*  
```

简化说明 

t2主机的server.xml中，如下

```xml
<Host name="t2.magedu.com" appBase="/data/webapps" autoDeploy="true" >
   其他略去
    <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
  address="10.0.0.102"  #只改此行
  port="4000"
  autoBind="100"
  selectorTimeout="5000"
  maxThreads="6"/>
```

尝试使用刚才配置过得负载均衡(移除Session黏性)，测试发现Session还是变来变去。

#### 5.2.3 修改应用的web.xml文件开启该应用程序的分布式 

参考官方说明: https://tomcat.apache.org/tomcat-8.5-doc/cluster-howto.html

```http
Make sure your web.xml has the <distributable/> element
```

为所有tomcat主机应用web.xml的  标签增加子标签  来开启该应用程序的分布式。

##### 5.2.3.1 修改t1主机的应用的web.xml文件

```bash
[root@t1 ~]#ll /usr/local/tomcat/webapps/ROOT/WEB-INF/
total 4
-rw-r----- 1 tomcat tomcat 1227 Jul  1 05:53 web.xml
[root@t1 ~]#cp -a /usr/local/tomcat/webapps/ROOT/WEB-INF/ /data/webapps/ROOT/
[root@t1 ~]#tree /data/webapps/ROOT/
/data/webapps/ROOT/
├── index.jsp
└── WEB-INF
   └── web.xml
1 directory, 2 files
#在倒数第二行加一行
[root@t1 ~]##vim /data/webapps/ROOT/WEB-INF/web.xml 
[root@t1 ~]#tail -n3 /data/webapps/ROOT/WEB-INF/web.xml
 </description>
<distributable/>  #添加此行
</web-app>
#注意权限
[root@t1 ~]#ll /data/webapps/ROOT/WEB-INF/
total 4
-rw-r----- 1 tomcat tomcat 1243 Jan 17 09:37 web.xml
[root@t1 ~]#systemctl restart tomcat

#同时观察日志
[root@t1 ~]#tail -f /usr/local/tomcat/logs/catalina.out 
15-Jul-2020 11:29:10.998 INFO [Membership-MemberAdded.] 
org.apache.catalina.ha.tcp.SimpleTcpCluster.memberAdded Replication member added:
[org.apache.catalina.tribes.membership.MemberImpl[tcp://{10, 0, 0, 102}:4000,
{10, 0, 0, 102},4000, alive=1022, securePort=-1, UDP Port=-1, id={89 -26 -30 -99
16 80 65 95 -65 14 -33 124 -55 -123 -30 82 }, payload={}, command={}, domain={}]]
```

##### 5.2.3.2 修改t2主机的应用的web.xml文件

```bash
#与5.2.3.1上的t1相同的操作
[root@t2 ~]#cp -a /usr/local/tomcat/webapps/ROOT/WEB-INF/ /data/webapps/ROOT/
[root@t2 ~]#vim /data/webapps/ROOT/WEB-INF/web.xml
[root@t2 ~]#tail -n3 /data/webapps/ROOT/WEB-INF/web.xml
 </description>
<distributable/>  #添加此行
</web-app>
#注意权限
[root@t2 ~]#ll /data/webapps/ROOT/WEB-INF/
total 4
-rw-r----- 1 tomcat tomcat 1243 Jan 17 09:38 web.xml
[root@t2 ~]#systemctl restart tomcat
#同时观察日志
[root@t2 ~]#tail -f /usr/local/tomcat/logs/catalina.out 
15-Jul-2020 11:29:12.088 INFO [t2.magedu.org-startStop-1] 
org.apache.catalina.ha.session.DeltaManager.getAllClusterSessions Manager [], 
requesting session state from 
[org.apache.catalina.tribes.membership.MemberImpl[tcp://{10, 0, 0, 101}:4000,
{10, 0, 0, 101},4000, alive=208408, securePort=-1, UDP Port=-1, id={118 -108
-116 119 58 22 73 113 -123 -96 -94 111 -65 -90 -87 -107 }, payload={}, command=
{}, domain={}]]. This operation will timeout if no session state has been 
received within [60] seconds.
```

#### 5.2.4 测试访问 

重启全部Tomcat，通过负载均衡调度到不同节点，返回的SessionID不变了。 

用浏览器访问,并刷新多次,发现SessionID 不变,但后端主机在轮询 

但此方式当后端tomcat主机较多时,会重复占用大量的内存,并不适合后端服务器众多的场景

```bash
#修改t1和t2的配置项,删除jvmRoute配置项
[root@t1 tomcat]#vim conf/server.xml
 <Engine name="Catalina" defaultHost="t1.magedu.org" > 
[root@t1 tomcat]#systemctl restart tomcat
#多次执行下面操作,可以看到SessionID不变
[root@centos7 ~]#curl -b 'JSESSIONID=1A3E7EED14F3E44FAF7469F8693E1CB6' proxy.magedu.org/index.jsp
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On tomcat-server</div>
<div>10.0.0.102:8080</div>
<div>SessionID = <span 
style="color:blue">1A3E7EED14F3E44FAF7469F8693E1CB6</span></div>
Wed Jul 15 11:33:09 CST 2020
</body>
</html>
[root@centos7 ~]#curl -b 'JSESSIONID=1A3E7EED14F3E44FAF7469F8693E1CB6' proxy.magedu.org/index.jsp
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On tomcat-server</div>
<div>10.0.0.101:8080</div>
<div>SessionID = <span 
style="color:blue">1A3E7EED14F3E44FAF7469F8693E1CB6</span></div>
Wed Jul 15 11:33:10 CST 2020
</body>
</html>
[root@centos7 ~]#
```

#### 5.2.5 故障模拟

```bash
#模拟t2节点故障
[root@t2 ~]#systemctl stop tomcat
#多次访问SessionID不变
[root@centos7 ~]#curl -b 'JSESSIONID=1A3E7EED14F3E44FAF7469F8693E1CB6' proxy.magedu.org/index.jsp
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On tomcat-server</div>
<div>10.0.0.101:8080</div>
<div>SessionID = <span 
style="color:blue">1A3E7EED14F3E44FAF7469F8693E1CB6</span></div>
Wed Jul 15 12:01:16 CST 2020
</body>
</html>
```

#### 5.2.6 恢复实验环境

本小节结束,为学习后面的内容,删除此节相关配置,为后续内容准备

```bash
#恢复t1环境
[root@t1 ~]#vim /usr/local/tomcat/conf/server.xml 
[root@t1 ~]#tail /usr/local/tomcat/conf/server.xml 
       <Valve className="org.apache.catalina.valves.AccessLogValve"
directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
     </Host>
     <Host name="t1.magedu.org"  appBase="/data/webapps" unpackWARs="true"
autoDeploy="true">
     </Host>
   </Engine>
 </Service>
</Server>
[root@t1 ~]#rm -f   /data/webapps/ROOT/WEB-INF/web.xml
[root@t1 ~]#systemctl restart tomcat
#恢复t2环境
[root@t2 ~]#vim /usr/local/tomcat/conf/server.xml
[root@t2 ~]#tail /usr/local/tomcat/conf/server.xml
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
     </Host>
         <Host name="t2.magedu.org" appBase="/data/webapps" autoDeploy="true" >
             </Host>
   </Engine>
 </Service>
</Server>
[root@t2 ~]#rm -f   /data/webapps/ROOT/WEB-INF/web.xml
[root@t2 ~]#systemctl restart tomcat
```

## 6 Memcached

### 6.1 NoSQL介绍

NoSQL是对 Not Only SQL、非传统关系型数据库的统称。 

NoSQL一词诞生于1998年，2009年这个词汇被再次提出指非关系型、分布式、不提供ACID的数据库设 计模式。 

随着互联网时代的到来，数据爆发式增长，数据库技术发展日新月异，要适应新的业务需求。

而随着移动互联网、物联网的到来，大数据的技术中NoSQL也同样重要。 

数据库排名：https://db-engines.com/en/ranking

NoSQL 分类 

* Key-value Store k/v数据库 
  * 性能好 O(1) , 如: redis、memcached
* Document Store 文档数据库 
  * mongodb、CouchDB 
* Column Store 列存数据库，Column-Oriented DB 
  * HBase、Cassandra，大数据领域应用广泛
* Graph DB 图数据库 
  * Neo4j
* Time Series 时序数据库 
  * InfluxDB、Prometheus

### 6.2 Memcached

Memcached 只支持能序列化的数据类型，不支持持久化，基于Key-Value的内存缓存系统 

memcached 虽然没有像redis所具备的数据持久化功能，比如RDB和AOF都没有，但是可以通过做集群 同步的方式，让各memcached服务器的数据进行同步，从而实现数据的一致性，即保证各memcached 的数据是一样的，即使有任何一台 memcached 发生故障，只要集群中有一台 memcached 可用就不会 出现数据丢失，当其他memcached 重新加入到集群的时候,可以自动从有数据的memcached 当中自动 获取数据并提供服务。 

Memcached 借助了操作系统的 libevent 工具做高效的读写。libevent是个程序库，它将Linux的 epoll、BSD类操作系统的kqueue等事件处理功能封装成统一的接口。即使对服务器的连接数增加，也能发挥高性能。

memcached使用这个libevent库，因此能在Linux、BSD、Solaris等操作系统上发挥其 高性能 Memcached支持最大的内存存储对象为1M，超过1M的数据可以使用客户端压缩或拆分报包放到多个 key中，比较大的数据在进行读取的时候需要消耗的时间比较长，memcached 最适合保存用户的 session实现session共享 

Memcached存储数据时, Memcached会去申请1MB的内存, 把该块内存称为一个slab, 也称为一个page 

Memcached 支持多种开发语言，包括：JAVA,C,Python,PHP,C#,Ruby,Perl等 

Memcached 官网：http://memcached.org/

### 6.3 Memcached 和 Redis 比较

| 比较类别       | Redis                                                        | Memcached                                                    |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 支持的数据结构 | 哈希、列表、集合、有序集合                                   | 纯kev-value                                                  |
| 持久化支持     | 有                                                           | 无                                                           |
| 高可用支持     | redis支持集群功能，可以实现主动复制，读写分离。 官方也提供了sentinel集群管理工具，能够实现主从 服务监控，故障自动转移，这一切，对于客户端都是透明的，无需程序改动，也无需人工介入 | 需要二次开发                                                 |
| 存储value容量  | 最大512M                                                     | 最大1M                                                       |
| 内存分配       | 临时申请空间，可能导致碎片                                   | 预分配内存池的方式管理内 存，能够省去内存分配时间            |
| 虚拟内存使用   | 有自己的VM机制，理论上能够存储比物理内存更多的数据，当数据超量时，会引发swap，把冷数据刷到磁盘上 | 所有的数据存储在物理内存里                                   |
| 网络模型       | 非阻塞IO复用模型,提供一些非KV存储之外的排序， 聚合功能，在执行这些功能时，复杂的CPU计算，会 阻塞整个IO调度 | 非阻塞IO复用模型                                             |
| 水平扩展的支持 | redis cluster 可以横向扩展                                   | 暂无                                                         |
| 多线程         | Redis6.0之前是只支持单线程                                   | Memcached支持多线程,CPU 利用方面Memcache优于 Redis           |
| 过期策略       | 有专门线程，清除缓存数据                                     | 懒淘汰机制：每次往缓存放入 数据的时候，都会存一个时 间，在读取的时候要和设置的时间做TTL比较来判断是否过期 |
| 单机QPS        | 约10W                                                        | 约60W                                                        |
| 源代码可读性   | 代码清爽简洁                                                 | 可能是考虑了太多的扩展性， 多系统的兼容性，代码不清爽        |
| 适用场景       | 复杂数据结构、有持久化、高可用需求、value存储内容较大        | 纯KV，数据量非常大，并发量非常大的业务                       |

### 6.4 Memcached 工作机制

#### 6.4.1 内存分配机制

应用程序运行需要使用内存存储数据，但对于一个缓存系统来说，申请内存、释放内存将十分频繁，非 常容易导致大量内存碎片，最后导致无连续可用内存可用。

Memcached采用了Slab Allocator机制来分配、管理内存。

* Page：分配给Slab的内存空间，默认为1MB，分配后就得到一个Slab。Slab分配之后内存按照固定 字节大小等分成chunk。 
* Chunk：用于缓存记录k/v值的内存空间。Memcached会根据数据大小选择存到哪一个chunk中， 假设chunk有128bytes、64bytes等多种，数据只有100bytes存储在128bytes中，存在少许浪费。 
  * Chunk最大就是Page的大小，即一个Page中就一个Chunk
* Slab Class：Slab按照Chunk的大小分组，就组成不同的Slab Class, 第一个Chunk大小为 96B的 Slab为Class1,Chunk 120B为Class 2,如果有100bytes要存，那么Memcached会选择下图中Slab  Class 2 存储，因为它是120bytes的Chunk。Slab之间的差异可以使用Growth Factor 控制，默认 1.25。

范例：查看Slab Class

```bash
#-f, --slab-growth-factor=<num> chunk size growth factor (default: 1.25)
[root@centos8 ~]#memcached -u memcached -f 2 -vv
slab class   1: chunk size        96 perslab   10922
slab class   2: chunk size       192 perslab    5461
slab class   3: chunk size       384 perslab    2730
slab class   4: chunk size       768 perslab    1365
slab class   5: chunk size      1536 perslab     682
slab class   6: chunk size      3072 perslab     341
slab class   7: chunk size      6144 perslab     170
slab class   8: chunk size     12288 perslab      85
slab class   9: chunk size     24576 perslab      42
slab class  10: chunk size     49152 perslab      21
slab class  11: chunk size     98304 perslab      10
slab class  12: chunk size    196608 perslab       5
slab class  13: chunk size    524288 perslab       2
<27 server listening (auto-negotiate)
<28 server listening (auto-negotiate)
```

#### 6.4.2 懒过期 Lazy Expiration

memcached不会监视数据是否过期，而是在取数据时才看是否过期，如果过期,把数据有效期限标识为0，并不清除该数据。以后可以覆盖该位置存储其它数据。

#### 6.4.3 LRU

当内存不足时，memcached会使用LRU（Least Recently Used）机制来查找可用空间，分配给新记录使用。

#### 6.4.4 集群

Memcached集群，称为基于客户端的分布式集群，即由客户端实现集群功能，即Memcached本身不支 持集群 

Memcached集群内部并不互相通信，一切都需要客户端连接到Memcached服务器后自行组织这些节 点，并决定数据存储的节点。

### 6.5 安装和启动

官方安装说明：https://github.com/memcached/memcached/wiki/Install

#### 6.5.1 yum安装

范例: CentOS 8 安装 memcached

```bash
[root@centos8 ~]#dnf info memcached
[root@centos8 ~]#dnf -y install memcached
[root@centos8 ~]#rpm -ql memcached
/etc/sysconfig/memcached
/usr/bin/memcached
/usr/bin/memcached-tool
/usr/lib/.build-id
/usr/lib/.build-id/25
/usr/lib/.build-id/25/2528fb78bbe5b14596eb4ee8c88120b5cc6b59
/usr/lib/systemd/system/memcached.service
/usr/share/doc/memcached
/usr/share/doc/memcached/AUTHORS
/usr/share/doc/memcached/CONTRIBUTORS
/usr/share/doc/memcached/COPYING
/usr/share/doc/memcached/ChangeLog
/usr/share/doc/memcached/NEWS
/usr/share/doc/memcached/README.md
/usr/share/doc/memcached/new_lru.txt
/usr/share/doc/memcached/protocol.txt
/usr/share/doc/memcached/readme.txt
/usr/share/doc/memcached/storage.txt
/usr/share/doc/memcached/threads.txt
/usr/share/man/man1/memcached-tool.1.gz
/usr/share/man/man1/memcached.1.gz

[root@t1 ~]#cat /etc/sysconfig/memcached 
PORT="11211"     #监听端口
USER="memcached" #启动用户
MAXCONN="1024"  #最大连接数
CACHESIZE="64"  #最大使用内存
OPTIONS="-l 127.0.0.1,::1" #其他选项

[root@centos8 ~]#grep -Ev "^#|^$" /usr/lib/systemd/system/memcached.service
[Unit]
Description=memcached daemon
Before=httpd.service
After=network.target
[Service]
EnvironmentFile=/etc/sysconfig/memcached
ExecStart=/usr/bin/memcached -p ${PORT} -u ${USER} -m ${CACHESIZE} -c ${MAXCONN}
$OPTIONS
PrivateTmp=true
ProtectSystem=full
NoNewPrivileges=true
PrivateDevices=true
CapabilityBoundingSet=CAP_SETGID CAP_SETUID CAP_SYS_RESOURCE
RestrictAddressFamilies=AF_INET AF_INET6 AF_UNIX
[Install]
WantedBy=multi-user.target

[root@centos8 ~]#getent passwd memcached
memcached:x:992:989:Memcached daemon:/run/memcached:/sbin/nologin
[root@centos8 ~]#systemctl enable --now memcached
[root@centos8 ~]#pstree -p |grep memcached
           |-memcached(25582)-+-{memcached}(25584)
           |                 |-{memcached}(25585)
           |                 |-{memcached}(25586)
           |                 |-{memcached}(25587)
           |                 |-{memcached}(25588)
           |                 |-{memcached}(25589)
           |                 |-{memcached}(25590)
           |                 |-{memcached}(25591)
           |                  `-{memcached}(25592)
[root@centos8 ~]#ss -ntlup|grep memcached
tcp   LISTEN  0       128                127.0.0.1:11211         0.0.0.0:*     users:(("memcached",pid=25453,fd=27))           
tcp   LISTEN  0       128                   [::1]:11211           [::]:*     users:(("memcached",pid=25453,fd=28)) 

#修改端口绑定的IP为当前主机的所有IP
[root@centos8 ~]#vim /etc/sysconfig/memcached 
[root@centos8 ~]#cat /etc/sysconfig/memcached 
PORT="11211"
USER="memcached"
MAXCONN="1024"
CACHESIZE="64"
#OPTIONS="-l 127.0.0.1,::1" #注释此行
OPTIONS=""

[root@centos8 ~]#systemctl restart memcached.service 
[root@centos8 ~]#ss -ntul
Netid     State   Recv-Q Send-Q Local Address:Port   Peer Address:Port      
udp       UNCONN  0      0          127.0.0.1:323          0.0.0.0:*         
udp       UNCONN  0      0             [::1]:323             [::]:*         
tcp       LISTEN  0      128          0.0.0.0:22           0.0.0.0:*         
tcp       LISTEN  0      100        127.0.0.1:25           0.0.0.0:*         
tcp       LISTEN  0      128          0.0.0.0:11211        0.0.0.0:*         
tcp       LISTEN  0      128             [::]:22             [::]:*         
tcp       LISTEN  0      100           [::1]:25             [::]:*         
tcp       LISTEN  0      128             [::]:11211           [::]:* 
```

范例: CentOS 7 安装 memcached

```bash
[root@centos7 ~]#yum info memcached
[root@centos7 ~]# yum install memcached
[root@centos7 ~]# rpm -ql memcached
/etc/sysconfig/memcached
/usr/bin/memcached
/usr/bin/memcached-tool
/usr/lib/systemd/system/memcached.service
/usr/share/doc/memcached-1.4.15
/usr/share/doc/memcached-1.4.15/AUTHORS
/usr/share/doc/memcached-1.4.15/CONTRIBUTORS
/usr/share/doc/memcached-1.4.15/COPYING
/usr/share/doc/memcached-1.4.15/ChangeLog
/usr/share/doc/memcached-1.4.15/NEWS
/usr/share/doc/memcached-1.4.15/README.md
/usr/share/doc/memcached-1.4.15/protocol.txt
/usr/share/doc/memcached-1.4.15/readme.txt
/usr/share/doc/memcached-1.4.15/threads.txt
/usr/share/man/man1/memcached-tool.1.gz
/usr/share/man/man1/memcached.1.gz

[root@centos7 ~]#getent passwd memcached
memcached:x:997:995:Memcached daemon:/run/memcached:/sbin/nologin

[root@centos7 ~]# cat /usr/lib/systemd/system/memcached.service
[Service]
Type=simple
EnvironmentFile=-/etc/sysconfig/memcached
ExecStart=/usr/bin/memcached -u $USER -p $PORT -m $CACHESIZE -c $MAXCONN
$OPTIONS

[root@centos7 ~]# cat /etc/sysconfig/memcached
PORT="11211"
USER="memcached"
MAXCONN="1024"
CACHESIZE="64"
OPTIONS=""

#前台显示看看效果
[root@centos7 ~]# memcached -u memcached -p 11211 -f 1.25 -vv
[root@centos7 ~]# systemctl start memcached
[root@centos7 ~]#ss -ntlpu|grep memcached
udp   UNCONN     0      0         *:11211                 *:*         users:
(("memcached",pid=2591,fd=28))
udp   UNCONN     0      0     [::]:11211             [::]:*         users:
(("memcached",pid=2591,fd=29))
tcp   LISTEN     0      128       *:11211                 *:*         users:
(("memcached",pid=2591,fd=26))
tcp   LISTEN     0      128   [::]:11211             [::]:*         users:
(("memcached",pid=2591,fd=27))
tcp   LISTEN     0      100   [::1]:25              [::]:* 
```

#### 6.5.2 编译安装

```bash
[root@centos7 ~]#yum -y install gcc libevent-devel 
[root@centos7 ~]#wget http://memcached.org/files/memcached-1.6.6.tar.gz
[root@centos7 ~]#tar xvf memcached-1.6.6.tar.gz
[root@centos7 ~]#cd memcached-1.6.6/
[root@centos7 memcached-1.6.6]#./configure --prefix=/apps/memcached
[root@centos7 memcached-1.6.6]#make && make install
[root@centos7 ~]#tree /apps/memcached/
/apps/memcached/
├── bin
│   └── memcached
├── include
│   └── memcached
│       └── protocol_binary.h
└── share
   └── man
       └── man1
           └── memcached.1
6 directories, 3 files
[root@centos7 ~]#echo 'PATH=/apps/memcached/bin:$PATH' > 
/etc/profile.d/memcached.sh
[root@centos7 ~]#. /etc/profile.d/memcached.sh
#准备用户
[root@centos7 ~]#useradd -r -s /sbin/nologin memcached
[root@centos7 ~]#cat /etc/sysconfig/memcached 
PORT="11211"
USER="memcached"
MAXCONN="1024"
CACHESIZE="64"
OPTIONS="-l 127.0.0.1,::1"
#默认前台执行
[root@centos7 ~]#memcached -u memcached -m 2048 -c 65536 -f 2 -vv 
#以台方式执行
[root@centos7 ~]#memcached -u memcached -m 2048 -c 65536 -d
#准备service文件
[root@centos7 ~]#cat /lib/systemd/system/memcached.service 
[Unit]
Description=memcached daemon
Before=httpd.service
After=network.target
[Service]
EnvironmentFile=/etc/sysconfig/memcached
ExecStart=/apps/memcached/bin/memcached -p ${PORT} -u ${USER} -m ${CACHESIZE} -c
${MAXCONN} $OPTIONS
[Install]
WantedBy=multi-user.target

[root@centos7 ~]#systemctl daemon-reload 
[root@centos7 ~]#systemctl enable --now memcached.service

[root@centos7 ~]#ss -ntl
State     Recv-Q Send-Q Local Address:Port   Peer Address:Port
LISTEN     0      128         127.0.0.1:11211     *:*                  
LISTEN     0      128                 *:22       *:*                  
LISTEN     0      100         127.0.0.1:25       *:*                  
LISTEN     0      128             [::1]:11211     [::]:*                  
LISTEN     0      128             [::]:22       [::]:*                  
LISTEN     0      100             [::1]:25       [::]:*  
[root@centos7 ~]#memcached --version 
memcached 1.6.6
```

#### 6.5.3 memcached 启动程序说明

修改memcached 运行参数，可以使用下面的选项修改/etc/sysconfig/memcached文件

memcached 常见选项

```bash
-u username memcached运行的用户身份，必须普通用户
-p 绑定的端口，默认11211
-m num 最大内存，单位MB，默认64MB
-c num 最大连接数，缺省1024
-d 守护进程方式运行
-f 增长因子Growth Factor，默认1.25
-v 详细信息，-vv能看到详细信息
-M 使用内存直到耗尽，不许LRU
-U 设置UDP监听端口，0表示禁用UDP
```

范例: 

```bash
[root@centos8 ~]#memcached -u memcached -p 11211 -f 2 -vv
[root@centos8 ~]#memcached -u memcached -p 11211 -f 1.25 -vv
```

### 6.6 使用 memcached

#### 6.6.1 memcached 开发库和工具

与memcached通信的不同语言的连接器。libmemcached提供了C库和命令行工具。  

范例: 查看memcached相关包

```bash
[root@centos8 ~]#yum list "*memcached*"
[root@centos7 ~]#yum list "*memcached*"
```

协议 

查看/usr/share/doc/memcached-1.4.15/protocol.txt 

```bash
[root@centos8 ~]#dnf info libmemcached
[root@centos8 ~]#dnf repoquery -l libmemcached
#测试memcached是否可访问
[root@centos8 ~]#memping --servers=10.0.0.7
[root@centos8 ~]#echo $?
0
[root@centos8 ~]#memping --servers=10.0.0.77
Failed to ping 10.0.0.77:11211 SYSTEM ERROR

#查看memcached状态
[root@centos8 ~]#memstat --servers=10.0.0.101
```

#### 6.6.2 memcached 操作命令

帮助文档: 

```bash
[root@centos8 ~]#cat /usr/share/doc/memcached/protocol.txt
```

五种基本 memcached 命令执行最简单的操作。这些命令和操作包括：

* set
* add 
* replace 
* get 
* delete

```bash
#前三个命令是用于操作存储在 memcached 中的键值对的标准修改命令,都使用如下所示的语法：
command <key> <flags> <expiration time> <bytes>
<value>
#参数说明如下：
command set/add/replace
key     key 用于查找缓存值
flags     可以包括键值对的整型参数，客户机使用它存储关于键值对的额外信息
expiration time     在缓存中保存键值对的时间长度（以秒为单位，0 表示永远）
bytes     在缓存中存储的字节数
value     存储的值（始终位于第二行）
#增加key，过期时间为秒，bytes为存储数据的字节数
add key flags exptime bytes
```

范例：

```bash
[root@centos8 ~]#systemctl start memcached
[root@centos8 ~]#telnet localhost 11211
Trying ::1...
Connected to localhost.
Escape character is '^]'.
stats
......

stats items #显示各个 slab 中 item 的数目和存储时长(最后一次访问距离现在的秒数)。
STAT items:1:number 2
STAT items:1:number_hot 0
......

stats slabs #用于显示各个slab的信息，包括chunk的大小、数目、使用情况等
STAT 1:chunk_size 96
STAT 1:chunks_per_page 10922
......

#加
add mykey 1 60 4   
test
STORED
#查
get mykey
VALUE mykey 1 4
test
END

#改
set mykey 1 60 5
test1
STORED
get mykey
VALUE mykey 1 5
test1
END

#删除
delete mykey
DELETED
get mykey
END

#删除
delete mykey
DELETED
get mykey
END
```

#### 6.6.3 python 语言连接 memcached

##### 6.6.3.1 范例: python3 测试代码

```bash
[root@centos8 ~]#yum -y install python3 python3-memcached
[root@centos8 ~]#cat m3.py 
#!/usr/bin/python3
#coding:utf-8
import memcache
m = memcache.Client(['127.0.0.1:11211'], debug=True)
for i in range(10):
   m.set("key%d" % i,"v%d" % i)
   ret = m.get('key%d' % i)
   print("%s" % ret)
[root@centos8 ~]#chmod +x m3.py
[root@centos8 ~]#./m3.py 
v0
v1
v2
v3
v4
v5
v6
v7
v8
v9
```

##### 6.6.3.2 范例: python2 测试代码

```bash
[root@centos7 ~]#yum -y install python-memcached 
[root@centos7 ~]#cat m2.py 
#!/usr/bin/env python
#coding:utf-8
import memcache
m = memcache.Client(['127.0.0.1:11211'], debug=True)
for i in range(10):
 m.set("key%d" % i,"v%d" % i)
 ret = m.get('key%d' % i)
 print ret
[root@centos7 ~]#python m2.py
v0
v1
v2
v3
v4
v5
v6
v7
v8
v9

[root@centos7 ~]#telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
get key1
VALUE key1 0 2
v1
END
get key9
VALUE key9 0 2
v9
END
get key10
END
quit
Connection closed by foreign host.
[root@centos7 ~]#
```

### 6.7 memcached 集群部署架构

#### 6.7.1 基于 magent 的部署架构

Magent是google开发的项目,应用端通过负载均衡服务器连接到magent，然后再由magent代理用户应 用请求到memcached处理，底层的memcached为双主结构会自动同步数据，本部署方式存在magent 单点问题,因此需要两个magent做高可用。

项目站点：https://code.google.com/archive/p/memagent/ 

此项目过于陈旧,且不稳定,当前已经废弃

#### 6.7.2 Repcached 实现原理 

项目站点：http://repcached.sourceforge.net/

在 master上可以通过 -X 选项指定 replication port(默认为11212/tcp)，在 slave上通过 -x 指定复制的 master并连接，事实上，如果同时指定了 -x/-X， repcached先会尝试连接对端的master，但如果连接 失败，它就会用 -X参数来自己 listen（成为master）；如果 master坏掉， slave侦测到连接断了，它会 自动 listen而成为 master；而如果 slave坏掉，master也会侦测到连接断开，它就会重新 listen等待新 的 slave加入。

从这方案的技术实现来看，其实它是一个单 master单 slave的方案，但它的 master/slave都是可读写 的，而且可以相互同步，所以从功能上看，也可以认为它是双机master-master方案

#### 6.7.3 简化后的部署架构

magent已经有很长时间没有更新，因此可以不再使用magent，直接通过负载均衡连接到 memcached，仍然有两台memcached做高可用，repcached版本的memcached之间会自动同步数 据，以保持数据一致性，即使其中的一台memcached故障也不影响业务正常运行，故障的memcached 修复上线后再自动从另外一台同步数据即可保持数据一致性。

#### 6.7.4 部署repcached

```bash
[root@centos7 ~]# yum -y install gcc libevent libevent-devel
[root@centos7 ~]# wget 
https://jaist.dl.sourceforge.net/project/repcached/repcached/2.2.1-
1.2.8/memcached-1.2.8-repcached-2.2.1.tar.gz
[root@centos7 ~]# tar xf memcached-1.2.8-repcached-2.2.1.tar.gz
[root@centos7 ~]# cd memcached-1.2.8-repcached-2.2.1
[root@centos7 memcached-1.2.8-repcached-2.2.1]# ./configure --prefix=/apps/repcached --enable-replication
[root@centos7 memcached-1.2.8-repcached-2.2.1]# make #报错如下
```

解决办法：

```bash
[root@centos7 memcached-1.2.8-repcached-2.2.1]# vim memcached.c
56 #ifndef IOV_MAX
57 #if defined(__FreeBSD__) || defined(__APPLE__)
58 # define IOV_MAX 1024
59 #endif
60 #endif
#改为如下内容，即删除原有的原第57，59行
56 #ifndef IOV_MAX
57 # define IOV_MAX 1024
58 #endif
```

再次编译安装：

```bash
[root@centos7 memcached-1.2.8-repcached-2.2.1]#make && make install 
[root@centos7 ~]#tree /apps/repcached/
/apps/repcached/
├── bin
│   ├── memcached
│   └── memcached-debug
└── share
   └── man
       └── man1
           └── memcached.1
4 directories, 3 files
```

#### 6.7.5 验证是否可执行

```bash
[root@centos7 ~]#echo 'PATH=/apps/repcached/bin:$PATH' > 
/etc/profile.d/repcached.sh
[root@centos7 ~]#. /etc/profile.d/repcached.sh
[root@centos7 ~]#memcached -h
memcached 1.2.8
repcached 2.2.1
-p <num>     TCP port number to listen on (default: 11211)
-U <num>     UDP port number to listen on (default: 11211, 0 is off)
...
```

#### 6.7.6 启动 memcached 

##### 6.7.6.1 server1 相关操作

```bash
#创建用户
[root@centos7 ~]#useradd -r -s /sbin/nologin memcached
#后台启动
#-x 10.0.0.17为对端memcached的地址
[root@centos7 ~]#memcached -d -m 2048 -p 11211 -u memcached -c 2048 -x 10.0.0.17 
[root@centos7 ~]#ss -ntl
```

##### 6.7.6.2 server2 相关操作

```bash
[root@centos7 ~]#memcached -d -m 2048 -p 11211 -u memcached -c 2048 -x 10.0.0.7
[root@centos7 ~]#ss -ntlu
```

#### 6.7.7 连接到 memcached 验证数据

##### 6.7.7.1 shell 命令测试

```bash
#在第一台memcached上创建数据
[root@centos7 ~]#telnet 10.0.0.7 11211
Trying 10.0.0.7...
Connected to 10.0.0.7.
Escape character is '^]'.
add name 0 0 4  #key 名为name，且永不超时
wang
STORED
get name
VALUE name 0 4
wang
END
quit
Connection closed by foreign host.
[root@centos7 ~]#
#到另外一台memcached服务器验证是否有同步过来的数据
[root@centos7 ~]#telnet 10.0.0.17 11211 
Trying 10.0.0.17...
Connected to 10.0.0.17.
Escape character is '^]'.
get name
VALUE name 0 4
wang
END
set age 0 0 2   #新建 key
20
quit
Connection closed by foreign host.
#在第一台memcached上验证实现双向复制
[root@centos7 ~]#telnet 10.0.0.7 11211
Trying 10.0.0.7...
Connected to 10.0.0.7.
Escape character is '^]'.
get age
VALUE age 0 2
20
END
```

##### 6.7.7.2 python 脚本连接 memcached

示例：测试连接memcached的Python脚本

```python
#!/usr/bin/env python
#coding:utf-8
import memcache
m = memcache.Client(['10.0.0.100:11211'], debug=True)
for i in range(100):
 m.set("key%d" % i,"v%d" % i)
 ret = m.get('key%d' % i)
 print ret
```

范例：

```bash
[root@centos7 ~]#yum -y install python-memcached 
[root@centos7 ~]#cat ./m2.py
#!/usr/bin/env python
#coding:utf-8
import memcache
m = memcache.Client(['10.0.0.7:11211'], debug=True)
for i in range(10):
 m.set("key%d" % i,"v%d" % i)
 ret = m.get('key%d' % i)
 print ret
[root@centos7 ~]#chmod +x m2.py
[root@centos7 ~]#./m2.py
v0
v1
v2
v3
v4
v5
v6
v7
v8
v9
[root@centos7 ~]#telnet 10.0.0.7 11211
Trying 10.0.0.7...
Connected to 10.0.0.7.
Escape character is '^]'.
get key1
VALUE key1 0 2
v1
END
get key6
VALUE key6 0 2
v6
END
get key10
END
quit
Connection closed by foreign host.

#在第二台memcached上验证同步的数据
[root@centos7 ~]#telnet 10.0.0.17 11211
Trying 10.0.0.17...
Connected to 10.0.0.17.
Escape character is '^]'.
get key1
VALUE key1 0 2
v1
END
get key6
VALUE key6 0 2
v6
END
get key10
END
quit
Connection closed by foreign host.
```

## 7 session 共享服务器

### 7.1 msm 介绍

msm（memcached session manager）提供将Tomcat的session保持到memcached或redis的程序， 可以实现高可用。 

项目早期托管在google code,目前在Github 

github网站链接: https://github.com/magro/memcached-session-manager

支持Tomcat的 6.x、7.x、8.x、9.x 

* Tomcat的Session管理类，Tomcat版本不同 
  * memcached-session-manager-2.3.2.jar 
  * memcached-session-manager-tc8-2.3.2.jar 
* Session数据的序列化、反序列化类 
  * 官方推荐kyro 
  * 在webapp中WEB-INF/lib/下
* 驱动类 
  * memcached(spymemcached.jar) 
  * Redis(jedis.jar)

### 7.2 安装

参考链接: https://github.com/magro/memcached-session-manager/wiki/SetupAndConfiguration 

将spymemcached.jar、memcached-session-manage、kyro相关的jar文件都放到Tomcat的lib目录中 去，这个目录是 $CATALINA_HOME/lib/ ，对应本次安装就是/usr/local/tomcat/lib。

```http
kryo-3.0.3.jar
asm-5.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
minlog-1.3.1.jar
kryo-serializers-0.45.jar
msm-kryo-serializer-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
spymemcached-2.12.3.jar
memcached-session-manager-2.3.2.jar
```

### 7.3 sticky 模式 

#### 7.3.1 sticky 模式工作原理

sticky 模式即前端tomcat和后端memcached有关联(粘性)关系 

参考文档:https://github.com/magro/memcached-session-manager/wiki/SetupAndConfiguration

```bash
Tomcat-1 (t1) will primarily store it's sessions in memcached-2 (m2) which is 
running on another machine (m2 is a regular node for t1). Only if m2 is not 
available, t1 will store it's sessions in memcached-1 (m1, m1 is the failoverNode 
for t1). With this configuration, sessions won't be lost when machine 1 (serving 
t1 and m1) crashes. The following really nice ASCII art shows this setup.
Tomcat-1（t1）主要将其会话存储在另一台计算机上运行的memcached-2（m2）中（m2是t1的常规节
点）。 仅当m2不可用时，t1才会将其会话存储在memcached-1中（m1，m1是t1的failoverNode）。 使
用此配置，当计算机1（服务于t1和m1）崩溃时，会话不会丢失。 以下非常好的ASCII艺术显示了此设置。
<t1>   <t2>
 . \ / .
 . X .
 . / \ .
<m1>   <m2>
```

t1和m1部署可以在一台主机上，t2和m2部署也可以在同一台。 

当新用户发请求到Tomcat1时, Tomcat1生成session返回给用户的同时,也会同时发给memcached2备 份。即Tomcat1 session为主session，memcached2 session为备用session，使用memcached相当于备份了一份Session 

如果Tomcat1发现memcached2 失败,无法备份Session到memcached2,则将Sessoin备份存放在 memcached1中

#### 7.3.2 配置过程 

##### 7.3.2.1 下载相关jar包 

下载相关jar包,参考下面官方说明的下载链接 

https://github.com/magro/memcached-session-manager/wiki/SetupAndConfiguration 

**tomcat和memcached相关包**

##### 7.3.2.2 修改tomcat配置

修改 $CATALINA_HOME/conf/context.xml 

特别注意，t1配置中为failoverNodes="n1"， t2配置为failoverNodes="n2"

```xml
#以下是sticky的配置
<Context>
 ...
  <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="n1:10.0.0.101:11211,n2:10.0.0.102:11211"
    failoverNodes="n1"
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFac
tory"
    />
</Context>
```

**配置说明**

```http
memcachedNodes="n1:host1.yourdomain.com:11211,n2:host2.yourdomain.com:11211"

memcached的节点: n1、n2只是别名，可以重新命名。
failoverNodes 为故障转移节点，n1是备用节点，n2是主存储节点。另一台Tomcat将n1改为n2，其主节点是n1，备用节点是n2。
```

如果配置成功，可以在logs/catalina.out中看到下面的内容

```bash
12-APR-2020 16:24:08.975 INFO [t1.magedu.com-startStop-1] 
de.javakaffee.web.msm.MemcachedSessionService.startInternal --------
- finished initialization:
- sticky: true
- operation timeout: 1000
- node ids: [n2]
- failover node ids: [n1]
- storage key prefix: null
- locking mode: null (expiration: 5s)
```

配置成功后，网页访问以下，页面中看到了Session。然后运行下面的Python程序，就可以看到是否存储到了memcached中了。

##### 7.3.2.3 准备测试msm的python脚本

范例：安装python环境准备python程序查看memcached中的SessionID

```bash
[root@centos8 ~]#yum -y install python3 python3-memcached
#或者执行下面两步
[root@centos8 ~]#dnf install python3 -y
[root@centos8 ~]#pip3 install python-memcached
#脚本内容
[root@centos8 ~]#cat showmemcached.py 
#!/usr/bin/python3
import memcache
mc = memcache.Client(['10.0.0.101:11211'], debug=True)
stats = mc.get_stats()[0]
print(stats)
for k,v in stats[1].items():
    print(k, v)
print('-' * 30)
# 查看全部key
print(mc.get_stats('items')) # stats items 返回 items:5:number 1
print('-' * 30)
print(mc.get_stats('cachedump 5 0')) # stats cachedump 5 0 # 5和上面的items返回的值有关；0表示全部
```

t1、t2、n1、n2依次启动成功，分别使用http://t1.magedu.org:8080/ 和http://t2.magedu.org:8080/ 观察。 

开启负载均衡调度器，通过http://proxy.magedu.com来访问看看效果

```bash
On tomcats
10.0.0.102:8080
SessionID = 2A19B1EB6D9649C9FED3E7277FDFD470-n2.Tomcat1
Wed Jun 26 16:32:11 CST 2019

On tomcats
10.0.0.101:8080
SessionID = 2A19B1EB6D9649C9FED3E7277FDFD470-n1.Tomcat2
Wed Jun 26 16:32:36 CST 2019
```

可以看到浏览器端被调度到不同Tomcat上，但是都获得了同样的SessionID。 

停止t2、n2看看效果，恢复看看效果。 

范例：访问tomcat，查看memcached中SessionID信息

```bash
[root@centos8 ~]#python3 showmemcached.py 
.....
[('10.0.0.48:11211 (1)', {'8D0B801640CA0B1EB9AD4A4C221EA81A-n2.Tomcat1': '[97 b; 
1581598952 s]'})]
#以上结果表示SessionID来自于memcached的n2节点，但是最终是由tomcat1返回的SessionID
```

#### 7.3.3 实战案例 1 : tomcat和memcached集成在一台主机

环境准备： 

* 时间同步，确保NTP或Chrony服务正常运行。 
* 防火墙规则 
* 禁用SELinux 
* 三台主机

| IP         | 主机名 | 服务    | 软件                              |
| ---------- | ------ | ------- | --------------------------------- |
| 10.0.0.100 | proxy  | 调度器  | CentOS8、Nginx、HTTPD             |
| 10.0.0.101 | t1     | tomcat1 | CentOS8、JDK8、Tomcat8、memcached |
| 10.0.0.102 | t2     | tomcat2 | CentOS8、JDK8、Tomcat8、memcached |

##### 7.3.3.1 配置nginx充当proxy

```bash
[root@proxy ~]#cat /etc/nginx/nginx.conf
http {
......
 upstream tomcat-server {
        #ip_hash; 
       server t1.magedu.org:8080;
       server t2.magedu.org:8080;
 }
   server {
 ......
       location / {
       }
       location ~* \.(jsp|do)$ {
         proxy_pass http://tomcat-server;
       }
[root@proxy ~]#cat /etc/hosts
10.0.0.100 proxy.magedu.org proxy
10.0.0.101 t1.magedu.org t1
10.0.0.102 t2.magedu.org t2
```

##### 7.3.3.2 配置memcached

###### 7.3.3.2.1 在 tomcat1 上配置 memcached

```bash
[root@t1 ~]#dnf -y install memcached 
[root@t1 ~]#vim /etc/sysconfig/memcached 
[root@t1 ~]#cat /etc/sysconfig/memcached 
PORT="11211"
USER="memcached"
MAXCONN="1024"
CACHESIZE="64"
#注释下面行
#OPTIONS="-l 127.0.0.1,::1" 
[root@t1 ~]#systemctl enable --now memcached.service 
```

###### 7.3.3.2.2 在 tomcat2 上配置 memcached

配置和t1相同

```bash
[root@t2 ~]#dnf -y install memcached 
[root@t2 ~]#vim /etc/sysconfig/memcached 
[root@t2 ~]#cat /etc/sysconfig/memcached 
PORT="11211"
USER="memcached"
MAXCONN="1024"
CACHESIZE="64"
#注释下面行
#OPTIONS="-l 127.0.0.1,::1"
[root@t2 ~]#systemctl enable --now memcached.service 
```

##### 7.3.3.3 配置 tomcat 

###### 7.3.3.3.1 配置 tomcat1

```bash
[root@t1 tomcat]#vim conf/server.xml
 <Engine name="Catalina" defaultHost="t1.magedu.org" jvmRoute="Tomcat1">  
 ......
       <Host name="t1.magedu.org" appBase="/data/webapps" autoDeploy="true" > 
     </Host>  
   </Engine>
 </Service>
</Server>
[root@t1 tomcat]#vim conf/context.xml
<Context>
......
   <Manager pathname="" />
    -->
###倒数第一行前,即</Context>行的前面,加下面内容
 <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="n1:10.0.0.101:11211,n2:10.0.0.102:11211"                     
            
    failoverNodes="n1"
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFact
ory"
   />
</Context>  #最后一行
#将相关包传到lib/目录下
asm-5.2.jar
kryo-3.0.3.jar
kryo-serializers-0.45.jar
memcached-session-manager-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
minlog-1.3.1.jar
msm-kryo-serializer-2.3.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
spymemcached-2.12.3.jar
[root@t1 tomcat]#ls lib/ -t |tail 
kryo-3.0.3.jar
asm-5.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
minlog-1.3.1.jar
kryo-serializers-0.45.jar
msm-kryo-serializer-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
spymemcached-2.12.3.jar
memcached-session-manager-2.3.2.jar
[root@t1 tomcat]#systemctl restart tomcat
[root@t1 tomcat]#cat /data/webapps/ROOT/index.jsp
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On <%=request.getServerName() %></div>
<div><%=request.getLocalAddr() + ":" + request.getLocalPort() %></div>
<div>SessionID = <span style="color:blue"><%=session.getId() %></span></div>
<%=new Date()%>
</body>
</html>
```

###### 7.3.3.3.2 配置 tomcat2

```bash
#t2参考上面t1做配置
[root@t2 tomcat]#vim conf/server.xml
 <Engine name="Catalina" defaultHost="t2.magedu.org" jvmRoute="Tomcat2">  
 ......
     <Host name="t2.magedu.org" appBase="/data/webapps" autoDeploy="true" > 
     </Host>  
   </Engine>
 </Service>
</Server>
[root@t2 tomcat]#vim conf/context.xml
<Context>
......
   <Manager pathname="" />
    -->
####################加下面内容################
 <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="n1:10.0.0.101:11211,n2:10.0.0.102:11211"                     
            
    failoverNodes="n2"  #只修改此行,和t1不同,其它都一样
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFact
ory"
   />
####################加上面内容################
</Context>
#将相关包传到lib/目录下
asm-5.2.jar
kryo-3.0.3.jar
kryo-serializers-0.45.jar
memcached-session-manager-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
minlog-1.3.1.jar
msm-kryo-serializer-2.3.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
spymemcached-2.12.3.jar
[root@t2 tomcat]#ls lib/ -t |tail 
kryo-3.0.3.jar
asm-5.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
minlog-1.3.1.jar
kryo-serializers-0.45.jar
msm-kryo-serializer-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
spymemcached-2.12.3.jar
memcached-session-manager-2.3.2.jar
[root@t2 tomcat]#systemctl restart tomcat
[root@t2 tomcat]#cat /data/webapps/ROOT/index.jsp 
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<h1> tomcat website </h1>
<div>On <%=request.getServerName() %></div>
<div><%=request.getLocalAddr() + ":" + request.getLocalPort() %></div>
<div>SessionID = <span style="color:blue"><%=session.getId() %></span></div>
<%=new Date()%>
</body>
</html>
```

###### 7.3.3.3.3 查看tomcat日志

```bash
[root@t1 tomcat]#tail -n 20 logs/catalina.out
```

##### 7.3.3.4 python测试脚本

在t1上安装部署python3环境,访问memcached

```bash
[root@t1 ~]#dnf -y install python3 python3-memcached
#或者下面方式也可以安装
[root@t1 ~]#dnf -y install python3
[root@t1 ~]#pip3 install python-memcached
#准备python测试脚本
[root@t1 ~]#cat showmemcached.py 
#!/usr/bin/python3
import memcache
mc = memcache.Client(['10.0.0.101:11211','10.0.0.102:11211'], debug=True)
print('-' * 30)
#查看全部 key
#for x in mc.get_stats('items'): # stats items 返回 items:5:number 1
#   print(x)
#print('-' * 30)
for x in mc.get_stats('cachedump 5 0'):  # stats cachedump 5 0 # 5和上面的items返回
的值有关；0表示全部
   print(x)
[root@t1 ~]#chmod +x showmemcached.py
#运行python脚本
[root@t1 ~]#./showmemcached.py
('10.0.0.101:11211 (1)', {})
('10.0.0.102:11211 (1)', {})
```

##### 7.3.3.5 浏览器访问

##### 7.3.3.6 故障访问

###### 7.3.3.6.1 模拟tomcat故障

```bash
[root@t2 ~]#systemctl stop tomcat
```

运行脚本看到下面结果

```bash
[root@t1 ~]#./showmemcached.py
------------------------------
('10.0.0.101:11211 (1)', {'items:5:number': '1', 'items:5:number_hot': '0', 
'items:5:number_warm': '0', 'items:5:number_cold': '1', 'items:5:age_hot': '0', 
'items:5:age_warm': '0', 'items:5:age': '1531', 'items:5:evicted': '0', 
'items:5:evicted_nonzero': '0', 'items:5:evicted_time': '0', 
'items:5:outofmemory': '0', 'items:5:tailrepairs': '0', 'items:5:reclaimed': 
'0', 'items:5:expired_unfetched': '0', 'items:5:evicted_unfetched': '0', 
'items:5:evicted_active': '0', 'items:5:crawler_reclaimed': '0', 
'items:5:crawler_items_checked': '9', 'items:5:lrutail_reflocked': '0', 
'items:5:moves_to_cold': '2', 'items:5:moves_to_warm': '0', 
'items:5:moves_within_lru': '0', 'items:5:direct_reclaims': '0', 
'items:5:hits_to_hot': '0', 'items:5:hits_to_warm': '0', 'items:5:hits_to_cold': 
'1', 'items:5:hits_to_temp': '0'})
('10.0.0.102:11211 (1)', {})
------------------------------
('10.0.0.101:11211 (1)', {'4E92417BEFB38ED09E50433EF6F96DDF-n1.Tomcat2': '[97 b; 
1594562924 s]'})
('10.0.0.102:11211 (1)', {})
```

###### 7.3.3.6.2 模拟memcached故障

```bash
[root@t2 ~]#systemctl start tomcat
[root@t2 ~]#systemctl stop memcached.service
```

再次运行脚本

```bash
[root@t1 ~]#./showmemcached.py
------------------------------
MemCached: MemCache: inet:10.0.0.102:11211: connect: [Errno 111] Connection 
refused. Marking dead.
('10.0.0.101:11211 (1)', {'items:5:number': '2', 'items:5:number_hot': '0', 
'items:5:number_warm': '0', 'items:5:number_cold': '2', 'items:5:age_hot': '0', 
'items:5:age_warm': '0', 'items:5:age': '60', 'items:5:evicted': '0', 
'items:5:evicted_nonzero': '0', 'items:5:evicted_time': '0', 
'items:5:outofmemory': '0', 'items:5:tailrepairs': '0', 'items:5:reclaimed': 
'1', 'items:5:expired_unfetched': '1', 'items:5:evicted_unfetched': '0', 
'items:5:evicted_active': '0', 'items:5:crawler_reclaimed': '0', 
'items:5:crawler_items_checked': '11', 'items:5:lrutail_reflocked': '0', 
'items:5:moves_to_cold': '13', 'items:5:moves_to_warm': '0', 
'items:5:moves_within_lru': '0', 'items:5:direct_reclaims': '0', 
'items:5:hits_to_hot': '0', 'items:5:hits_to_warm': '0', 'items:5:hits_to_cold': 
'5', 'items:5:hits_to_temp': '0'})
------------------------------
('10.0.0.101:11211 (1)', {'63B8F0CE41AF8943351AB9B71DE174C3-n1.Tomcat1': '[97 b; 
1594564819 s]', '63B8F0CE41AF8943351AB9B71DE174C3-n1.Tomcat2': '[97 b; 
1594564785 s]'})
#看到只有10.0.0.101有信息,并且生成两个sessionID,说明当memcached2挂掉后,tomcat1只能将Session信息写入至memcached1做为备份
```

恢复memcached 

```bash
[root@t2 ~]#systemctl start memcached.service 
[root@t1 ~]#./showmemcached.py
------------------------------
('10.0.0.101:11211 (1)', {'items:5:number': '2', 'items:5:number_hot': '0', 
'items:5:number_warm': '0', 'items:5:number_cold': '2', 'items:5:age_hot': '0', 
'items:5:age_warm': '0', 'items:5:age': '23', 'items:5:evicted': '0', 
'items:5:evicted_nonzero': '0', 'items:5:evicted_time': '0', 
'items:5:outofmemory': '0', 'items:5:tailrepairs': '0', 'items:5:reclaimed': 
'1', 'items:5:expired_unfetched': '1', 'items:5:evicted_unfetched': '0', 
'items:5:evicted_active': '0', 'items:5:crawler_reclaimed': '0', 
'items:5:crawler_items_checked': '13', 'items:5:lrutail_reflocked': '0', 
'items:5:moves_to_cold': '20', 'items:5:moves_to_warm': '0', 
'items:5:moves_within_lru': '0', 'items:5:direct_reclaims': '0', 
'items:5:hits_to_hot': '0', 'items:5:hits_to_warm': '0', 'items:5:hits_to_cold': 
'5', 'items:5:hits_to_temp': '0'})
('10.0.0.102:11211 (1)', {'items:5:number': '1', 'items:5:number_hot': '0', 
'items:5:number_warm': '0', 'items:5:number_cold': '1', 'items:5:age_hot': '0', 
'items:5:age_warm': '0', 'items:5:age': '20', 'items:5:evicted': '0', 
'items:5:evicted_nonzero': '0', 'items:5:evicted_time': '0', 
'items:5:outofmemory': '0', 'items:5:tailrepairs': '0', 'items:5:reclaimed': 
'0', 'items:5:expired_unfetched': '0', 'items:5:evicted_unfetched': '0', 
'items:5:evicted_active': '0', 'items:5:crawler_reclaimed': '0', 
'items:5:crawler_items_checked': '0', 'items:5:lrutail_reflocked': '0', 
'items:5:moves_to_cold': '2', 'items:5:moves_to_warm': '0', 
'items:5:moves_within_lru': '0', 'items:5:direct_reclaims': '0', 
'items:5:hits_to_hot': '0', 'items:5:hits_to_warm': '0', 'items:5:hits_to_cold': 
'0', 'items:5:hits_to_temp': '0'})
------------------------------
('10.0.0.101:11211 (1)', {'63B8F0CE41AF8943351AB9B71DE174C3-n1.Tomcat2': '[97 b; 1594564785 s]', '63B8F0CE41AF8943351AB9B71DE174C3-n1.Tomcat1': '[97 b; 1594564819 s]'})
('10.0.0.102:11211 (1)', {'63B8F0CE41AF8943351AB9B71DE174C3-n2.Tomcat1': '[97 b; 1594564796 s]'})
```

#### 7.3.4 实战案例 2 : tomcat和memcached 分别处于不同主机

环境准备： 

* 时间同步，确保NTP或Chrony服务正常运行。 
* 防火墙规则 
* 禁用SELinux 
* 五台主机

| IP         | 主机名 | 服务       | 软件                   |
| ---------- | ------ | ---------- | ---------------------- |
| 10.0.0.100 | proxy  | 调度器     | CentOS8、Nginx、HTTPD  |
| 10.0.0.101 | t1     | tomcat1    | CentOS8、JDK8、Tomcat8 |
| 10.0.0.102 | t2     | tomcat2    | CentOS8、JDK8、Tomcat8 |
| 10.0.0.103 | m1     | memcached1 | CentOS8、memcached     |
| 10.0.0.104 | m2     | memcached2 | CentOS8、memcached     |

##### 7.3.4.1 准备proxy主机的配置，利用nginx作为反向代理

```bash
[root@proxy ~]#cat /etc/nginx/nginx.conf
http {
 ......
 upstream tomcat-server {
       server t1.magedu.org:8080;
       server t2.magedu.org:8080;
}
   server {
 ......
       location / {
       }
       location ~* \.(jsp|do)$ {
         proxy_pass http://tomcat-server;
       }
[root@proxy ~]#cat /etc/hosts
10.0.0.100 proxy.magedu.org proxy
10.0.0.101 t1.magedu.org t1
10.0.0.102 t2.magedu.org t2
#准备一台测试机(可选)
[root@centos7 ~]#cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
centos7.wangxiaochun.com
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.0.0.100  proxy.magedu.org
[root@centos7 ~]#hostname -I
10.0.0.7
```

##### 7.3.4.2 在m1和m2上分别配置memcached

```bash
#在m1和m2上做相同的配置
[root@m1 ~]#dnf -y install memcached 
[root@m1 ~]#vim /etc/sysconfig/memcached 
[root@m1 ~]#cat /etc/sysconfig/memcached 
PORT="11211"
USER="memcached"
MAXCONN="1024"
CACHESIZE="64"
#OPTIONS="-l 127.0.0.1,::1"
OPTIONS=""
[root@m1 ~]#systemctl start memcached.service 
#m2的配置和m1相同
```

##### 7.3.4.3 在t1和t2上准备tomcat

**t1 的配置**

```bash
[root@t1 tomcat]#vim conf/server.xml
 <Engine name="Catalina" defaultHost="t1.magedu.org" jvmRoute="Tomcat1">  
 ......
       <Host name="t1.magedu.org" appBase="/data/webapps" autoDeploy="true" > 
     </Host>  
   </Engine>
 </Service>
</Server>
[root@t1 tomcat]#vim conf/context.xml
.....
<Context>
......
   <Manager pathname="" />
    -->
#在最后一行前加下面内容
 <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="n1:10.0.0.103:11211,n2:10.0.0.104:11211"  
    failoverNodes="n1"
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFact
ory"
   />
</Context> #此行是最后一行
#将相关包传到lib/目录下
asm-5.2.jar
kryo-3.0.3.jar
kryo-serializers-0.45.jar
memcached-session-manager-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
minlog-1.3.1.jar
msm-kryo-serializer-2.3.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
spymemcached-2.12.3.jar
[root@t1 tomcat]#ls lib/ -t |tail
kryo-3.0.3.jar
asm-5.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
minlog-1.3.1.jar
kryo-serializers-0.45.jar
msm-kryo-serializer-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
spymemcached-2.12.3.jar
memcached-session-manager-2.3.2.jar
[root@t1 tomcat]#cat /data/webapps/ROOT/index.jsp 
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<div>On <%=request.getServerName() %></div>
<div><%=request.getLocalAddr() + ":" + request.getLocalPort() %></div>
<div>SessionID = <span style="color:blue"><%=session.getId() %></span></div>
<%=new Date()%>
</body>
</html>
[root@t1 ~]#systemctl restart tomcat
```

**t2参考上面t1做类似的配置**

```bash
[root@t2 tomcat]#vim conf/server.xml
 <Engine name="Catalina" defaultHost="t2.magedu.org" jvmRoute="Tomcat2">  
 ......
       <Host name="t2.magedu.org" appBase="/data/webapps" autoDeploy="true" > 
     </Host>  
   </Engine>
 </Service>
</Server>
[root@t2 tomcat]#vim conf/context.xml
.....
<Context>
......
   <Manager pathname="" />
    -->
#在最后一行前加下面内容
 <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="n1:10.0.0.103:11211,n2:10.0.0.104:11211"  
    failoverNodes="n2" #只修改此行,和t1不同,其它都一样
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFact
ory"
   />
</Context> #此行是最后一行
#将相关包传到lib/目录下
asm-5.2.jar
kryo-3.0.3.jar
kryo-serializers-0.45.jar
memcached-session-manager-2.3.2.jar
memcached-session-manager-tc8-2.3.2.jar
minlog-1.3.1.jar
msm-kryo-serializer-2.3.2.jar
objenesis-2.6.jar
reflectasm-1.11.9.jar
spymemcached-2.12.3.jar
[root@t2 tomcat]#cat /data/webapps/ROOT/index.jsp 
<%@ page import="java.util.*" %>
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>tomcat test</title>
</head>
<body>
<div>On <%=request.getServerName() %></div>
<div><%=request.getLocalAddr() + ":" + request.getLocalPort() %></div>
<div>SessionID = <span style="color:blue"><%=session.getId() %></span></div>
<%=new Date()%>
</body>
</html>
[root@t2 ~]#systemctl restart tomcat
```

查看日志

```bash
[root@t1 ~]#tail /usr/local/tomcat/logs/catalina.out
.......
...... INFO [t1.magedu.org-startStop-1] 
de.javakaffee.web.msm.MemcachedSessionService.startInternal --------
- finished initialization:
- sticky: true
- operation timeout: 1000
- node ids: [n2]
- failover node ids: [n1]
- storage key prefix: null
- locking mode: null (expiration: 5s)
```

##### 7.3.4.4 测试的python脚本

在proxy 上安装部署python3环境,访问memcached

```bash
[root@proxy ~]#dnf -y install python3 python3-memcached
#准备python测试脚本
[root@proxy ~]#cat showmemcached.py 
#!/usr/bin/python3
import memcache
mc = memcache.Client(['10.0.0.103:11211','10.0.0.104:11211'], debug=True)
print('-' * 30)
for x in mc.get_stats('cachedump 5 0'):  # stats cachedump 5 0 # 5和上面的items返回
的值有关；0表示全部
   print(x)
[root@proxy ~]#chmod +x showmemcached.py
#运行python脚本
[root@proxy ~]#./showmemcached.py
------------------------------
('10.0.0.103:11211 (1)', {})
('10.0.0.104:11211 (1)', {})
```

##### 7.3.4.5 浏览器访问测试

##### 7.3.4.6 模拟故障

```bash
[root@t2 ~]#systemctl stop tomcat
```

多次刷新页面,SessionID不变

```bash
[root@m2 ~]#systemctl stop memcached.service
```

运行脚本

```bash
[root@proxy ~]#./showmemcached.py
------------------------------
MemCached: MemCache: inet:10.0.0.104:11211: connect: [Errno 111] Connection 
refused. Marking dead.
('10.0.0.103:11211 (1)', {'F82C3A7C84B2E0D6B05E6E6F496F9FBC-n1.Tomcat2': '[97 b; 
1594815951 s]', 'F82C3A7C84B2E0D6B05E6E6F496F9FBC-n1.Tomcat1': '[97 b; 
1594815949 s]'})
```

### 7.4 non-sticky 模式

#### 7.4.1 non-sticky 模式工作原理

non-sticky 模式即前端tomcat和后端memcached无关联(无粘性)关系 

从msm 1.4.0之后版本开始支持non-sticky模式。 

Tomcat session为中转Session，对每一个SessionID随机选中后端的memcached节点n1(或者n2)为主 session，而另一个memcached节点n2(或者是n1)为备session。产生的新的Session会发送给主、备 memcached，并清除本地Session。

后端两个memcached服务器对一个session来说是一个是主,一个是备,但对所有session信息来说每个 memcached即是主同时也是备 

如果n1下线，n2则转正。n1再次上线，n2依然是主Session存储节点。

#### 7.4.2 memcached配置

放到 $CATALINA_HOME/conf/context.xml 中

```xml
<Context>
 ...
  <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="n1:10.0.0.101:11211,n2:10.0.0.102:11211"
    sticky="false"
    sessionBackupAsync="false"
    lockingMode="uriPattern:/path1|/path2"
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFac
tory"
    />
</Context>
```

#### 7.4.3 redis 配置

支持将session存放在Redis中,但当前对Redis的支持不允许连接到多个Redis节点,可以通过Redis的集群服务实现防止redis的单点失败 

参考文档 :  

https://github.com/ran-jit/tomcat-cluster-redis-session-manager/wiki 

https://github.com/magro/memcached-session-manager/wiki/SetupAndConfiguration#example-for-non-sticky-sessions--kryo--redis

下载 jedis.jar，放到 $CATALINA_HOME/lib/ ，对应本次安装就是/usr/local/tomcat/lib。

```bash
# yum install redis
# vim /etc/redis.conf
bind 0.0.0.0
# systemctl start redis
```

放到 $CATALINA_HOME/conf/context.xml 中

```xml
<Context>
 ...
  <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="redis://:password@redis.example.com:portnumber"
    sticky="false"
    sessionBackupAsync="false"
    lockingMode="uriPattern:/path1|/path2"
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFac
tory"
    />
</Context>
```

#### 7.4.4 实战案例: memcached 实现non-sticky模式

环境准备： 

* 时间同步，确保NTP或Chrony服务正常运行。 
* 防火墙规则 
* 禁用SELinux 
* 三台主机

| IP         | 主机名 | 服务    |                         |
| ---------- | ------ | ------- | ----------------------- |
| 10.0.0.100 | proxy  | 调度器  | Nginx、HTTPD            |
| 10.0.0.101 | t1     | Tomcat1 | JDK8、Tomcat8,memcached |
| 10.0.0.102 | t2     | Tomcat2 | JDK8、Tomcat8,memcached |

##### 7.4.4.1 修改tomcat配置

```bash
#在前面实验基础上修改,memcached配置不变,只需要修改tomcat配置
[root@t1 tomcat]#vim conf/context.xml
<Context>
 ...
 <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="n1:10.0.0.101:11211,n2:10.0.0.102:11211"
    sticky="false"               #下面三行和sticky模式不同
    sessionBackupAsync="false"
    lockingMode="uriPattern:/path1|/path2"
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFact
ory"
   />
</Context>
[root@t1 tomcat]#systemctl restart tomcat
#查看日志
[root@t1 ~]#tail -f /usr/local/tomcat/logs/catalina.out
..... INFO [t1.magedu.org-startStop-1] 
de.javakaffee.web.msm.MemcachedSessionService.startInternal --------
- finished initialization:
- sticky: false
- operation timeout: 1000
- node ids: [n1, n2]
- failover node ids: []
- storage key prefix: null
- locking mode: uriPattern:/path1|/path2 (expiration: 5s)
.......
#t2和t1相同操作
[root@t2 tomcat]#vim conf/context.xml
[root@t2 tomcat]#systemctl restart tomcat
#运行脚本查看key
[root@t1 ~]#cat ./showmemcached.py
#!/usr/bin/python3
import memcache 
mc = memcache.Client(['10.0.0.101:11211','10.0.0.102:11211'], debug=True)
print('-' * 30)
for x in mc.get_stats('cachedump 5 0'):  # stats cachedump 5 0 # 5和上面的items返回
的值有关；0表示全部
   print(x)
[root@t1 ~]#./showmemcached.py 
------------------------------
('10.0.0.101:11211 (1)', {})
('10.0.0.102:11211 (1)', {})
[root@t1 ~]#
```

##### 7.4.4.2 通过浏览器访问

```bash
#再次运行脚本后可以看到,t2为memcached的主节点,t1为备份节点
[root@t1 ~]#./showmemcached.py 
------------------------------
('10.0.0.101:11211 (1)', {'bak:D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 1594609275 s]'})
('10.0.0.102:11211 (1)', {'D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 1594609274 s]'})
```

##### 7.4.4.3 模拟故障 

**模拟t1的memcached 故障**

```bash
[root@t1 ~]#systemctl stop memcached.service 
[root@t1 ~]#./showmemcached.py 
------------------------------
MemCached: MemCache: inet:10.0.0.101:11211: connect: [Errno 111] Connection 
refused. Marking dead.
('10.0.0.102:11211 (1)', {'D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 1594609274 s]'})
[root@t1 ~]#systemctl start memcached.service 
[root@t1 ~]#./showmemcached.py 
------------------------------
('10.0.0.101:11211 (1)', {})
('10.0.0.102:11211 (1)', {'D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 1594609274 s]'})
#刷新几次页面后,再运行脚本
[root@t1 ~]#./showmemcached.py 
------------------------------
('10.0.0.101:11211 (1)', {'bak:D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': 
'[97 b; 1594609854 s]'})
('10.0.0.102:11211 (1)', {'D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 1594609274 s]'})
```

模拟t2的memcached故障

```bash
[root@t2 ~]#systemctl stop memcached.service 
[root@t1 ~]#./showmemcached.py 
------------------------------
MemCached: MemCache: inet:10.0.0.102:11211: connect: [Errno 111] Connection 
refused. Marking dead.
('10.0.0.101:11211 (1)', {'bak:D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': 
'[97 b; 1594609854 s]'})
```

```bash
#运行脚本看到在t1上生成相同的SessionID,并成为memcached新主
[root@t1 ~]#./showmemcached.py 
------------------------------
MemCached: MemCache: inet:10.0.0.102:11211: connect: [Errno 111] Connection 
refused. Marking dead.
('10.0.0.101:11211 (1)', {'D65D49C8BB068B0854F11BD1A7BFAE88-n1.Tomcat2': '[97 b; 
1594610030 s]', 'bak:D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 
1594609854 s]'})
[root@t2 ~]#systemctl start memcached.service 
[root@t1 ~]#./showmemcached.py 
------------------------------
('10.0.0.101:11211 (1)', {'D65D49C8BB068B0854F11BD1A7BFAE88-n1.Tomcat2': '[97 b; 
1594610030 s]', 'bak:D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 
1594609854 s]'})
('10.0.0.102:11211 (1)', {})
#再刷新页,运行脚本看到t2成为备用节点
[root@t1 ~]#./showmemcached.py 
------------------------------
('10.0.0.101:11211 (1)', {'D65D49C8BB068B0854F11BD1A7BFAE88-n1.Tomcat2': '[97 b; 
1594610030 s]', 'bak:D65D49C8BB068B0854F11BD1A7BFAE88-n2.Tomcat2': '[97 b; 
1594609854 s]'})
('10.0.0.102:11211 (1)', {'bak:D65D49C8BB068B0854F11BD1A7BFAE88-n1.Tomcat2': 
'[97 b; 1594610367 s]'})
```

模拟t1的tomcat故障,刷新页面,可以看到SessionID不变

```
[root@t1 ~]#systemctl stop tomcat
```

#### 7.4.5 实战案例: redis 实现 non-sticky 模式的msm

环境准备： 

* 时间同步，确保NTP或Chrony服务正常运行 
* 防火墙规则 
* 禁用SELinux 
* 四台主机

| IP         | 主机名           | 服务    | 软件包        |
| ---------- | ---------------- | ------- | ------------- |
| 10.0.0.100 | proxy            | 调度器  | Nginx、HTTPD  |
| 10.0.0.101 | t1.magedu.org    | tomcat1 | JDK8、Tomcat8 |
| 10.0.0.102 | t2.magedu.org    | tomcat2 | JDK8、Tomcat8 |
| 10.0.0.103 | redis.magedu.org | redis   | Redis         |

##### 7.4.5.1 上传redis库到tomcat服务器

```bash
[root@t1 ~]#ll /usr/local/tomcat/lib/jedis-3.0.0.jar 
-rw-r--r-- 1 root root 586620 Jun 26  2019 /usr/local/tomcat/lib/jedis-3.0.0.jar
[root@t2 ~]#ll /usr/local/tomcat/lib/jedis-3.0.0.jar 
-rw-r--r-- 1 root root 586620 Jun 26  2019 /usr/local/tomcat/lib/jedis-3.0.0.jar
```

##### 7.4.5.2 安装并配置 Redis 服务

```bash
[root@redis ~]#dnf -y install redis
[root@redis ~]#sed -i.bak 's/^bind.*/bind 0.0.0.0/' /etc/redis.conf
[root@redis ~]#systemctl enable --now redis
[root@redis ~]#ss -ntl
State             Recv-Q   Send-Q Local Address:Port Peer Address:Port           
LISTEN            0        128          0.0.0.0:22    0.0.0.0:*               
LISTEN            0        100          127.0.0.1:25  0.0.0.0:*               
LISTEN            0        128          0.0.0.0:6379  0.0.0.0:*               
LISTEN            0        128          [::]:22       [::]:*  
LISTEN            0        100          [::1]:25      [::]:* 
```

##### 7.4.5.3 修改tomcat 配置指定redis服务器地址

```bash
[root@t1 ~]#vim /usr/local/tomcat/conf/context.xml 
<Context>
 ...
 <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"
    memcachedNodes="redis://10.0.0.103:6379"     #和non-sticky的memcached相比,只修
改此行
    sticky="false"
    sessionBackupAsync="false"
    lockingMode="uriPattern:/path1|/path2"
    requestUriIgnorePattern=".*\.(ico|png|gif|jpg|css|js)$"
   
transcoderFactoryClass="de.javakaffee.web.msm.serializer.kryo.KryoTranscoderFact
ory"
   />
</Context>
[root@t1 ~]#systemctl restart tomcat
#查看日志
[root@t1 ~]#tail -f /usr/local/tomcat/logs/catalina.out
....., INFO [t1.magedu.org-startStop-1] 
de.javakaffee.web.msm.MemcachedSessionService.startInternal --------
- finished initialization:
- sticky: false
- operation timeout: 1000
- node ids: []
- failover node ids: []
- storage key prefix: null
- locking mode: uriPattern:/path1|/path2 (expiration: 5s)
.......
#t2和t1配置相同
[root@t2 ~]#vim /usr/local/tomcat/conf/context.xml
[root@t2 ~]#systemctl restart tomcat
```

##### 7.4.5.4 测试访问 

浏览器刷新访问多次,主机轮询,但SessionID不变

使用redis工具连接redis 查看SessionID

```bash
[root@redis ~]#redis-cli 
127.0.0.1:6379> KEYS *
1) "4245D5D28B2CDAB292623A01DD5D2C83.Tomcat2"
2) "validity:4245D5D28B2CDAB292623A01DD5D2C83.Tomcat2"
127.0.0.1:6379> GET 4245D5D28B2CDAB292623A01DD5D2C83.Tomcat2
"\x00\x02\x00\\\x00\x00\x01sR\xf6\x8b@\x00\x00\x01sR\xf6\x8b@\x00\x00\a\b11\x00\
x00\x01sR\xf6\x8b@\x00\x00\x01sR\xf6\x8bA\x00(4245D5D28B2CDAB292623A01DD5D2C83.T
omcat2\x00\x00\x00\x00\x00\x00\x00\x00\x01\x00"
127.0.0.1:6379>
```

##### 7.4.5.5 模拟故障 

###### 7.4.5.5.1 模拟tomcat故障,查看是否SessionID变化

```bash
[root@t2 ~]#systemctl stop tomcat
```

刷新页面,可以看到SessinID不变

###### 7.4.5.5.2 模拟Redis故障 

模拟Redis 故障,再刷新页面,可以看到SessionID不断变化,说明redis存在单点故障

```bash
[root@t1 ~]#systemctl start tomcat
[root@redis ~]#systemctl stop redis
```

### 7.5 Session 问题方案总结

通过多组实验，使用不同技术实现了session持久机制 

1. session绑定，基于IP或session cookie的。其部署简单，尤其基于session黏性的方式，粒度小， 对负载均衡影响小。但一旦后端服务器有故障，其上的session丢失。 
2. session复制集群，基于tomcat实现多个服务器内共享同步所有session。此方法可以保证任意一台 后端服务器故障，其余各服务器上还都存有全部session，对业务无影响。但是它基于多播实现心 跳，TCP单播实现复制，当设备节点过多，这种复制机制不是很好的解决方案。且并发连接多的时 候，单机上的所有session占据的内存空间非常巨大，甚至耗尽内存。 
3. session服务器，将所有的session存储到一个共享的内存空间中，使用多个冗余节点保存 session，这样做到session存储服务器的高可用，且占据业务服务器内存较小。是一种比较好的解 决session持久的解决方案。 

以上的方法都有其适用性。生产环境中，应根据实际需要合理选择。 

不过以上这些方法都是在内存中实现了session的保持，可以使用数据库或者文件系统，把session数据 存储起来，持久化。这样服务器重启后，也可以重新恢复session数据。不过session数据是有时效性 的，是否需要这样做，视情况而定。

## 8 Tomcat 性能优化

在目前流行的互联网架构中，Tomcat在目前的网络编程中是举足轻重的，由于Tomcat的运行依赖于 JVM，从虚拟机的角度把Tomcat的调整分为外部环境调优 JVM 和Tomcat 自身调优两部分

### 8.1 JVM组成

```bash
[root@t1 ~]#java -version
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```

#### 8.1.1 JVM组成

JVM 组成部分 

* 类加载子系统: 使用Java语言编写.java Source Code文件，通过javac编译成.class Byte Code文 件。class loader类加载器将所需所有类加载到内存，必要时将类实例化成实例 
* 运行时数据区: 最消耗内存的空间,需要优化 
* 执行引擎: 包括JIT (JustInTimeCompiler)即时编译器, GC垃圾回收器 
* 本地方法接口: 将本地方法栈通过JNI(Java Native Interface)调用Native Method Libraries, 比 如:C,C++库等,扩展Java功能,融合不同的编程语言为Java所用

JVM运行时数据区域由下面部分构成：

* **Method Area (线程共享)**：方法区是所有线程共享的内存空间，存放已加载的类信息(构造方法,接 口定义),常量(final),静态变量(static), 运行时常量池等。但实例变量存放在堆内存中. 从JDK8开始此 空间由永久代改名为元空间 
* **heap (线程共享)**：堆在虚拟机启动时创建,存放创建的所有对象信息。如果对象无法申请到可用内 存将抛出OOM异常.堆是靠GC垃圾回收器管理的,通过-Xmx -Xms 指定最大堆和最小堆空间大小 
* **Java stack (线程私有)**：Java栈是每个线程会分配一个栈，存放java中8大基本数据类型,对象引用, 实例的本地变量,方法参数和返回值等,基于FILO()（First In Last Out）,每个方法为一个栈帧 
* **Program Counter Register (线程私有)**：PC寄存器就是一个指针,指向方法区中的方法字节码,每 一个线程用于记录当前线程正在执行的字节码指令地址。由执行引擎读取下一条指令.因为线程需要 切换，当一个线程被切换回来需要执行的时候，知道执行到哪里了 
* **Native Method stack (线程私有)**：本地方法栈为本地方法执行构建的内存空间，存放本地方法 执行时的局部变量、操作数等。 所谓本地方法，使用native 关健字修饰的方法,比如:Thread.sleep方法. 简单的说是非Java实现的方 法，例如操作系统的C编写的库提供的本地方法，Java调用这些本地方法接口执行。但是要注意， 本地方法应该避免直接编程使用，因为Java可能跨平台使用，如果用了Windows API，换到了 Linux平台部署就有了问题

#### 8.1.2 虚拟机

目前Oracle官方使用的是HotSpot， 它最早由一家名为"Longview Technologies"公司设计，使用了很 多优秀的设计理念和出色的性能，1997年该公司被SUN公司收购。后来随着JDK一起发布了HotSpot  VM。目前HotSpot是最主要的 JVM。 

安卓程序需要运行在JVM上，而安卓平台使用了Google自研的Java虚拟机——Dalvid，适合于内存、处理器能力有限系统。

### 8.2 GC (Garbage Collection) 垃圾收集器

在堆内存中如果创建的对象不再使用,仍占用着内存,此时即为垃圾.需要即使进行垃圾回收,从而释放内存空间给其它对象使用 

其实不同的开发语言都有垃圾回收问题,C,C++需要程序员人为回收,造成开发难度大,容易出错等问题,但 执行效率高,而JAVA和Python中不需要程序员进行人为的回收垃圾,而由JVM或相关程序自动回收垃圾,减轻程序员的开发难度,但可能会造成执行效率低下 

堆内存里面经常创建、销毁对象，内存也是被使用、被释放。如果不妥善处理，一个使用频繁的进程， 可能会出现虽然有足够的内存容量，但是无法分配出可用内存空间，因为没有连续成片的内存了，内存全是碎片化的空间。 

所以需要有合适的垃圾回收机制,确保正常释放不再使用的内存空间,还需要保证内存空间尽可能的保持一 定的连续 

对于垃圾回收,需要解决三个问题

* 哪些是垃圾要回收 
* 怎么回收垃圾 
* 什么时候回收垃圾

#### 8.2.1 Garbage 垃圾确定方法

* 引用计数: 每一个堆内对象上都与一个私有引用计数器，记录着被引用的次数，引用计数清零，该对象所占用堆内存就可以被回收。循环引用的对象都无法将引用计数归零，就无法清除。Python中即使用此种方式 
* 根搜索(可达)算法 Root Searching

#### 8.2.2 垃圾回收基本算法

##### 8.2.2.1 标记-清除 Mark-Sweep

分垃圾标记阶段和内存释放阶段。标记阶段，找到所有可访问对象打个标记。清理阶段，遍历整个堆， 对未标记对象(即不再使用的对象)逐一进行清理。

标记-清除最大的问题会造成内存碎片,但是不浪费空间,效率较高(如果对象较多,逐一删除效率也会影响)

##### 8.2.2.2 标记-压缩 (压实)Mark-Compact

分垃圾标记阶段和内存整理阶段。标记阶段，找到所有可访问对象打个标记。内存清理阶段时，整理时将对象向内存一端移动，整理后存活对象连续的集中在内存一端。

标记-压缩算法好处是整理后内存空间连续分配，有大段的连续内存可分配，没有内存碎片。 缺点是内存整理过程有消耗,效率相对低下

##### 8.2.2.3 复制 Copying

先将可用内存分为大小相同两块区域A和B，每次只用其中一块，比如A。当A用完后，则将A中存活的对 象复制到B。复制到B的时候连续的使用内存，最后将A一次性清除干净。 

缺点是比较浪费内存，只能使用原来一半内存，因为内存对半划分了，复制过程毕竟也是有代价。 

好处是没有碎片，复制过程中保证对象使用连续空间,且一次性清除所有垃圾,所以效率很高

##### 8.2.2.4 多种算法总结

没有最好的算法,在不同场景选择最合适的算法 

* 效率: 复制算法>标记清除算法> 标记压缩算法 
* 内存整齐度: 复制算法=标记压缩算法> 标记清除算法 
* 内存利用率: 标记压缩算法=标记清除算法>复制算法

##### 8.2.2.5 STW

对于大多数垃圾回收算法而言，GC线程工作时，停止所有工作的线程，称为Stop The World。GC 完成时，恢复其他工作线程运行。这也是JVM运行中最头疼的问题。

#### 8.2.3 分代堆内存GC策略

上述垃圾回收算法都有优缺点，能不能对不同数据进行区分管理，不同分区对数据实施不同回收策略， 分而治之。

##### 8.2.3.1 堆内存分代 

将heap内存空间分为三个不同类别: 年轻代、老年代、持久代

Heap堆内存分为 

* 年轻代Young：Young Generation 
  * 伊甸园区eden: 只有一个,刚刚创建的对象 
  * 幸存(存活)区Servivor Space：有2个幸存区，一个是from区，一个是to区。大小相等、地位 相同、可互换。 
    * from 指的是本次复制数据的源区 
    * to 指的是本次复制数据的目标区
* 老年代Tenured：Old Generation, 长时间存活的对象

**默认空间大小比例:**

永久代：JDK1.7之前使用, 即Method Area方法区,保存JVM自身的类和方法,存储JAVA运行时的环境信息,  JDK1.8后 改名为 MetaSpace,此空间不存在垃圾回收,关闭JVM会释放此区域内存,此空间物理上不属于 heap内存,但逻辑上存在于heap内存 

* 永久代必须指定大小限制,字符串常量JDK1.7存放在永久代,1.8后存放在heap中 
* MetaSpace 可以设置,也可不设置,无上限

**规律: 一般情况99%的对象都是临时对象** 

范例: 在tomcat 状态页可以看到以下的内存分代

范例: 查看JVM内存分配情况

```bash
[root@centos8 ~]#cat Heap.java
public class Heap {
   public static void main(String[] args){
       //返回JVM试图使用的最大内存,字节单位
       long max = Runtime.getRuntime().maxMemory();
       //返回JVM初始化总内存
       long total = Runtime.getRuntime().totalMemory();
       System.out.println("max="+max+"字节\t"+(max/(double)1024/1024)+"MB");
       System.out.println("total="+total+"字节\t"+
(total/(double)1024/1024)+"MB");
   }
}
[root@centos8 ~]#javac Heap.java
[root@centos8 ~]#java -classpath . Heap
max=243269632字节 232.0MB
total=16252928字节 15.5MB
[root@centos8 ~]#java -XX:+PrintGCDetails Heap
max=243269632字节 232.0MB
total=16252928字节 15.5MB
Heap
 def new generation   total 4928K, used 530K [0x00000000f1000000, 0x00000000f1550000, 0x00000000f6000000)
 eden space 4416K,  12% used [0x00000000f1000000, 0x00000000f1084a60, 0x00000000f1450000)
 from space 512K,   0% used [0x00000000f1450000, 0x00000000f1450000, 0x00000000f14d0000)
 to   space 512K,   0% used [0x00000000f14d0000, 0x00000000f14d0000, 0x00000000f1550000)
 tenured generation   total 10944K, used 0K [0x00000000f6000000, 0x00000000f6ab0000, 0x0000000100000000)
   the space 10944K,   0% used [0x00000000f6000000, 0x00000000f6000000, 0x00000000f6000200, 0x00000000f6ab0000)
 Metaspace       used 2525K, capacity 4486K, committed 4864K, reserved 1056768K
 class space   used 269K, capacity 386K, committed 512K, reserved 1048576K
  
[root@centos8 ~]#echo "scale=2;(4928+10944)/1024" |bc
15.50
#说明年轻代+老年代占用了所有heap空间, Metaspace实际不占heap空间,逻辑上存在于Heap
#默认JVM试图分配最大内存的总内存的1/4,初始化默认总内存为总内存的1/64
```

##### 8.2.3.2 年轻代回收 Minor GC

1. 起始时，所有新建对象(特大对象直接进入老年代)都出生在eden，当eden满了，启动GC。这个称 为Young GC 或者 Minor GC。 
2. 先标记eden存活对象，然后将存活对象复制到s0（假设本次是s0，也可以是s1，它们可以调 换），eden剩余所有空间都清空。GC完成。 
3. 继续新建对象，当eden再次满了，启动GC。 
4. 先同时标记eden和s0中存活对象，然后将存活对象复制到s1。将eden和s0清空,此次GC完成 
5. 继续新建对象，当eden满了，启动GC。 
6. 先标记eden和s1中存活对象，然后将存活对象复制到s0。将eden和s1清空,此次GC完成

以后就重复上面的步骤。 

通常场景下,大多数对象都不会存活很久，而且创建活动非常多，新生代就需要频繁垃圾回收。 

但是，如果一个对象一直存活，它最后就在from、to来回复制，如果from区中对象复制次数达到阈值 (默认15次,CMS为6次,可通过java的选项 -XX:MaxTenuringThreshold=N 指定)，就直接复制到老年代。

##### 8.2.3.3 老年代回收 Major GC

进入老年代的数据较少，所以老年代区被占满的速度较慢，所以垃圾回收也不频繁。 

如果老年代也满了,会触发老年代GC,称为Old GC或者 Major GC。 

由于老年代对象一般来说存活次数较长，所以较常采用标记-压缩算法。 

当老年代满时,会触发 Full GC,即对所有"代"的内存进行垃圾回收 

Minor GC比较频繁，Major GC较少。但一般Major GC时，由于老年代对象也可以引用新生代对象，所 以先进行一次Minor GC，然后在Major GC会提高效率。可以认为回收老年代的时候完成了一次Full  GC。 

所以可以认为 MajorGC = FullGC

##### 8.2.3.4 GC 触发条件

**Minor GC 触发条件**：当eden区满了触发

**Full GC 触发条件**： 

* 老年代满了 
* System.gc()手动调用。不推荐

**年轻代:**

* 存活时长低 
* 适合复制算法

**老年代:**

* 区域大,存活时长高 
* 适合标记压缩算法

#### 8.2.4 java 内存调整相关参数 

##### 8.2.4.1 JVM 内存常用相关参数 

Java 命令行参考文档: https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html

帮助：man java 

选项分类 

* -选项名称 此为标准选项,所有HotSpot都支持 
* -X选项名称 此为稳定的非标准选项 
* -XX:选项名称 非标准的不稳定选项，下一个版本可能会取消

| 参数                | 说明                                                         | 举例                                                   |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| -Xms                | 设置应用程序初始使用的堆内存大小（年轻代 +老年代）           | -Xms2g                                                 |
| -Xmx                | 设置应用程序能获得的最大堆内存 早期JVM不建议超过32G，内存管理效率下降 | -Xms4g                                                 |
| -XX:NewSize         | 设置初始新生代大小                                           | -XX:NewSize=128m                                       |
| -XX:MaxNewSize      | 设置最大新生代内存空间                                       | \- XX:MaxNewSize=256m                                  |
| -Xmnsize            | 同时设置-XX:NewSize 和 -XX:MaxNewSize，代替两者              | -Xmn1g                                                 |
| -XX:NewRatio        | 同时设置-XX:NewSize 和 -XX:MaxNewSize，代替两者              | -XX:NewRatio=2 new/old=1/2                             |
| \- XX:SurvivorRatio | 以比例方式设置eden和survivor(S0或S1)                         | -XX:SurvivorRatio=6 eden/survivor=6/1 new/survivor=8/1 |
| -Xss                | 设置每个线程私有的栈空间大小,依据具体线程 大小和数量         | -Xss256k                                               |

范例: 查看java的选项帮助

```bash
#查看java命令标准选项
[root@centos ~]#java 
Usage: java [-options] class [args...]
           (to execute a class)
   or java [-options] -jar jarfile [args...]
           (to execute a jar file)
where options include:
    -d32 use a 32-bit data model if available
    -d64 use a 64-bit data model if available
    -server to select the "server" VM
                 The default VM is server.

#查看java的非标准选项
[root@centos8 ~]#java -X
    -Xmixed           mixed mode execution (default)
    -Xint             interpreted mode execution only
    -Xbootclasspath:<directories and zip/jar files separated by :>
                      set search path for bootstrap classes and resources
    -Xbootclasspath/a:<directories and zip/jar files separated by :>
                     append to end of bootstrap class path
                     
#查看所有不稳定选项的当前生效值
[root@centos8 ~]#java -XX:+PrintFlagsFinal
[Global flags]
     intx ActiveProcessorCount                      = -1                         
        {product}
   uintx AdaptiveSizeDecrementScaleFactor          = 4                         
          {product}
          
#查看当前命令行的使用的选项设置
[root@centos8 ~]#java -XX:+PrintCommandLineFlags
-XX:InitialHeapSize=15598528 -XX:MaxHeapSize=249576448 -
XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops 
-XX:+UseParallelGC
#上面的-XX:+UseParallelGC 说明当前使用Parallel Scavenge + Parallel Old
```

范例: 查看和指定JVM内存分配

```bash
#默认JVM试图分配最大内存的总内存的1/4,初始化默认总内存为总内存的1/64
[root@centos8 ~]#cat Heap.java
public class Heap {
   public static void main(String[] args){
       //返回JVM试图使用的最大内存,字节单位
       long max = Runtime.getRuntime().maxMemory();
       //返回JVM初始化总内存
       long total = Runtime.getRuntime().totalMemory();
       System.out.println("max="+max+"字节\t"+(max/(double)1024/1024)+"MB");
       System.out.println("total="+total+"字节\t"+
(total/(double)1024/1024)+"MB");
   }
}
#编译生成class文件
[root@centos8 ~]#javac Heap.java
#通过$CLASSPATH指定类文件路径,否则无法找到类,也可以通过 java -cp /path指定类路径
[root@centos8 ~]#echo $CLASSPATH
/usr/local/jdk/lib/:/usr/local/jdk/jre/lib/
[root@centos8 ~]#cp Heap.class /usr/local/jdk/lib
#查看当前内存默认值
[root@centos8 ~]#java -XX:+PrintGCDetails Heap
max=243269632字节 232.0MB
total=16252928字节 15.5MB
Heap
 def new generation   total 4928K, used 530K [0x00000000f1000000, 
0x00000000f1550000, 0x00000000f6000000)

#指定内存空间
[root@centos8 ~]#java -Xms1024m -Xmx1024m -XX:+PrintGCDetails Heap
max=1037959168字节 989.875MB
total=1037959168字节 989.875MB
Heap
 def new generation   total 314560K, used 11185K [0x00000000c0000000, 
0x00000000d5550000, 0x00000000d5550000)
#以下计算结果和max一样,说明Metaspace逻辑存在,但物理上并不属于heap空间
[root@centos8 ~]#echo '(314560+699072)*1024'|bc
1037959168 
```

范例: 查看OOM

```bash
[root@centos8 ~]#cat HeapOom2.java
import java. util. Random;
public class HeapOom2 {
 public static void main(String[] args) {
 String str = "I am lao wang";
 while (true){
 str += str + new Random().nextInt(88888888); //生成0到88888888之间的随
机数字
 }
 }
}
[root@centos8 ~]#javac -cp . HeapOom2.java
[root@centos8 ~]#java -Xms100m -Xmx100m -XX:+PrintGCDetails -cp . HeapOom2
```

##### 8.2.4.2 JDK 工具监控使用情况

###### 8.2.4.2.1 案例1: jvisualvm工具 

范例: 指定参数运行Java程序

```bash
#java -cp . -Xms512m -Xmx1g HelloWorld
```

```
#测试用java程序
javac HelloWorld.java
java -classpath . -Xms512m -Xmx1g HelloWorld
[root@tomcat ~]#cat HelloWorld.java
#编译为字节码.class文件
[root@tomcat ~]#javac HelloWorld.java
[root@tomcat ~]#ll HelloWorld.class 
-rw-r--r-- 1 root root 590 Jul 13 16:31 HelloWorld.class
[root@tomcat ~]#file HelloWorld.class 
HelloWorld.class: compiled Java class data, version 52.0 (Java 1.8)
#指定路径运行class文件
[root@tomcat ~]#java -cp . -Xms512m -Xmx1g HelloWorld
hello magedu
hello magedu
hello magedu
#或者用下面方法指定CLASS文件的搜索路径
[root@tomcat ~]#echo $CLASSPATH
/usr/local/jdk/lib/:/usr/local/jdk/jre/lib/
[root@tomcat ~]#mv HelloWorld.class /usr/local/jdk/lib/
#分别执行多次观察
[root@tomcat ~]#java HelloWorld 
[root@tomcat ~]#java -Xms256m -Xmx512m HelloWorld 
[root@tomcat ~]#java -Xms128m -Xmx512m -XX:NewSize=64m -XX:MaxNewSize=200m 
HelloWorld 
hello magedu
[root@tomcat ~]#jps
21299 Main
21418 Jps
21407 HelloWorld
#将Linux的图形工具显示到windows桌面
#方法1
#注意:先在windows上开启Xwindows Server，如Xmanager
[root@tomcat ~]#export DISPLAY=10.0.0.1:0.0
#方法2:使用 MobaXterm 连接
[root@tomcat ~]#yum -y install xorg-x11-xauth xorg-x11-fonts-* xorg-x11-fontutils xorg-x11-fonts-Type1 
[root@tomcat ~]#exit
#运行图形工具
[roottomcat ~]#which jvisualvm 
/usr/local/jdk/bin/jvisualvm
[root@tomcat ~]#jvisualvm 
```

###### 8.2.4.2.2 案例2: 使用 jvisualvm的 Visual GC 插件

```bash
[root@centos8 ~]#cat HeapOom.java 
import java.util.ArrayList;
import java.util.List;
public class HeapOom {
   public static void main(String[] args) {
       List<byte[]> list =new ArrayList<byte[]>();
       int i = 0;
       boolean flag = true;
        while(flag){
           try{
               i++;
               list.add(new byte[1024* 1024]);//每次增加一个1M大小的数组对象
               Thread.sleep(1000);
           }catch(Throwable e){
               e.printStackTrace();
               flag =false;
               System.out.println("count="+i);//记录运行的次数
           }
       }
   }
}
```

运行上面的OOM程序,重新运行jvisualm, 可以看到多了一下VisualGC页标,下面的图示

```bash
[root@centos8 ~]#java -Xms100m -Xmx200m HeapOom
```

当OOM时退出JAVA程序时,会显示下面图示

```bash
[root@centos8 ~]#java HeapOom 
java.lang.OutOfMemoryError: Java heap space
 at HeapOom.main(HeapOom.java:12)
count=229
```

###### 8.2.4.2.3 Jprofiler定位OOM的问题原因

JProfiler是一款功能强大的Java开发分析工具，它可以快速的帮助用户分析出存在的错误，软件还可对需 要的显示类进行标记，包括了内存的分配情况和信息的视图等 

JProfiler官网：http://www.ej-technologies.com/products/jprofiler/overview.html

范例: 安装jprofiler工具定位OOM原因和源码问题的位置

```
#安装OpenJDK
[root@centos8 ~]#dnf -y install java-1.8.0-openjdk-devel
[root@centos8 ~]#cat HeapOom.java 
[root@centos8 ~]#javac HeapOom.java 
[root@centos8 ~]#java -cp . -Xms5m -Xmx10m -XX:+HeapDumpOnOutOfMemoryError HeapOom
[root@centos8 ~]#cat HeapOom2.java 
import java. util. Random;
public class HeapOom2 {
	public static void main(String[] args) {
		String str = "I am lao wang";
		while (true){
			str += str + new Random().nextInt(88888888);
		}
	}
}
[root@centos8 ~]#javac HeapOom2.java 
[root@centos8 ~]#java -cp . -Xms5m -Xmx10m -XX:+HeapDumpOnOutOfMemoryError HeapOom2
```

下载并安装JProfiler(傻瓜式安装略)后,双击打开前面生成的两个文件java_pid96271.hprof和 java_pid96339,可以看到下面显示,从中分析OOM原因

##### 8.2.4.3 Tomcat的JVM参数设置

默认不指定，-Xmx大约使用了1/4的内存，当前本机内存指定约为1G。 

在bin/catalina.sh中增加一行

```bash
......
# OS specific support. $var _must_ be set to either true or false.
#添加下面一行
JAVA_OPTS="-server -Xms128m -Xmx512m -XX:NewSize=48m -XX:MaxNewSize=200m"
                                                            
cygwin=false
darwin=false
........
```

```bash
-server：VM运行在server模式，为在服务器端最大化程序运行速度而优化
-client：VM运行在Client模式，为客户端环境减少启动时间而优化
```

重启 Tomcat 观察

```bash
[root@tomcat ~]#ps aux|grep tomcat
tomcat    22194  5.1 15.1 2497008 123480 ?     Sl   13:31   0:03 
/usr/local/jdk/jre/bin/java -
Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties -
Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -server -
Xmx512m -Xms128m -XX:NewSize=48m -XX:MaxNewSize=200m -
Djdk.tls.ephemeralDHKeySize=2048 -
Djava.protocol.handler.pkgs=org.apache.catalina.webresources -
Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -
Dignore.endorsed.dirs= -classpath
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar -
Dcatalina.base=/usr/local/tomcat -Dcatalina.home=/usr/local/tomcat -
Djava.io.tmpdir=/usr/local/tomcat/temp org.apache.catalina.startup.Bootstrap start
```

#### 8.2.5 垃圾收集方式

按工作模式不同：指的是GC线程和工作线程是否一起运行 

* 独占垃圾回收器：只有GC在工作，STW 一直进行到回收完毕，工作线程才能继续执行 
* 并发垃圾回收器：让GC线程垃圾回收某些阶段可以和工作线程一起进行,如:标记阶段并行,回收阶段仍然串行

按回收线程数：指的是GC线程是否串行或并行执行 

* 串行垃圾回收器：一个GC线程完成回收工作 
* 并行垃圾回收器：多个GC线程同时一起完成回收工作，充分利用CPU资源

#### 8.2.6 调整策略

对JVM调整策略应用极广 

* 在WEB领域中Tomcat等 
* 在大数据领域Hadoop生态各组件 
* 在消息中间件领域的Kafka等 
* 在搜索引擎领域的ElasticSearch、Solr等

注意: 在不同领域和场景对JVM需要不同的调整策略 

* 减少 STW 时长，串行变并行 
* 减少 GC 次数，要分配合适的内存大小

一般情况下，大概可以使用以下原则： 

* 客户端或较小程序，内存使用量不大，可以使用串行回收 
* 对于服务端大型计算，可以使用并行回收 
* 大型WEB应用，用户端不愿意等待，尽量少的STW，可以使用并发回收

#### 8.2.7 垃圾回收器

##### 8.2.7.1 常用垃圾回收器

###### 8.2.7.1.1 按分代设置不同垃圾回收器

新生代 

* 新生代串行收集器Serial：单线程、独占式串行，采用复制算法,简单高效但会造成STW

* 新生代并行回收收集器PS(Parallel Scavenge)：多线程并行、独占式，会产生STW, 使用复制算法 关注调整吞吐量,此收集器关注点是达到一个可控制的吞吐量 

  吞吐量 = 运行用户代码时间/（运行用户代码时间+垃圾收集时间），比如虚拟机总共运行100分 钟，其中垃圾回收花掉1分钟，那吞吐量就是99%。 

  高吞吐量可以高效率利用CPU时间，尽快完成运算任务，主要适合在后台运算而不需要太多交互的任务。 

  除此之外，Parallel Scavenge 收集器具有自适应调节策略，它可以将内存管理的调优任务交给虚 拟机去完成。自适应调节策略也是Parallel Scavenge与 ParNew 收集器的一个重要区别。 

  和ParNew不同,PS不可以和老年代的CMS组合

* 新生代并行收集器ParNew：就是Serial 收集器的多线程版,将单线程的串行收集器变成了多线程并行、独占式,使用复制算法,相当于PS的改进版 

  经常和CMS配合使用,关注点是尽可能地缩短垃圾收集时用户线程的停顿时间，适合需要与用户交 互的程序，良好的响应速度能提升用户体验

老年代：

* 老年代串行收集器Serial Old：Serial Old是Serial收集器的老年代版本,单线程、独占式串行，回收 算法使用标记压缩 

* 老年代并行回收收集器Parallel Old：多线程、独占式并行，回收算法使用标记压缩，关注调整吞吐量 

  Parallel Old收集器是Parallel Scavenge 收集器的老年代版本，这个收集器是JDK1.6之后才开始提 供，从HotSpot虚拟机的垃圾收集器的图中也可以看出，Parallel Scavenge 收集器无法与CMS收集 器配合工作,因为一个是为了吞吐量，一个是为了客户体验（也就是暂停时间的缩短）

* CMS (Concurrent Mark Sweep并发标记清除算法) 收集器

  * 在某些阶段尽量使用和工作线程一起运行，减少STW时长(200ms以内), 提升响应速度,是互联 网服务端BS系统上较佳的回收算法 
  * 分为4个阶段：初始标记、并发标记、重新标记、并发清除，在初始标记、重新标记时需要 STW。
  * 初始标记：此过程需要STW（Stop The Word），只标记一下GC Roots能直接关联到的对象，速度很快。 
  * 并发标记：就是GC Roots进行扫描可达链的过程，为了找出哪些对象需要收集。这个过程远 远慢于初始标记，但它是和用户线程一起运行的，不会出现STW，所有用户并不会感受到。 
  * 重新标记：为了修正在并发标记期间，用户线程产生的垃圾，这个过程会比初始标记时间稍微长一点，但是也很快，和初始标记一样会产生STW。 
  * 并发清理：在重新标记之后，对现有的垃圾进行清理，和并发标记一样也是和用户线程一起运 行的，耗时较长（和初始标记比的话），不会出现STW。 
  * 由于整个过程中，耗时最长的并发标记和并发清理都是与用户线程一起执行的，所以总体上来 说，CMS收集器的内存回收过程是与用户线程一起并发执行的。

###### 8.2.7.1.2 以下收集器不再按明确的分代单独设置

* G1(Garbage First)收集器
  * 是最新垃圾回收器，从JDK1.6实验性提供，JDK1.7发布，其设计目标是在多处理器、大内存 服务器端提供优于CMS收集器的吞吐量和停顿控制的回收器。JDK9将G1设为默认的收集器,建议JDK9版本以后使用。 
  * 基于标记压缩算法,不会产生大量的空间碎片，有利于程序的长期执行。 
  * 分为4个阶段：初始标记、并发标记、最终标记、筛选回收。并发标记并发执行，其它阶段 STW只有GC线程并行执行。 
  * G1收集器是面向服务端的收集器，它的思想就是首先回收尽可能多的垃圾（这也是GarbageFirst名字的由来） 
  * G1能充分的利用多CPU，多核环境下的硬件优势，使用多个CPU来缩短STW停顿的时间 (10ms以内)。 
  * 可预测的停顿：这是G1相对于CMS的另一大优势，G1和CMS一样都是关注于降低停顿时间， 但是G1能够让使用者明确的指定在一个M毫秒的时间片段内，消耗在垃圾收集的时间不得超过N毫秒。 
  * 通过此选项指定: +UseG1GC
* ZGC收集器: 减少SWT时长(1ms以内), 可以PK C++的效率,目前实验阶段 
* Shenandoah收集器: 和ZGC竞争关系,目前实验阶段 
* Epsilon收集器: 调试 JDK 使用,内部使用,不用于生产环境

JVM 1.8 默认的垃圾回收器：PS + ParallelOld,所以大多数都是针对此进行调优

##### 8.2.7.2 垃圾收集器设置

优化调整Java 相关参数的目标: 尽量减少FullGC和STW 

通过以下选项可以单独指定新生代、老年代的垃圾收集器 

* -server 指定为Server模式,也是默认值,一般使用此工作模式 

* -XX:+UseSerialGC 

  * 运行在Client模式下，新生代是Serial, 老年代使用SerialOld 

* -XX:+UseParNewGC 

  * 新生代使用ParNew，老年代使用SerialOld

* -XX:+UseParallelGC  

  * 运行于server模式下，新生代使用Serial Scavenge, 老年代使用SerialOld

* -XX:+UseParallelOldGC  

  * 新生代使用Paralell Scavenge, 老年代使用Paralell Old
  *  -XX:ParallelGCThreads=N，在关注吞吐量的场景使用此选项增加并行线程数

* -XX:+UseConcMarkSweepGC 

  * 新生代使用ParNew, 老年代优先使用CMS，备选方式为Serial Old 
  * 响应时间要短，停顿短使用这个垃圾收集器 
  * -XX:CMSInitiatingOccupancyFraction=N，N为0-100整数表示达到老年代的大小的百分比多少触发回收 
    * 默认68
  * -XX:+UseCMSCompactAtFullCollection 开启此值,在CMS收集后，进行内存碎片整理 
  * -XX:CMSFullGCsBeforeCompaction=N 设定多少次CMS后，进行一次内存碎片整理 
  * -XX:+CMSParallelRemarkEnabled 降低标记停顿

  范例: 

  ```bash
  -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=80 -
  XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=5
  ```

范例: 查看默认模式

```bash
[root@centos8 ~]#java |& grep '-server'
    -server to select the "server" VM
                 The default VM is server.
[root@centos8 ~]#tail -n 2 /usr/local/jdk/jre/lib/amd64/jvm.cfg
-server KNOWN
-client IGNORE
```

范例: 指定垃圾回收设置

```bash
#将参数加入到bin/catalina.sh中，重启观察Tomcat status。老年代已经使用CMS
[root@tomcat ~]#vim /usr/local/tomcat/bin/catalina.sh
......
# OS specific support. $var _must_ be set to either true or false.
JAVA_OPTS="-server -Xmx512m -Xms128m -XX:NewSize=48m -XX:MaxNewSize=200m -
XX:+UseConcMarkSweepGC -XX:+UseCMSCompactAtFullCollection -
XX:CMSFullGCsBeforeCompaction=5"                                                 
                                                          
cygwin=false
darwin=false
os400=false
.......
[root@tomcat ~]#systemctl restart tomcat
[root@tomcat ~]#ps aux |grep tomcat
tomcat     22093  2.2 15.2 3018616 148448 ?     Sl   17:51   0:06 
/usr/local/jdk/bin/java -
Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties -
Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -server -
Xmx512m -Xms128m -XX:NewSize=48m -XX:MaxNewSize=200m -XX:+UseConcMarkSweepGC -
XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=5 -
Djdk.tls.ephemeralDHKeySize=2048 -
Djava.protocol.handler.pkgs=org.apache.catalina.webresources -
Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -
Dignore.endorsed.dirs= -classpath
/usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar -
Dcatalina.base=/usr/local/tomcat -Dcatalina.home=/usr/local/tomcat -
Djava.io.tmpdir=/usr/local/tomcat/temp org.apache.catalina.startup.Bootstrap 
start
root       22158  0.0  0.1  12108  1084 pts/1   S+   17:56   0:00 grep --color=auto tomcat
```

开启垃圾回收输出统计信息,适用于调试环境的相关选项 

* -XX:+PrintGC 输出GC信息 -XX:+PrintGCDetails 输出GC详细信息 
* -XX:+PrintGCTimeStamps 与前两个组合使用，在信息上加上一个时间戳 
* -XX:+PrintHeapAtGC 生成GC前后椎栈的详细信息，日志会更大 

注意: 以上适用调试环境，生产环境请移除这些参数，否则有非常多的日志输出

#### 8.2.8 JAVA参数总结

| 参数名称                      | 含义                                                         | 默认值                |                                                              |
| ----------------------------- | ------------------------------------------------------------ | --------------------- | ------------------------------------------------------------ |
| -Xms                          | 初始堆大小                                                   | 物理内存的 1/64(<1GB) | 默认(MinHeapFreeRatio参 数可以调整)空余堆内存小于 40%时，JVM就会增大堆直 到-Xmx的最大限制. |
| -Xmx                          | 最大堆大小                                                   | 物理内存的 1/4(<1GB)  | 默认(MaxHeapFreeRatio参 数可以调整)空余堆内存大于 70%时，JVM会减少堆直到 - Xms的最小限制 |
| -Xmn                          | 年轻代大小 (1.4or lator)                                     |                       | 注意：此处的大小是 （eden+ 2 survivor space). 与jmap -heap中显示的New gen是不同的。 整个堆大小= 年轻代大小 + 年老代大小 + 持久代大小. 增大年轻代后, 将会减小年老代大小.此值对 系统性能影响较大,Sun官方 推荐配置为整个堆的3/8 |
| -XX:NewSize                   | 设置年轻代大 小(for 1.3/1.4)                                 |                       |                                                              |
| -XX:MaxNewSize                | 年轻代最大值 (for 1.3/1.4)                                   |                       |                                                              |
| -XX:PermSize                  | 设置持久代 (perm gen)初 始值                                 | 物理内存的 1/64       |                                                              |
| -XX:MaxPermSize               | 设置持久代最 大值                                            | 物理内存的 1/4        |                                                              |
| -Xss                          | 每个线程的堆 栈大小                                          |                       | JDK5.0以后每个线程堆栈大 小为1M,以前每个线程堆栈 大小为256K.更具应用的线程 所需内存大小进行 调整.在相 同物理内存下,减小这个值能 生成更多的线程.但是操作系 统对一个进程内的线程数还 是有限制的,不能无限生成,经 验值在3000~5000左右 一般 小的应用， 如果栈不是很 深， 应该是128k够用的 大 的应用建议使用256k。这个 选项对性能影响比较大，需 要严格的测试。 |
| -XX:ThreadStackSize           | Thread Stack Size                                            |                       | (0 means use default stack size) [Sparc: 512; Solaris x86: 320 (was 256 prior in 5.0 and earlier); Sparc 64 bit: 1024; Linux amd64: 1024 (was 0 in 5.0 and earlier); all others 0.] |
| -XX:NewRatio                  | 年轻代(包括 Eden和两个 Survivor区)与 年老代的比值 (除去持久代) |                       | -XX:NewRatio=4表示年轻代 与年老代所占比值为1:4,年 轻代占整个堆栈的1/5 Xms=Xmx并且设置了Xmn 的情况下，该参数不需要进 行设置。 |
| -XX:SurvivorRatio             | Eden区与 Survivor区的 大小比值                               |                       | 设置为8,则两个Survivor区 与一个Eden区的比值为2:8, 一个Survivor区占整个年轻 代的1/10 |
| -XX:LargePageSizeInBytes      | 内存页的大小 不可设置过 大， 会影响 Perm的大小               |                       | =128m                                                        |
| \- XX:+UseFastAccessorMethods | 原始类型的快 速优化                                          |                       |                                                              |
| -XX:+DisableExplicitGC        | 关闭 System.gc()                                             |                       | 关闭 System.gc()                                             |
| -XX:MaxTenuringThreshold      | 垃圾最大年龄                                                 |                       | 如果设置为0的话,则年轻代 对象不经过Survivor区,直接 进入年老代. 对于年老代比较 多的应用,可以提高效率.如果 将此值设置为一个较大值,则 年轻代对象会在Survivor区 进行多次复制,这样可以增加 对象再年轻代的存活 时间,增 加在年轻代即被回收的概率 该参数只有在串行GC时才有 效 |
| -XX:+AggressiveOpts           | 加快编译                                                     |                       |                                                              |
| -XX:+UseBiasedLocking         | 锁机制的性能 改善                                            |                       |                                                              |
| -Xnoclassgc                   | 禁用垃圾回收                                                 |                       |                                                              |
| \- XX:SoftRefLRUPolicyMSPerMB | 每兆堆空闲空 间中 SoftReference 的存活时间                   |                       | 可达的对象在上次被引用后 将保留一段时间。 缺省值是 堆中每个空闲兆字节的生命 周期的一秒钟 |
| -XX:PretenureSizeThreshold    | 对象超过多大 是直接在旧生 代分配                             | 0                     | 单位字节 新生代采用 Parallel Scavenge GC时无 效 另一种直接在旧生代分配 的情况是大的数组对象,且数 组中无外部引用对象. |
| -XX:TLABWasteTargetPercent    | TLAB占eden 区的百分比                                        | 1%                    |                                                              |
| -XX:+CollectGen0First         | FullGC时是否 先YGC                                           | false                 |                                                              |

**并行收集器相关参数**

| -XX:+UseParallelGC           | Full GC采用 parallel MSC                             |      | 选择垃圾收集器为并行收集器.此配置 仅对年轻代有效.即上述配置下,年轻代 使用并发收集,而年老代仍旧使用串行 收集 |
| ---------------------------- | ---------------------------------------------------- | ---- | ------------------------------------------------------------ |
| -XX:+UseParNewGC             | 设置年轻代 为并行收集                                |      | 可与CMS收集同时使用 JDK5.0以 上,JVM会根据系统配置自行设置,所以 无需再设置此值 |
| -XX:ParallelGCThreads        | 并行收集器 的线程数                                  |      | 此值最好配置与处理器数目相等 同样 适用于CMS                  |
| -XX:+UseParallelOldGC        | 年老代垃圾 收集方式为 并行收集 (Parallel Compacting) |      | 年老代垃圾 收集方式为 并行收集 (Parallel Compacting)         |
| -XX:MaxGCPauseMillis         | 每次年轻代 垃圾回收的 最长时间(最 大暂停时间)        |      | 如果无法满足此时间,JVM会自动调整 年轻代大小,以满足此值.      |
| \- XX:+UseAdaptiveSizePolicy | 自动选择年 轻代区大小 和相应的 Survivor区比例        |      | 设置此选项后,并行收集器会自动选择 年轻代区大小和相应的Survivor区比 例,以达到目标系统规定的最低相应时 间或者收集频率等,此值建议使用并行 收集器时,一直打开. |
| -XX:GCTimeRatio              | 设置垃圾回 收时间占程 序运行时间 的百分比            |      | 公式为1/(1+n)                                                |
| \- XX:+ScavengeBeforeFullGC  | Full GC前调 用YGC                                    | true |                                                              |

### 8.3 JVM相关工具

#### 8.3.1 JVM 工具概述

$JAVA_HOME/bin下

| 命令      | 说明                                        |
| --------- | ------------------------------------------- |
| jps       | 查看所有jvm进程                             |
| jinfo     | 查看进程的运行环境参数，主要是jvm命令行参数 |
| jstat     | 对jvm应用程序的资源和性能进行实时监控       |
| jstack    | 查看所有线程的运行状态                      |
| jmap      | 查看jvm占用物理内存的状态                   |
| jhat      | +UseParNew                                  |
| jconsole  | 图形工具                                    |
| jvisualvm | 图形工具                                    |

#### 8.3.2 jps

JVM 进程状态工具

```bash
# 格式
jps：Java virutal machine Process Status tool，
jps [-q] [-mlvV] [<hostid>]
-q：静默模式；
-v：显示传递给jvm的命令行参数；
-m：输出传入main方法的参数；
-l：输出main类或jar完全限定名称；
-v：显示通过flag文件传递给jvm的参数；
[<hostid>]：主机id，默认为localhost；
```

```bash
#显示java进程
[root@tomcat ~]#jps
22357 Jps
21560 Main
21407 HelloWorld
#详细列出当前Java进程信息
[root@tomcat ~]#jps -l -v
21560 org.netbeans.Main -Djdk.home=/usr/local/jdk -
Dnetbeans.default_userdir_root=/root/.visualvm -
Dnetbeans.dirs=/usr/local/jdk/lib/visualvm/visualvm:/usr/local/jdk/lib/visualvm/
profiler: -Dnetbeans.home=/usr/local/jdk/lib/visualvm/platform -Xms24m -Xmx256m
-Dsun.jvmstat.perdata.syncWaitMs=10000 -Dsun.java2d.noddraw=true -
Dsun.java2d.d3d=false -Dnetbeans.keyring.no.master=true -
Dplugin.manager.install.global=false --add-exports=java.desktop/sun.awt=ALLUNNAMED --add-exports=jdk.jvmstat/sun.jvmstat.monitor.event=ALL-UNNAMED --add-exports=jdk.jvmstat/sun.jvmstat.monitor=ALL-UNNAMED --addexports=java.desktop/sun.swing=ALL-UNNAMED --addexports=jdk.attach/sun.tools.attach=ALL-UNNAMED --add-modules=java.activation -
XX:+IgnoreUnrecognizedVMOptions -XX:+HeapDumpOnOutOfMemoryError -
XX:HeapDumpPath=/root/.visualvm/8u131/var/log/heapdump.hprof
22270 sun.tools.jps.Jps -
Denv.class.path=/usr/local/jdk/lib/:/usr/local/jdk/jre/lib/ -
Dapplication.home=/usr/local/jdk1.8.0_241 -Xms8m
21407 HelloWorld -Xms256m -Xmx512m
```

#### 8.3.3 jinfo

输出给定的java进程的所有配置信息

```bash
jinfo [option] <pid>
-flags：打印 VM flags
-sysprops：to print Java system properties
-flag <name>：to print the value of the named VM flag
```

```bash
#先获得一个java进程ID，然后jinfo
[root@tomcat ~]#jps
22357 Jps
21560 Main
21407 HelloWorld
[root@tomcat ~]#jinfo 21407
Attaching to process ID 21407, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.241-b07
Java System Properties:
[root@tomcat ~]#jinfo -flags 21407
Attaching to process ID 21407, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.241-b07
Non-default VM flags: -XX:CICompilerCount=2 -XX:InitialHeapSize=268435456 -
XX:MaxHeapSize=536870912 -XX:MaxNewSize=178913280 -XX:MinHeapDeltaBytes=196608 -
XX:NewSize=89456640 -XX:OldSize=178978816 -XX:+UseCompressedClassPointers -
XX:+UseCompressedOops -XX:+UseFastUnorderedTimeStamps 
Command line:  -Xms256m -Xmx512m
```

#### 8.3.4 jstat

输出指定的java进程的统计信息

```bash
jstat -help|-options
jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
[<interval> [<count>]]
interval：时间间隔，单位是毫秒；
count：显示的次数；
#返回可用统计项列表
# jstat -options
    -class：class loader
    -compiler：JIT
    -gc：gc
    -gccapacity：统计堆中各代的容量
    -gccause：
    -gcmetacapacity
    -gcnew：新生代
    -gcnewcapacity
    -gcold：老年代
    -gcoldcapacity
    -gcutil
    -printcompilation
```

```bash
[root@tomcat ~]#jstat -gc 21407
 S0C   S1C   S0U   S1U     EC       EU       OC         OU       MC     MU   
CCSC   CCSU   YGC     YGCT   FGC   FGCT     GCT   
8704.0 8704.0 1563.6  0.0   69952.0  58033.0   174784.0     0.0     9344.0 
8830.0 1152.0 1013.0      2    0.050   0      0.000    0.050
S0C:S0区容量
YGC：新生代的垃圾回收次数
YGCT：新生代垃圾回收消耗的时长；
FGC：Full GC的次数
FGCT：Full GC消耗的时长
GCT：GC消耗的总时长
#3次，一秒一次
[root@tomcat ~]#jstat -gcnew 21407 1000 3
 S0C   S1C   S0U   S1U   TT MTT DSS     EC       EU     YGC     YGCT  
8704.0 8704.0 1563.6    0.0 15  15 4352.0  69952.0  62794.3      2    0.050
8704.0 8704.0 1563.6    0.0 15  15 4352.0  69952.0  62794.3      2    0.050
8704.0 8704.0 1563.6    0.0 15  15 4352.0  69952.0  63074.5      2    0.050
```

#### 8.3.5 jstack 

程序员常用堆栈情况查看工具 

jstack：查看指定的java进程的线程栈的相关信息

```bash
jstack [-l] <pid>
jstack -F [-m] [-l] <pid>
-l：long listings，会显示额外的锁信息，因此，发生死锁时常用此选项
-m：混合模式，既输出java堆栈信息，也输出C/C++堆栈信息
-F：当使用"jstack -l PID"无响应，可以使用-F强制输出信息
```

```bash
#先获得一个java进程PID，然后jinfo
[root@tomcat ~]#jstack -l 21407
2020-02-15 13:49:56
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.241-b07 mixed mode):
"RMI TCP Connection(4)-10.0.0.101" #16 daemon prio=9 os_prio=0 
tid=0x00007f279c29e800 nid=0x5753 runnable [0x00007f279b181000]
   java.lang.Thread.State: RUNNABLE
 at java.net.SocketInputStream.socketRead0(Native Method)
......
```

#### 8.3.6 jmap 

Memory Map, 用于查看堆内存的使用状态

```bash
#查看进程堆内存情况
[root@tomcat ~]#jmap -heap 21407
Attaching to process ID 21407, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.241-b07
using thread-local object allocation.
Mark Sweep Compact GC
Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 536870912 (512.0MB)
   NewSize                  = 89456640 (85.3125MB)
   MaxNewSize               = 178913280 (170.625MB)
   OldSize                  = 178978816 (170.6875MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)
```

#### 8.3.7 jhat 

Java Heap Analysis Tool 堆分析工具

```bash
jmap [option] <pid> 
#查看堆空间的详细信息：
jmap -heap <pid>
```

```bash
# 范例
[root@t1 ~]#jmap -heap 96334
Attaching to process ID 96334, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.251-b08
using thread-local object allocation.
Mark Sweep Compact GC
Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 251658240 (240.0MB)
   NewSize                  = 5570560 (5.3125MB)
   MaxNewSize               = 83886080 (80.0MB)
   OldSize                  = 11206656 (10.6875MB)
#查看堆内存中的对象的数目：
jmap -histo[:live] <pid>
 live：只统计活动对象；
 
#保存堆内存数据至文件中，而后使用jvisualvm或jhat进行查看：
jmap -dump:<dump-options> <pid>
 dump-options:
 live   dump only live objects; if not specified, all objects in the heap are 
dumped.
 format=b     binary format
 file=<file> dump heap to <file>
```

#### 8.3.8 jconsole 

图形化工具,可以用来查看Java进程信息 

JMX（Java Management Extensions，即Java管理扩展）是一个为JAVA应用程序、设备、系统等植入管 理功能的框架。JMX可以跨越一系列异构操作系统平台、系统体系结构和网络传输协议，灵活的开发无 缝集成的系统、网络和服务管理应用。 

JMX最常见的场景是监控Java程序的基本信息和运行情况，任何Java程序都可以开启JMX，然后使用 JConsole或Visual VM进行预览。

```bash
#为Java程序开启JMX很简单，只要在运行Java程序的命令后面指定如下命令即可
-Djava.rmi.server.hostname=10.0.0.100      #指定自已监听的IP
-Dcom.sun.management.jmxremote.port=1000   #指定监听的PORT
-Dcom.sun.management.jmxremote.ssl=false
-Dcom.sun.management.jmxremote.authenticate=false
```

在 tomcat 开启远程 JMX 支持 Zabbix 监控，如下配置

```bash
vim /usr/local/tomcat/bin/catalina.sh
CATALINA_OPTS="$CATALINA__OPTS
-Dcom.sun.management.jmxremote         #启用远程监控JMX 
-Dcom.sun.management.jmxremote.port=XXXXX       #默认启动的JMX端口号，要和
zabbix添加主机时候的端口一致即可
-Dcom.sun.management.jmxremote.authenticate=false    #不使用用户名密码
-Dcom.sun.management.jmxremote.ss1=false       #不使用ssl认证
-Djava.rmi.server.hostname=<JAVA主机IP>"     #tomcat主机自己的IP地址,不要写zabbix服务器的地址
```

### 8.4 Tomcat 性能优化常用配置

#### 8.4.1 内存空间优化

```bash
JAVA_OPTS="-server -Xms4g -Xmx4g -XX:NewSize= -XX:MaxNewSize= "
-server：服务器模式
-Xms：堆内存初始化大小
-Xmx：堆内存空间上限
-XX:NewSize=：新生代空间初始化大小 
-XX:MaxNewSize=：新生代空间最大值
```

生产案例：

```bash
[root@centos8 ~]#vim /usr/local/tomcat/bin/catalina.sh 
JAVA_OPTS="-server -Xms4g -Xmx4g -Xss512k -Xmn1g -
XX:CMSInitiatingOccupancyFraction=65 -XX:+AggressiveOpts -XX:+UseBiasedLocking -
XX:+DisableExplicitGC -XX:MaxTenuringThreshold=10 -XX:NewRatio=2 -
XX:PermSize=128m -XX:MaxPermSize=512m -XX:CMSFullGCsBeforeCompaction=5 -
XX:+ExplicitGCInvokesConcurrent -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -
XX:+CMSParallelRemarkEnabled -XX:+UseCMSCompactAtFullCollection -
XX:LargePageSizeInBytes=128m -XX:+UseFastAccessorMethods"
#一台tomcat服务器并发连接数不高,生产建议分配物理内存通常4G到8G较多,如果需要更多连接,一般会利用虚拟化技术实现多台tomcat
```

#### 8.4.2 线程池调整

```bash
[root@centos8 ~]#vim /usr/local/tomcat/conf/server.xml
......
<Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"
redirectPort="8443" />
......
```

常用属性： 

* connectionTimeout ：连接超时时长,单位ms 
* maxThreads：最大线程数，默认200 
* minSpareThreads：最小空闲线程数 
* maxSpareThreads：最大空闲线程数 
* acceptCount：当启动线程满了之后，等待队列的最大长度，默认100 
* URIEncoding：URI 地址编码格式，建议使用 UTF-8 
* enableLookups：是否启用客户端主机名的DNS反向解析，缺省禁用，建议禁用，就使用客户端IP 就行 
* compression：是否启用传输压缩机制，建议 "on"，CPU和流量的平衡 
  * compressionMinSize：启用压缩传输的数据流最小值，单位是字节 
  * compressableMimeType：定义启用压缩功能的MIME类型text/html, text/xml, text/css,  text/javascript

#### 8.4.3 Java压力测试工具 

PerfMa 致力于打造一站式IT系统稳定性保障解决方案，专注于性能评测与调优、故障根因定位与解决， 为企业提供一系列技术产品与专家服务，提升系统研发与运行质量。

```bash
#社区产品
https://opts.console.perfma.com/
```

