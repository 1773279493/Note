# SpringBoot

## 1.SpringBoot基本介绍

### 1.1SpringBoot是什么

SpringBoot可以轻松创建独立的，生产级的基于Spring的应用程序

SpringBoot直接嵌入Tomcat，Jetty或Undertow，可以直接运行SpringBoot应用程序

### 1.2SpringBoot快速入门

构建一个SpringBoot项目

pom文件配置如下

```xml-dtd
<!-- 导入 springboot 父工程，规定的写法 -->
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.5.3</version>
</parent>
<!-- 导入 web 项目场景启动器,会自动导入和 web 开发相关依赖,非常方便 -->
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
</dependencies>
```

### 1.4 Spring SpringMVC SpringBoot 的关系

#### 1.4.1 梳理关系

他们的关系大概是: Spring Boot > Spring > Spring MVC

Spring MVC 只是 Spring 处理 WEB 层请求的一个模块/组件, Spring MVC 的基石是 Servlet

Spring 的核心是 IOC 和 AOP, IOC 提供了依赖注入的容器 , AOP 解决了面向切面编程

Spring Boot 是为了简化开发, 推出的封神框架(约定优于配置[COC]，简化了 Spring 项目 的配置流程), SpringBoot 包含很多组件/框架，Spring就是最核心的内容之一，也包含 Spring MVC

Spring 家族，有众多衍生框架和组件例如 boot、security、jpa 等, 他们的基础都是 Spring

#### 1.4.2 如何理解 -约定优于配置

1、约定优于配置(Convention over Configuration/COC)，又称按约定编程，是一种软件设计 规范, 本质上是对系统、类库或框架中一些东西假定一个大众化合理的默认值(缺省值)

3、简单来说就是假如你所期待的配置与约定的配置一致，那么就可以不做任何配置，约 定不符合期待时, 才需要对约定进行替换配

4、约定优于配置理念【解读：为什么要搞一个约定优于配置】 约定其实就是一种规范，遵循了规范，那么就存在通用性，存在通用性，那么事情就会变 得相对简单，程序员之间的沟通成本会降低，工作效率会提升，合作也会变得更加简单

## 2.依赖管理和自动配置

### 2.1依赖管理

#### 2.1.1什么是依赖管理

1、spring-boot-starter-parent 还有父项目, 声明了开发中常用的依赖的版本号

2、 并且进行 自动版本仲裁 , 即如果程序员没有指定某个依赖 jar 的版本，则以父项目指 定的版本为准

<!-- 根据依赖就近优先原则，以自己指定的为准 -->

### 2.2 starter场景启动器

#### 2.2.1 starter 场景启动器基本介绍

1、 开发中我们引入了相关场景的 starter，这个场景中所有的相关依赖都引入进来了，比如 我们做 web 开发引入了，该 starter 将导入与 web 开发相关的所有包

2、依赖树 : 可以看到 spring-boot-starter-web ，帮我们引入了 spring-webmvc，spring-web 开发模块，还引入了 spring-boot-starter-tomcat 场景，spring-boot-starter-json 场景，这些 场景下面又引入了一大堆相关的包，这些依赖项可以快速启动和运行一个项目，提高开发 效率.

3、所有场景启动器最基本的依赖就是 spring-boot-starter , 前面的依赖树分析可以看到,

这个依赖也就是 SpringBoot 自动配置的核心依赖

## 3 容器功能

### 3.1 Spring 注入组件的注解

#### 3.1.1 @Component、@Controller、 @Service、@Repositor

说明: 这些在 Spring 中的传统注解仍然有效，通过这些注解可以给容器注入组件

### 3.2 @Configuration

3.2.2 @Configuration 注意事项和细节

1、 配置类本身也是组件， 因此也可以获取, 测试 修改 MainApp.java

2、 SpringBoot2 新增特性： proxyBeanMethods 指定 Full 模式 和 Lite 模式

1. proxyBeanMethods：代理 bean 的方法

(1) Full(proxyBeanMethods = true)、【保证每个@Bean 方法被调用多少次返回的组件都是 单实例的, 是代理方式】

(2) Lite(proxyBeanMethods = false)【每个@Bean 方法被调用多少次返回的组件都是新创 建的, 是非代理方式】

(3) 特别说明: proxyBeanMethods 是在 调用@Bean 方法 才生效，因此，需要先获取 BeanConfig 组件，再调用方法

而不是直接通过 SpringBoot 主程序得到的容器来获取 bean, 注意观察直接通过 ioc.getBean() 获取 Bean, proxyBeanMethods 值并没有生效 

(4) 如何选择: 组件依赖必须使用 Full 模式默认。如果不需要组件依赖使用 Lite 模 

(5) Lite 模 也称为轻量级模式，因为不检测依赖关系，运行速度快

### 3.3 @Import

注意@Import 方式注入的组件, 默认组件的名字就是全类名

@Import({Dog.class, Cat.class})

@Configuration(proxyBeanMethods = false）

加在配置类上

### 3.4 @Conditional

#### 3.4.1 @Conditional 介绍

1、 条件装配：满足 Conditional 指定的条件，则进行组件注入

2、@Conditional 是一个根注解，下面有很多扩展注解

### 3.5 @ImportResource

#### 3.5.1 作用：原生配置文件引入, 也就是可以直接导入 Spring 传统的 beans.xml ，可以认为是 SpringBoot 对 Spring 容器文件的兼容.

### 3.6 配置绑定

#### 3.6.1 一句话：使用 Java 读取到 SpringBoot 核心配置文件 application.properties 的内容， 并且把它封装到 JavaBean 中

@ConfigurationProperties(prefix = "furn01") 指定在 application.properties 前缀 * 这样 Furn 组件就会属性文件中的 值绑定了

#### 3.6.3 注意事项和细节

1、如果 application.properties 有中文, 需要转成 unicode 编码写入, 否则出现乱码

2、使用 @ConfigurationProperties(prefix = "furn01") 会提示如下信息, 但是不会影响使用

![image-20220830115224438](SpringBoot.assets/image-20220830115224438.png)

3、解决 @ConfigurationProperties(prefix = "furn01") 提示信息, 在 pom.xml 增加依赖, 即

```xml-dtd
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-configuration-processor</artifactId>
<optional>true</optional>
</dependency>
```

## 5Lombok

#### 5.1 Lombok 介绍

Lombok 作用

1、 简化 JavaBean 开发, 可以使用 Lombok 的注解让代码更加简洁

2、 Java 项目中，很多没有技术含量又必须存在的代码：POJO 的 getter/setter/toString；异常 处理；I/O 流的关闭操作等等，这些代码既没有技术含量，又影响着代码的美观，Lombok 应运而生                               //@ToString : lombok 注解, 会在编译时生成 toString()                       @ToString                                                                                             //@Data: lombok 注解, 会在编译时生成 setter / getter @Data //@NoArgsConstructor:lombok 注解, 会在编译时生成无参构造器 @NoArgsConstructor                                                                  //@AllArgsConstructor: lombok 注解, 会在编译时生成全参构造器 @AllArgsConstructor

## 6 Spring Initailizr

#### 6.1 Spring Initailizr 介绍

\1. 程序员通过 Maven Archetype 来生成 Maven 项目，项目原型相对简陋, 需要手动配置, 比 较灵活.

\2. 通过 Spring 官方提供的 Spring Initializr 来构建 Maven 项目，能完美支持 IDEA 和 Eclipse， 让程序员来选择需要的开发场景(starter)，还能自动生成启动类和单元测试代码

## 7 yaml

### 7.1 yaml 介绍

1、YAML 是"YAML Ain't a Markup Language"(YAML 不是一种标记语言) 的递归缩写。在开发 的这种语言时，YAML 的意思其实是："Yet Another Markup Language"(仍是一种标记语言)， 是为了强调这种语言以数据做为中心，而不是以标记语言为重点，而用反向缩略语重命名 【百度百科

1、YAML 以数据做为中心，而不是以标记语言为重点

2、YAML 仍然是一种标记语言, 但是和传统的标记语言不一样, 是以数据为中心的标记语 言.

3、YAML 非常适合用来做以数据为中心的配置文件 [springboot : application.yml

### 7.3 yaml 基本语法

1. 形式为 key: value；注意: 后面有空格
2. 区分大小写 
3. 使用缩进表示层级关系 
4.  缩进不允许使用 tab，只允许空格 [有些地方也识别 tab , 推荐使用空格]
5.  缩进的空格数不重要，只要相同层级的元素左对齐即可 
6. 字符串无需加引号 
7. yml 中, 注释使用 #

### 7.4 数据类型

### 7.4.1 字面量

\1. 字面量：单个的、不可再分的值。date、boolean、string、number、null 2. 保存形式为 key: value

### 7.4.2 对象

\1. 对象：键值对的集合, 比如 map、hash、set、object

行内写法： k: {k1:v1,k2:v2,k3:v3} monster: {id: 100,name: 牛魔王}

 #或换行形式

k: 

k1: v1 

k2: v2 

k3: v3 

monster: 

id: 100 

name: 牛魔王

### 7.4.3 数组

\1. 数组：一组按次序排列的值, 比如 array、list、queue

行内写法： k: [v1,v2,v3] hobby: [打篮球, 打乒乓球, 踢足球] 

#或者换行格式 

k: - v1 - v2 - v3 hobby: - 打篮球 - 打乒乓球 - 踢足球

## 8WEB开发-静态资源访问

### 8.2 基本介绍

1. 只要静态资源放在类路径下： /static 、 /public 、 /resources 、 /META-INF/resources 可以被直接访问- 对应文件 WebProperties.java

2. 常见静态资源：JS、CSS 、图片（.jpg .png .gif .bmp .svg）、字体文件(Fonts)等

3. 访问方式 ：默认: 项目根路径/ + 静态资源名 比如 http://localhost:8080/hi.jpg . - 设置 WebMvcProperties.java

### 8.4 静态资源访问注意事项和细节

1. 静态资源访问原理：静态映射是 /** , 也就是对所有请求拦截，请求进来，先看 Controller 能不能处理，不能处理的请求交给静态资源处理器，如果静态资源找不到则响应 404 页面

2. 改变静态资源访问前缀，比如我们希望 http://localhost:8080/hspres/* 去请求静态资源, 应用场景：静态资源访问前缀和控制器请求路径冲突

## 9 Rest 风格请求处理

### 9.1 基本介绍

\1. Rest 风格支持（使用 HTTP 请求方式动词来表示对资源的操作）

### 9.2.3 Rest 风格请求 -注意事项和细节

1、客户端是 PostMan 可以直接发送 Put、delete 等方式请求，可不设置 Filter

2、如果要 SpringBoot 支持 页面表单的 Rest 功能, 则需要注意如下细节

\1) Rest 风 格 请 求 核 心 Filter ； HiddenHttpMethodFilter ， 表 单 请 求 会 被 HiddenHttpMethodFilter 拦截 , 获取到表单 _method 的值， 再判断是 PUT/DELETE/PATCH (注释: PATCH 方法是新引入的，是对 PUT 方法的补充，用来对已知资源进行局部更新: https://segmentfault.com/q/1010000005685904)

\2) 如果要 SpringBoot 支持 页面表单的 Rest 功能, 需要在 application.yml 启用 filter 功能, 否则无效

\3) 修改 application.yml 启用 filter 功能

## 20数据库操作

### 20.1 JDBC+HikariDataSource

HikariDataSource : 目前市面上非常优秀的数据源, 是 springboot2 默认数据源

### 20.2 整合 Druid 到 Spring-Boot

#### 20.2.2 基本介绍

\1. HiKariCP: 目前市面上非常优秀的数据源, 是 springboot2 默认数据源

\2. Druid： 性能优秀，Druid 提供性能卓越的连接池功能外【Java 基础】，还集成了 SQL 监 控，黑名单拦截等功能，强大的监控特性，通过 Druid 提供的监控功能，可以清楚知道连 接池和 SQL 的工作情况，所以根据项目需要，我们也要掌握 Druid 和 SpringBoot 整合

\3. 整合 Druid 到 Spring-Boot 方式 自定义方式 引入 starter 方式

#### 20.2.3 Durid 基本使用
