# REST架构风格（Representational State Transfer）
    REST结构设计是以资源抽象为核心展开的。可寻址：每一个资源在WEB智商都有自己的地址，连通性：应该避免设计孤立的资源，
    除了设计资源本身，还需要设计资源之间的关联关系，并且通过超链接将资源关联起来。无状态、统一接口是REST的两种架构约束，
    超文本驱动是REST一个关键词。

## 6个架构约束
+ 客户-服务器（Client-Server) 通信只能由客户端单方面氦气，表现为请求-响应的形式
+ 无状态（Stateless） 通信的会话状态应该全部由客户端负责维护
+ 缓存（Cache) 响应内容可以在通信链的某处被缓存，以改善网络效率
+ 统一接口（Uniform Interface） 通信链的组件之间通过统一的接口相互通信，以提高交互的可见性
+ 分层系统（Layered System） 通过限制组件的行为，将构架分解为若干等级的层
+ 按需代码（Code-On_Demand，可选） 支持通过下载并执行一些代码，对客户端的功能进行扩展
    
## 5个关键词
+ 资源（Resource） 
+ 资源的表述（Representation）
+ 状态转移（State Transfer)
+ 统一接口（Uniform Interface）
+ 超文本驱动（Hypertext Driver）

## 6个特征
+ 面向资源（Resource Oriented）
+ 可寻址（Addressability）
+ 连通性（Connectedness)
+ 无状态（Statelessness）
+ 统一接口（Uniform Interface）
+ 超文本驱动（Hypertext Driven）
    
## 3个优点：
+ 简单性
+ 可伸缩性
+ 松耦合
