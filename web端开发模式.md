# web端开发范式（通用的数据结构，算法和设计模式）


## web开发生命周期
- 前期准备
  - 开发环境搭建
  - 技术选型
  - 构建系统
- 编码
  - 最小web应用
- 上线
- 数据分析
- 持续交互
- 遗留系统
- 回顾与新架构




- UI界面，html css js组成
- 模型（数据源），数据库 文件系统 第三方
- 逻辑 业务模型 url路由 控制器转发 数据验证 异常错误处理 
- 工具 验证码 分页 文件处理 图形处理 网络操作 数据封装 缓存

app-
  |-contorller
  |-model
  |-view
  |-util


浏览器发送一个请求交给web服务器，web服务器根据请求的类型响应相应的内容。浏览器根据响应的内容进行解析。
  
- 解析（不用浏览器对以下描述可能会不同处理，不过大体上时一样的）  
内容遇到html标签时会开始建立Dom模型结构。
js,css,jpg等其他格式的后缀都会直接显示出来，不会进行构建DOM模型。
遇到doc,zip,exe等文件可能会进行下载
总之浏览器根据Content-Type来判断需要进行什么样的处理

典型的web程序都是无状态的，一次请求一次响应，没有像客户端那样有事件处理、消息队列、异步工作线程的运行模式。
一次web请求都是单线程或单进程的，同步执行代码的。一次请求中，假如遇到耗时的网络操作（采集，抓取别人网站的内容），就会加长等待时间。这是web应用场景所决定的，因为它需要应付非常多的请求，所以创造者这样设计web后端的运行模式。上万次请求同时到达web服务器，web服务器为每个请求建立一个进程或线程。

php可以使用php的命令行模式运行，只不过这不是多线程，而是多进程
nodejs可以使用相应的模块
Java原生的多线程模型
web应用服务端和桌面客户端，移动客户端的不同之处就是这些方面。这个是因为直接在web程序里使用多线程，是不恰当的，web请求通常有时间限制，例如timeout默认是30秒。你如何保证，在线程任务执行完之前，它的父进程仍然在工作？
在客户端这边，我们可有单线程，多线程，异步，同步。 主线程 - 工作线程（ajax，事件处理，自定义等异步线程）

同步阻塞与异步非阻塞https://segmentfault.com/a/1190000004322358
JavaScript 是单线程的，说明代码只能按照执行顺序一行一行执行，遇到耗时代码就会阻塞住（不能像其他语言一样开一个线程来执行耗时操作），然而浏览器是事件驱动的，很多操作是异步的，浏览器有多个线程来做此工作，js 中的异步代码如 AJAX 完成请求、 setTimeout() 定时器到时间后，浏览器会把这些需要执行的异步操作放到自己的消息队列中，并轮循这个队列，在浏览器空闲时处理事件。


http://www.runoob.com/nodejs/nodejs-web-module.html
Web 应用架构
Web 应用架构
Client - 客户端，一般指浏览器，浏览器可以通过 HTTP 协议向服务器请求数据。
Server - 服务端，一般指 Web 服务器，可以接收客户端请求，并向客户端发送响应数据。
Business - 业务层， 通过 Web 服务器处理应用程序，如与数据库交互，逻辑运算，调用外部程序等。
Data - 数据层，一般由数据库组成。


如果PHP代码损坏，不会拖垮整个服务器，4. PHP进程短暂 这两点我要放在一起说，为什么？因为这正是node.js最大的优势。 坦率地讲PHP与node.js根本没有比较性而言，争的是什么？是HTTP SERVER之争。PHP的黄金搭档是apache？不，还少一个FastCgi,node.js和php的比较，是node.js与php+apache+fastcgi的比较，简单地讲FastCgi就是多线程的Cgi(当然有线程池），当每一个PHP请求，都会产生一个新的线程，每个线程都是独立的，线程不会影响另外的线程，那么这就形成PHP的两个特点，相对的稳定，相对的并发。为什么是相对的稳定和相对的并发？相对的稳定的是一个进程的崩溃不会影响另外一个进程（因为肯定是一个请求的代码出现问题造成的崩溃，但一个崩溃不影响其它我们就可以视而不见？），相对的并发是如果一个服务器可以开250个线程，再加上线程池就可以保证一定的并发量。 但是，线程是什么？是OS执行的最小单元，由线程ID，当前指令指针(PC），寄存器集合和堆栈组成（是不是专业的都要背?），然而线程是昂贵的，线程对cpu和内存的占用都是很重的，就象一个人跑步一样，你加速了，停下，启动再加速，那是很费力的。这就使得HTTP SERVER的后起之秀nginx以光的速度蔓延开来，nginx就是以单线程为基础的，为什么nginx快？WEB的世界是可以通过木桶效应来论述，也就是一次请求最短的木板就是瓶颈，很多人认为运算处理会阻塞主线程，这是需要由测试和经验来决定的，事实是它的处理时间占用很短，做100万次for循环要比一次网络访问的时间还要短，这些操作包括网络访问，I/O操作，数据库访问。nginx进程模型是所有需要阻塞请求的部分全部由epool（事件通知接口）触发相应事件，非阻塞（处理耗时 很短）部分用主线程一直执行，直到遇到阻塞部分就停止，交由阻塞部分监听异步完成事件，这样就构成了事件驱动模型。这部分可以研究一下nginx进程就可以得知。 说到这里，大家是不是觉得node.js与nginx进程模型很象，是的！这就是node.js创始人Ryan Dahl的初衷，单线程，事件模型。 node.js就是基于单线程和事件模块，这也恰恰是javascript特性（所以最终Ryan Dahl选择javascript是必然），node.js的单线程就是主线程，主线程是什么？就是事件队列，node.js(javascript)就是面向事件驱动的编程，web是事件化的，os也是事件化的。os的服务（I/O,database）都是在自己独立的进程中完成，当发生一个请求后，node.js只是在这个事件队列中注册一个事件，然后就不管了，具体这个事件的执行就交给了os(i/o,database)不同的进程，等os完成后，通过回调(node.js会轮询)再回到主线程的处理，所以node.js单线程是指自己的主线程，而不是所有的操作都是在这个线程里，这个线程只是事件调度，事件的执行都是交给操作系统在其它的线程中完成。所以node.js是非I/O阻塞，而不是非cpu阻塞,那么node.js会出现cpu阻塞，是不是性能就会有问题呢？不是的，就象Nginx一样，因为对web而言运算是非常非常小的，一次I/O的操作的时间远大于你做百万次的for循环。 5．更大的标准库 没有选择是最容易的，就以前我一直用微软的工具一样。其实对于库而言，应该是一些重要的库是不是还在更新和进步，至少在node.js发展了这么多年，对我而言对重要的web所必要的模块已经足够强壮

###理解两个概念
##IO密集型，cpu密集型。
web应用运行模式
1.单进程（可通过开启多个实例来实现多进程），单线程（单个主线程，多个其它工作线程）、事件驱动，异步执行，非阻塞IO---实际例子：nodejs、ReactPHP（自己建立web服务器处理静、动态请求），利用事件驱动模式实现异步。请求处理流程：
这种工作机制就有几个好处：

不用创建多个线程。这样基本上就不存在线程占用内存的问题。
没有了线程之间的切换的性能消耗，线程的频繁创建和销毁的代价也基本没有（Servlet中也可以用线程池解决线程的频繁创建贤惠问题）
另外一点，就是它的异步 IO 也是提升性能的重要手段之一，参考：Java IO：操作系统的IO处理过程以及5种网络IO模型
但是，这也伴随着一些缺点：

无法有效利用多核
适合 IO 密集型任务，不合适 CPU 密集型任务
Ps：当然，看资料显示加入其他一些手段可以解决上面两个问题，但是本身来说就是会存在上面两个问题。


2.多进程、同步执行，阻塞IO---实际例子：nginx处理静态，动态代理+php-FPM。请求处理流程

3.单进程、多线程,阻塞IO，IO池。实际例子：Java Servlet则是自成一套，当然由于Java的单进程多线程环境


1与2对比----http://taobaofed.org/blog/2015/11/24/nodejs-php-process-manager/

进程，线程，IO。各种语言，应用是如何实现这些的，或者说是以什么样的机制运行的。这些机制有同步，异步，单进程，多进程，单线程，多线程。（多去看看操作系统原理）
各种语言的hello world原理，从底层出发解释

lnmp利用nginx反向代理，php-fpm多进程，创建线程池，同步无状态（请求建立资源，完成释放资源）来实现高并发

nodejs利用自建多web服务器应用（上下文，每个app都是单进程多线程），异步io，事件驱动模型来实现高并发

javaEE利用多线程，创建线程池，连接池实现高并发


并发 同步 异步 cpu计算 io操作

并发是业务含义是能做更多事情，而异步是让多个人共同完成一个任务，异步其实是通过专业角度把一个大任务拆分成相对独立小任务，让更加专业的人完成这个小任务，小任务完成后最后汇总成一个大任务的结果

在计算机里不管执行什么任务都会包含IO操作和CPU计算两个过程，IO的速度问题常常拖累了CPU的计算，由于某些IO太慢了，如果站在CPU的角度，它等待IO处理的代价实在太高了，所以先进的IO就是为了调整IO处理和CPU计算的关系，我觉得新IO解决方案要解决的核心问题


## vps 虚拟主机 云服务器区别
虚拟主机就是空间，可以上传下载；vps就是虚拟服务器，不管是建站还是安装软件自定义系统都行；云主机就是弹性vps，当然价格和可操作性和性能都在依次提升



## web后端常用功能集合
- 数据库操作（读取，存储）
- 缓存操作（读取，存储）内存，redis，文件，数据库
- 文件操作（读取，存储）
- 异常处理（自定义异常如业务异常token失效等，系统异常如代码出错，运行时异常等）
- 数据交互（返回数据类型xml，json，html）
- 加解密数据
- 资源操作（如创建图片，文本，excel，word格式的文件）
- 操作日志统计（每一次请求的记录）
- 日期事件操作
- 正则过滤
- 图片操作

## web后端架构
- 框架的形成
- 采用单例构建配置
- 采用mvc构建请求
- 采用依赖注入
- 采用自动加载类
- 采用dao映射数据库和模型
- 图片鉴黄
- 中文分词
- 敏感词过滤
## 好用的composer库，php类库
爬虫类 
composer require kiddyu/beanbun
文档 http://www.beanbun.org

composer require jaeger/querylist  css选择器过滤支持
文档 https://doc.querylist.cc/

数据库操作类
composer require catfan/Medoo
文档 http://medoo.lvtao.net/

图片操作类
composer require kosinix/grafika
文档 https://segmentfault.com/a/1190000007411281

中文分词类
composer require fukuball/jieba-php:dev-master
文档 https://github.com/fukuball/jieba-php

http客户端
composer require guzzlehttp/guzzle
文档 http://guzzle-cn.readthedocs.io/zh_CN/latest/quickstart.html

orm类
composer require think/think-orm

有没有那种比较好用的orm操作类，文档齐全的。像thinkorm确实好用，但是只能用在框架里，很烦
我要那种能够用在cli里面的。这种其实就是提供使用数据库的开发效率。自己写一个？？？时间成本太高了

自己的工具类
```
/**
 * @param $str 要截取的字符串，兼容中文字符
 * @param $mark1 开始
 * @param $mark2 结束
 * @return bool|int|string
 */

function getNeedBetween($str,$mark1,$mark2){
    $kw=$str;
    $kw='123'.$kw.'123';
    $st =mb_stripos($kw,$mark1);
    $ed =mb_stripos($kw,$mark2);
    echo mb_strlen($mark1);
    if(($st==false||$ed==false)||$st>=$ed)
        return 0;
    $kw=mb_substr($kw,($st+mb_strlen($mark1)),($ed-$st-mb_strlen($mark2)));
    return $kw;
}
```
封装标准json格式返回
有数据，无数据，异常错误

封装curl网络请求


分层架构
服务层 模型层 控制器层 视图层

一些标准库都是采用设计模式来构造成让开发者比较好用的样子

感觉很难封装起来阿

在实际项目中针对业务和功能的分层和抽象，这点很重要。分的好的话
可以复用以前写的功能，提高开发效率
问题是如何呢

对于andorid
分模块 --核心模块  --UI模块 --业务模块  
分层 -- MVC  MVP

对于后端

分层
数据模型层--db--业务

架构演进==
1.lamp单台服务器 curd，使用源码开发
2.引入DAO，ORM解决sql查询语句容易出错，引入框架
3.数据库变成瓶颈，引入动静分离，数据库读写分离

网站在不同的阶段遇到的问题不一样，而解决这些问题使用的技术也不一样：
（1）流量小的时候，我们要提高开发效率，可以在早期要引入ORM，DAO；
（2）流量变大，可以使用动静分离、读写分离、主从同步、垂直拆分、CDN、MVC等方式不断提升网站的性能和研发效率；
（3）面对更大的流量时，通过垂直拆分、服务化、反向代理、开发框架（站点/服务）等等手段，可以不断提升高可用（研发效率）；
（4）在面对上亿级的流量时，通过配置中心、柔性服务、消息总线、自动化（回归，测试，运维，监控）来迎接新的挑战；


php执行方式
web php-fpm 
client发送请求到web服务器，web服务器转发给php-fpm(php-cgi)
php-cgi包装http协议，如_GET,_ POST,REQUEST等全局变量，在php代码里若有输出
则返回数据给web服务器，web服务器包装成http格式的数据返回客户端

服务器80端口侦听请求为80端口的tcp包。根据http协议通信。



需要提高效率的开发流程

url设计（版本控制，接口规范，返回格式，状态码）
安全设计
控制器设计
模型设计

分层架构
