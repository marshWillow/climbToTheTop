#### 基本理解

方便前后端交流的文档，叫接口文档

而能够达成不同领域开发工程师统一认知的，则是接口规范

接口分为四个基础部分：方法，urI，请求参数，返回值

- 方法：(新增)post,(修改)put,(删除)delete,(查)get
- url：
- 请求参数：一般分成5列，字段，说明，类型，备注，是否必填
- 返回值：分code，message和data。data不是必须的，

主流返回格式都是JSON的形式，

#### RESTFUL

API：Application Programming Interface 应用程序编程接口

REST【REpresentational State Transfer】表现层状态转移，

一句话：URL定位资源，用http动词描述操作

web是什么？

给超文本文件和其他对象提供访问入口的功能

一个web它的基本功能，就是对资源的操作，识别，表示，交互

我们一般通过uri【（Uniform Resource Identifier）统一资源标识符，而URL是统一资源定位符，L是Locator】识别资源，representation表示资源，通过协议（http或者ftp）与资源进行交互

#### swagger

一个接口文档自动生成+功能测试软件，遵循RESTFUL接口风格

