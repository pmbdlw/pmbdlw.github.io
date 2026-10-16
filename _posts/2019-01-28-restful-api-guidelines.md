---
title: "RESTful 接口开发规范"
date: 2019-01-28
categories: 开发规范
tags: [RESTful, API, 开发规范]
excerpt: "REST 概念、RESTful 架构风格，以及一份可落地的接口开发规范。"
---

版本：v1.0

* 目录
{:toc}


## REST概念
REST是Representational State Transfer的英文缩写，翻译成中文是"表述性状态转移"。具体含义下一章节会做详细解释

REST是一种软件架构风格，不是一种标准，它提供一组设计原则和约束条件，满足这些设计原则和约束条件的架构设计就是RESTful架构风格

##  RESTful Architectural Style
### RESTful架构风格特点

- 资源
- URI
- 统一接口
- 无状态

**资源**

资源就是网络上的一个实体，可以是一个对象，也可以是一种关系。资源需要通过某种载体来表现其内容，例如：图片可以用JPG格式表现，也可以用PNG格式表现，而JSON数据格式是现在最常用的资源表现形式。

**URI（统一资源定位符）**

每一个资源可以用一个特定的URI指向它，要获取这个资源，访问它的URI就可以，因此URI成了每一个资源的独一无二的地址或识别符。

**统一接口**

数据的元操作，即CRUD(create、 read、 update、delete)，分别基于HTTP方法：POST，GET，PUT，DELETE统一了数据操作的接口，仅通过HTTP方法，就可以完成对数据的所有操作

**无状态**

状态应该区分应用状态和资源状态，客户端负责应用状态，服务端负责资源状态。客户端与服务端的交互是无状态的，在每一次请求中包含处理该请求所需的一切信息。服务端不需要在请求间保留应用状态。这种无状态通信原则，使得在多次请求中，同一客户端不再需要依赖于同一服务器，可实现高可扩展和高可用性的服务端。

回过头来，再来理解一下REST的定义"表述性状态转移"，其实它忽略了一个主语"资源" - 资源表述性状态转移。因此结合RESTful架构风格的特点，可以理解REST就是：我们可以通过URI来唯一的定位一个资源，资源通过例如JSON对象来表示，当发生前后端无状态交互时，通过HTTP的GET，POST，PUT，DELETE方法对应create、 read、 update、delete来修改资源的状态。

### RESTful架构风格优势

java几种典型的软件架构

- 分布式对象（Distributed Objects，简称DO）
  架构实例有CORBA/RMI/EJB/DCOM, 面向对象，语言相关，客户端服务器紧耦合，API接口不统一

- 远程过程调用（Remote Procedure Call，简称RPC）
  架构实例有SOAP/XML-RPC/DWR，面向过程，语言相关，客户端服务器紧耦合，API接口不统一

- 表述性状态转移（Representational State Transfer，简称REST）
  架构实例有HTTP/WebDAV，面向资源，语言无关，客户端服务器松耦合，API接口统一

当今的软件开发过程中，往往需要与许多外部系统集成或者调用众多外部服务，REST这种与开发语言无关，仅仅基于HTTP的统一接口就能完成数据交互的特点，再加之客户端服务器松耦合，无疑带来了巨大的便利以及开发效率的提升。

## RESTful API开发规范
### API设计的基本准则

- 当准则合理，且满足条件时，遵守准则
- API应该面向资源定义
- API应该保证GET，PUT，POST，DELETE分别对应query，update，create，delete操作
- API应该对程序员友好，并且在浏览器地址栏容易输入
- API应该简单，直观，容易使用，并且尽量保证优雅
- API应该具有足够的灵活性来支持前端业务需求

```
说明一点：API的就是程序员的UI，和其他UI一样，必须仔细考虑它的用户体验！！！
```

### RESTful API设计和开发细则

下面将详细列出一些RESTful API的设计和开发规范：

1. 文档


```
文档和API一样重要，前期RESTful API文档插件调研，以选择apiggs作为内部RESTful API文档生成器，具体使用方法和文档模板，请参考
forever-play > resourcers 下面的 《Apiggs RESTful API文档生成工具使用指南》。
最终所有API的接口文档必须生成放在项目指定apiDocs目录
```

2. **使用https协议**
```
使用https安全,如果不使用https,请使用其他安全方案,比如使用token等其他认证方案。
```

3. **版本号放入URL**
```
在API上加入版本信息可以有效的防止用户访问已经更新的API，同时也能让不同主要版本之间平稳过渡
get api/v1/users/good  获得用户列表
v表示版本 1版本号.URL中不使用1.1带小版本的子版本号，如果需要放入HEAD中
```


4. **请求头公共参数**
```
每个请求公有的参数,比如token、请求客户端类型、请求来源(前台展示还是后台管理);
公共参数放在请求header中：
请求头参数|是否必须 | 说明
|---|---|---|
|x-app | 是 | 客户端类型:*-ios,*-android,*-pc,*-h5|
|x-lang | 是 | 客户端语言:'en-us','zh-cn'等,默认中文|
|x-orginal | 是 | 请求来源:0-前台展示调用,默认;1-后台管理|
|x-token | 是 | 认证凭证|
其他,后续根据实际情况添加.
```
5.  **只提供json返回格式(其实不仅仅返回json)**
```
对于响应返回的格式，JSON 因为它的可读性、紧凑性以及多种语言支持等优点，成为了 HTTP API 最常用的返回格式，因此采用JSON 作为返回内容的格式。如果用户需要其他格式，比如xml，应该在请求头部 Accept 中指定。
对于不支持的格式，服务端需要返回正确的status code，并给出详细的说明。
Accept:application/json  告诉服务器客户端接受JSON格式返回
Content-Type:application/json  告诉服务器请求内容为JSON格式
```

6.  **使用自定义错误编码**（~~使用http状态码作为错误提示~~）
```
使用http状态码作为错误提示是符合rest接口规范。
业界现在有两种做法:
1. 使用http状态码
比如:
200  请求成功接收并处理，一般响应中都会有 body
404  客户端要访问的资源不存在，链接失效或者客户端伪造 URL 的时候回遇到这个情况
...
2. 使用自定义返回的错误码
比如:
1000 系统错误
2001 用户不存在
2002 用户名称已经存在
...
建议使用自定义的状态。
原因：
http状态码可能不够用(暂时肯定够);
使用自定义返回的错误码更加灵活;
便于后台代码统一处理;
推荐而已;
```


7. **URI Path尽量使用名词，不使用动词，把每个URL看成一个资源**
```
get /getUserList   // bad 获得所有用户列表
get /getActiveUserList //bad 获得所有激活的用户列表
get /api/v1/users // good 获得所有用户列表
get /api/v1/users?state=active // good 获得所有激活的用户列表

rest只是一种接口规范不是标准.尽量遵守即可.有的接口不好用名词描述或者描述会让人很难理解,特殊情况我们还是可以使用动词进行描述.

比如:登录|注销
post /api/v1/session //登录接口
delete /api/v1/session //注销接口
十分不易与理解接口的真实含义，此时用大家非常熟悉的词语，即使是动词也未尝不可，只要注意在文档中写清楚就可以了。
post /api/v1/login //登录接口
delete /api/v1/logout // 注销接口
get /api/v1/search // 搜索

请记住，应该简单，直观，容易使用，并且尽量保证优雅
```

8. **避免URI Path最后带'/'**
```
get /api/v1/users/  // bad
get /api/v1/users  // good
最后带'/'与不带意思完全不一样,甚至两个不同的接口
```

9. **资源名称推荐用复数名词**
```
get /api/v1/users  // good 查询用户列表
get /api/v1/users/1 // good 查询用户1的信息
get /api/v1/user  // bad 查询用户列表
get /api/v1/user/1 // bad 查询用户1的信息
事实上，这是个人爱好问题，但复数形式更为常见。
此外，在资源集合URL上用GET方法，它更直观，特别是get /api/v1/users?state=active。
但最重要的是：避免复数和单数名词混合使用，这显得非常混乱且容易出错。
```

10. **为关系使用子资源**
```
多级资源命名:
get /api/v1/users/1/roles  //查看用户1的角色信息
get /api/v1/roles/admin/users // 查看管理员的角色列表
如果资源层级太深,可以使用除了第一级，其他级别都用查询字符串表达。
get /api/v1/roles?users=1  //查看用户1的角色列表
get /api/v1/users?roles=admin // 查看管理员的角色列表
```

11. **使用小驼峰命名法**
```
使用小驼峰命名法作为属性标识符。
{ "yearOfBirth": 1982 }
不要使用下划线（year_of_birth）或大驼峰命名法（YearOfBirth）。
通常，RESTful Web服务将被JavaScript编写的客户端使用。
客户端会将JSON响应转换为JavaScript对象（通过调用var person = JSON.parse(response)），然后调用其属性。
因此，最好遵循JavaScript代码通用规范。
person.year_of_birth // 不推荐，违反JavaScript代码通用规范
person.YearOfBirth // 不推荐，JavaScript构造方法命名
person.yearOfBirth // 推荐
```

12. **对可选的、复杂的查询参数，使用查询字符串(?)**
```
get /activeUsers    //bad
get /disableUsers   //bad
为了让你的URL更小、更简洁。
为资源设置一个基本URL，将可选的、复杂的参数用查询字符串表示。
get /api/v1/users?state=active&sex=male //good
```


13. **使用HTTP动词（GET,POST,PUT,DELETE）作为action操作URL资源**
```
get 查询
post 添加
patch 修改
put 修改(PUT 是幂等的，而 PATCH 不是幂等的,一般用put就ok)
delete 删除
推荐
get /api/v1/users  // good 查询用户列表
post /api/v1/users //good 添加用户
put  /api/v1/users //good 编辑用户
delete /api/v1/users/1 //good 删除参数
get请求传参使用url请求参数传参
post|put请求使用body传参,json格式
delete请求使用url路径传参,如果是批量删除使用body传参,json格式
get /api/v1/users?state=active //查询激活用户列表
post /api/v1/users  //添加用户
body:
{
    id:1,
    name:'duanledexianxian',
    state:'active',
    ....
}
put /api/v1/users   //修改用户
body:
{
    id:1,
    name:'duanledexianxian',
    state:'active',
    ....
}
delete /api/v1/users/1 //删除参数 1是用户id
delete /api/v1/users //批量删除参数
or 
delete /api/v1/users/batch //批量删除参数
{
    ids:[1,2,....]
}
```

14. **Query Filter信息**
```
例如：
pageNum：第几页,默认0
pageSize：每页条数,默认10
sort：column1,column2排序字段
orderby：排序规则，desc或asc
q：搜索关键字（uri encode之后的）

建议将查询参数封装成QueryCondition的dto对象,通过filter放入TreadLocal中,用的时候直接从TreadLocal获取.
```


15. **返回结果**
```
1. GET：返回资源对象
2. POST：返回新生成的资源对象
3. PUT：返回完整的资源对象
4. DELETE：返回一个空文档
统一返回格式:
{
    code:'SUCCESS',//成功返回'SUCCESS',否则返回自定义的错误编码
    message:'错误描述信息',//如果错误,则还有错误描述信息属性字段
    data:{}// 返回数据内容
}
分页查询返回结果格式
统一返回格式:
{
    code:'SUCCESS',//成功返回'SUCCESS',否则返回自定义的错误编码
    data:{
        pageNum:0,//分页页面
        pageSize:10,//分页大小
        total:100,//记录总数
        pages:10,//分页页数
        list:[...]//列表内容
    }// 返回数据内容
}
新增操作需要返回新增资源的资源唯一编号,比如:新增用户
{
    code:'SUCCESS',//成功返回'SUCCESS',否则返回自定义的错误编码
    data:{
      id:1,//用户编号
      name:'duanledexianxianxian',//用户名称
      ...
    }// 返回数据内容
}
```

16. **速率限制(暂时不需要)**
```
1. X-RateLimit-Limit: 每个IP每个时间窗口最大请求数
2. X-RateLimit-Remaining: 当前时间窗口剩余请求数
3. X-RateLimit-Reset: 下次更新时间窗口的时间（UNIX时间戳），达到下个时间窗口时，Remaining恢复为Limit

如果对访问的次数不加控制，很可能会造成 API 被滥用，甚至被 DDos 攻击。
根据使用者不同的身份对其进行限流，可以防止这些情况，减少服务器的压力。
暂时不需要,以后做限流控制、访问控制的时候会用的上
```

17. **限制返回值的域，fields=id,subject,customerName**
```
比如:/api/v1/users?fields=id,name,....//获取用户列表,包含的字段包括id、名称.可以一起处理
```



*注意*

 ```
对于REST未覆盖的内容：
1. Hypermedia API（HATEOAS），通过接口URL获取接口地址及帮助文档地址信息, 暂时不需要,如果需要实现前后台都需要改动
2. 缓存，使用ETag和Last-Modified 暂时不需要,做性能优化的时候,根据需要添加
3. 文件上传,下载 *

未完待续.....
 ```
## Reference
1. RESTful API 设计最佳实践 <http://blog.jobbole.com/41233/>
2. RESTful Representational State Transfer 表现层状态转化 https://blog.csdn.net/flancklin/article/details/52311505
3. RESTful设计原则和样例 https://my.oschina.net/u/2330859/blog/468829
