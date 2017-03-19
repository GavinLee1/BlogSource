---
title: 浅入浅出 SpringBoot 集成 Mybatis
date: 2017-03-17 17:08:44  
categories: 教程  
tags: [SpringBoot,converion-over-configuration,J2EE,MicroServices,Mybatis,Mybatis-Spring]  
---
最近看李笑来的一本众创的书《新生》。迷上里面一个词"重生"(不是穿越剧里面的那个玩意儿)以及"再生", "反复重生"。说的是换新行业的重生，学新知识的重生，换新环境的重生，等等诸如此类。大概意思其实就是"Stay hungry, stay foolish"。里面提了几个接地气儿的观点："复利", "践行", "主动选择"。所以，自从新公司入职开始，有很多新东西可以学，总体感觉 very excited。SpringBoot + Mybatis 就是其中的基础。算算时间，试用期结束的日子快到了。所以写点东西，也算是对这段时间学习的一个小小的交代。纯属个人编程经验总结，如果有什么不严谨的地方，欢迎评论指正 。  

<!-- more -->  
***  
## 照例解释几个概念 [1]  
**[SpringBoot](https://www.gitbook.com/book/qbgbook/spring-boot-reference-guide-zh/details):** Spring 是个在教科书里都已经存在 N 多年的东西了。随着 `Convension Over Configureation` 这个概率被越来越多的开发者接受并使用。Spring 略显臃肿的配置文件让这个 `J2EE` 元老级的框架，越发显得捉襟见肘。所以, 顺应`MicroServices`的潮流，SpringBoot 应运而生。用来构建基于Spring框架的标准化的独立部署应用程序。（“再也tmd不用寄人篱下，活在WebContainer的屋檐下了”）。  
> Spring Boot简化了基于Spring的应用开发，你只需要"run"就能创建一个独立的，产品级别的Spring应用。 我们为Spring平台及第三方库提供开箱即用的设置，这样你就可以有条不紊地开始。多数Spring Boot应用只需要很少的Spring配置。  
  
**[Mybatis](http://www.mybatis.org/mybatis-3/zh/):**  说白了就是操作数据库的。
> MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以对配置和原生Map使用简单的 XML 或注解，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。  
  
**[Mybatis - Spring](http://www.mybatis.org/spring/zh/index.html):**  Mybatis针对Spring的适配版本, 可以用依赖注入的方式调用SqlSession。
> MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。 使用这个类库中的类, Spring 将会加载必要的 MyBatis 工厂类和 session 类。 这个类库也提供一个简单的方式来注入 MyBatis 数据映射器和 SqlSession 到业务层的 bean 中。 而且它也会处理事务, 翻译 MyBatis 的异常到 Spring 的 DataAccessException 异常(数据访问异常,译者注)中。最终,它并不会依赖于 MyBatis,Spring 或 MyBatis-Spring 来构建应用程序代码。  

## 话不多说 先看看Demo [2]  
## 从开发环境开始讲起 [3]  
### 技术环境 [3.1]  
- Framwork: SpringBoot, Mybatis  
- IDE: Intellij  
- Project Manager: Maven  
- Version Control: Git  
- Prgrammong Language: Java  
    
### 用Intellij 新建一个`空`项目 [3.2]  
1.直接新建一个空的maven就行了  
{% asset_img 1createMavenEmptyProject.png create maven empty project%}  
  
2.填写maven项目的参数  
{% asset_img 2mavenArgument.png maven argument%}  
  
3.给项目取个名字  
{% asset_img 3mavenProjectName.png maven project name%}  
  
4.以上几步都成功了的话，初始项目的结构大概是这个样子的  
{% asset_img 4emptyProjectStructure.png empty project structure%}  

### Maven Dependencied [3.3]
#### SpringBoot 相关的 Dependencies  
**注意添加parent节点**    
{% codeblock lang:xml %}	<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.3.8.RELEASE</version>
    </parent>

    <properties>
        <spring-boot.version>1.3.8.RELEASE</spring-boot.version>
        <spring.context.version>4.2.4.RELEASE</spring.context.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>${spring-boot.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>${spring-boot.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-undertow</artifactId>
            <version>${spring-boot.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.context.version}</version>
        </dependency>
    </dependencies>  
{% endcodeblock %}  
#### Mybatis 相关的 Dependencies  
{% codeblock lang:xml %}	<properties>
        <mybatis.version>3.4.2</mybatis.version>
        <mybatis-spring.version>1.3.1</mybatis-spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>${mybatis-spring.version}</version>
        </dependency>
    </dependencies>  
{% endcodeblock %}  
#### 数据库连接需要的 Dependencies  
{% codeblock lang:xml %}    <properties>
        <spring-jdbc.version>4.3.5.RELEASE</spring-jdbc.version>
        <mysql-connector.version>5.1.38</mysql-connector.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring-jdbc.version}</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql-connector.version}</version>
        </dependency>
    </dependencies>  
{% endcodeblock %}  
### 项目结构 [3.4]  
### 数据库表 [3.5]  
## 整体上有了个映像之后 一步一步拆开看吧 [4]  
### 先让 SpringBoot 飞  
1.我一般习惯性在根目录下新建一个叫做`Application.java`的文件作为`启动器`  
  
2.然后, 像所以 `Java` 程序一样，你需要一个 `main` 函数  
  
3.在  `Application` 的头上戴个帽子(注解) `@SpringBootApplication` 这个注解其实等价于 `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`。用哪个得根据需求和喜好了，当然还有版本。  
  
4.看起来应该是这个样子:  
{% codeblock lang:java %}  
  
{% endcodeblock %}  

5.然后，amazing的事情发生了: 你的 SpringBoot 项目可以启动了 - "妈妈再也不用担心 So easy 系列"  
  
### 我常用的 SpringBoot 注解  

### Mybatis 的那些关系说 [4.2]  
1.通过Mapper访问数据库：这里有两层意思:  
a) 具体的sql都写在mapper中  
b)上层需要获得mapper去访问数据库  
  
2.sql写在哪儿？  
a) 可以写在定义mapper的interface 中  
b)可以写在 *Mapper.xml中  
这里使用第二种方法，个人感觉写在 xml 文件中，能更加方便的使用mybatis的动态sql特性  
  
3.我们需要SqlSession去打开一个会话。之后,你可以使用它来执行映射语句,提交或回滚连接,最后,当不再需要它的时候, 你可以关闭session。 使用 MyBatis-Spring 之后, 你的 bean 可以通过一个线程安全的 SqlSession 来注入,基于 Spring 的事务配置 来自动提交,回滚,关闭 session。 `不再需要手动关闭SqlSession`。  
  
4.所以，为了获得SqlSession。我们需要`SqlSessionTemplate `。SqlSessionTemplate 是 MyBatis-Spring 的核心。 这个类负责管理 MyBatis 的 SqlSession, 调用 MyBatis 的 SQL 方法, 翻译异常。 SqlSessionTemplate 是线程安全的, 可以被多个 DAO 所共享使用。当调用 SQL 方法时, 包含从映射器 getMapper()方法返回的方法, SqlSessionTemplate 将会保证使用的 SqlSession 是和当前 Spring 的事务相关的。此外,它管理 session 的生命 周期,包含必要的关闭,提交或回滚操作。  
  
5.为了获得`SqlSessionTemplate`。我们需要`SqlSessionFactory`。  
  
6.为了获得`SqlSessionFactory`。我们需要一个`DataSource`。  

## References  
[1] GitBook - SpringBoot参考指南 [Spring-Boot-Reference-Guide](https://www.gitbook.com/book/qbgbook/spring-boot-reference-guide-zh/details)  
[2] [What's Mybatis](http://www.mybatis.org/mybatis-3/zh/)  
[3] [Mybatis - Spring](http://www.mybatis.org/spring/zh/index.html)
  
 
{% codeblock lang:objc %}  
[rectangle setX: 10 y: 10 width: 20 height: 20];  
{% endcodeblock %}