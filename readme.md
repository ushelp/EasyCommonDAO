# EasyCommonDAO

EasyCommonDAO 为 Java ORM 主流持久层技术提供通用的 `DAO（Data Access Object）` 支持。旨在于消除 DAO 层接口定义和实现。


EasyCommonDAO is a Java ORM Common `DAO(Data Access Object)` implementation, can eliminate the interface and implements of DAO.

## Latest Version/最新版本 

- Least version:  `EasyCommonDAO-1.1.0-RELEASE`

- ORM test version: 

 ```
 Hibernate 5.2.4(JPA 2.1)
 Hibernate 4.3.11(JPA 2.1)
 Hibernate 3.6.10(JPA 2.0)
 EclipseLink 2.6.4(JPA 2.0+)
 spring 4.3.3
 ```

## Featuter/特点

- **中文**

 1. 提供基于 **Hibernate Native APIs ** 和 **Java Persistence API (JPA)** (`Hiberante`， `EclipseLink`) 的通用 DAO 实现
 
 2. 消灭 DAO 层接口定义和实现
 
 3. 两项实现选择：**纯 ORM DAO 实现**，**Spring ORM DAO 无缝集成实现**
 
 5. 简单易用，学习成本低，Hibernate 和 JPA 使用统一接口规范
 
 4. 提供分页对象（`PageBean`），条件对象（`EasyCriteria`）综合查询
 
 5. 提供缓存查询；缓存管理；批量数据处理；级联删除支持...


- **English**

 1. Provides common DAO implementation based on **Hibernate Native APIs** and **Java Persistence API (JPA)** (`Hiberante`, `EclipseLink`)
 
 2. Eliminate the definition and implementation of DAO layer interface
 
 3. Two implementation options: **pure ORM DAO implementation**, **Spring ORM DAO seamless integration implementation**
 
 4. Easy to use, low learning costs, Hibernate and JPA use a unified interface specification
 
 5. Provides paging objects (`PageBean`), conditional objects (`EasyCriteria`)
 
 6. Provide cache query; cache management; batch data processing; cascade delete support ...


## EasyCommonDAO Architecture


![EasyCommonDAO](images/Architecture.png)



## Technical comparison/技术比较

相较于 `Spring Data JPA` ，EasyCommonDAO 无需定义 DAO 接口，也无需学习方法命名解析规范。降低学习成本，并真正的消除 DAO 层实现。

Compared to `Spring Data JPA`, Easy CommonDAO does not need to define a DAO interface, nor does it need to learn method naming conventions. Reduce learning costs, and truly eliminate the DAO layer to achieve.

- **Basic DAO**

 ![Basic DAO](doc/images/BasicDAO.png)

- **Spring Data JPA**

 ![Spring Data JPA](doc/images/SpringDataJPA.png)

- **EasyCommonDAO**

 ![EasyCommonDAO](doc/images/EasyCommonDAO.png)



## Maven

```XML
<dependency>
    <groupId>cn.easyproject</groupId>
    <artifactId>${EasyCommonDAO.artifactId}</artifactId>
    <version>1.1.0-RELEASE</version>
</dependency> 
```


**Choice one EasyCommonDAO.artifactId**

- Hibernate Native API

 ```
 `easycommondao-hibernate5`
 
 `easycommondao-hibernate4`
 
 `easycommondao-hibernate3`
 ```

- JPA

 ```
 `easycommondao-jpa-hibernate5`
 
 `easycommondao-jpa-hibernate4`
 
 `easycommondao-jpa-hibernate3`
 
 `easycommondao-jpa-eclipselink`
 ```



## Document/文档

### 中文

[中文说明文档](doc/readme_zh_CN.md)

[官方主页](http://www.easyproject.cn/easycommondao/zh-cn/index.jsp '官方主页')

[留言评论](http://www.easyproject.cn/easycommondao/zh-cn/index.jsp#donation '留言评论')

如果您有更好意见，建议或想法，请联系我。

### English

[English Readme](doc/readme_en.md)

[The official home page](http://www.easyproject.cn/easycommondao/en/index.jsp 'The official home page')

[Comments](http://www.easyproject.cn/easycommondao/en/index.jsp#donation 'Comments')

If you have more comments, suggestions or ideas, please contact me.

## End

Email：<inthinkcolor@gmail.com>

[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")


**支付宝钱包扫一扫捐助：**

我们相信，每个人的点滴贡献，都将是推动产生更多、更好免费开源产品的一大步。

**感谢慷慨捐助，以支持服务器运行和鼓励更多社区成员。**

<img alt="支付宝钱包扫一扫捐助" src="http://www.easyproject.cn/images/s.png"  title="支付宝钱包扫一扫捐助"  height="256" width="256"></img>



We believe that the contribution of each bit by bit, will be driven to produce more and better free and open source products a big step.

**Thank you donation to support the server running and encourage more community members.**

[![PayPal](http://www.easyproject.cn/images/paypaldonation5.jpg)](https://www.paypal.me/easyproject/10 "Make payments with PayPal - it's fast, free and secure!")

