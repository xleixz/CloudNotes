# SpringMVC

![SpringMVC](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/SpringMVC.jpg)

---

# 1、MVC介绍

## 1.1 什么是MVC

- MVC是模型(`Model` - dao、service层)、视图(`View` - 数据 - jsp)、控制器(`Controller` - Servlet)的简写，是一种软件设计规范。
- 是将业务逻辑、数据、显示分离的方法来组织代码。
- MVC主要作用是**降低了视图与业务逻辑间的双向偶合**。
- MVC不是一种设计模式，**MVC是一种架构模式**。当然不同的MVC存在差异。

​	

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包

含数据和行为），不过现在一般都分离开来：Va lue Object（数据Dao） 和 服务层（行为Service）。也就是模型提

供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给

视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

​	

前端 	数据传输	实体类

实体类：用户名，密码，生日，爱好……

前端：用户名，密码

​	

pojo：User

vo：UserVo

在pojo中分出一个vo实体类给前端使用，这样可以避免实体类中很多前端用不到的属性，避免了属性的空值

​	

> **Model1时代**

早期开发中使用的是Model1时代，主要分为两层，视图层和模型层。在当前页面将数据返回到页面。

**JSP：本质就是一个Servlet**

> **Model2时代**

Model2把一个项目分成三部分，包括**视图、控制、模型。**

​	

面试题：你的项目架构，是设计好的，还是演进的？

回答：演进的

​	

**职责分析**

**Controller：控制器**

1. 获取表单数据
2. 调用业务逻辑
3. 转向指定的页面

就是Servlet！

**Model：模型**

1. 业务逻辑
2. 保存数据的状态

相当于service层和dao层

**View：视图**

1. 显示页面

相当于jsp

​	

## 1.2 回顾Servlet

**MVC框架要做哪些事情**

1. 将url映射到java类或java类的方法。
2. 封装用户提交的数据。
3. 处理请求--调用相关的业务处理--封装响应数据。
4. 将响应的数据进行渲染 . jsp / html 等表示层数据。

**说明：**

常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端MVC框架：

vue、angularjs、react、backbone；由MVC演化出了另外一些模式如：MVP、MVVM 等等....

---

​	

# 2、SpringMVC介绍

**Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。**

[SpringMVC官网文档](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html "SPringMVC官网文档")

 Spring MVC的特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4. 约定优于配置（约定好的东西不要去改）
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活

​	

> Spring的web框架围绕**DispatcherServlet** [ 调度Servlet ] 设计。
>
> DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可
>
> 以采用基于注解形式进行开发，十分简洁；正因为SpringMVC好 , 简单 , 便捷 , 易学 , 天生和Spring无缝集成(使
>
> 用SpringIoC和Aop) , 使用约定优于配置 . 能够进行简单的junit测试 . 支持Restful风格 .异常处理 , 本地化 , 国际
>
> 化 , 数据验证 , 类型转换 , 拦截器 等等......所以我们要学习。

**最重要的一点还是用的人多 , 使用的公司多。**

​	

## 2.1 中央控制器

Spring的web框架围绕**DispatcherServlet**设计。Dispatcher（调度员）Servlet的作用是将请求分发到不同的处理器。

![image-20220610185630069](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220610185630069.png)

从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

Spring MVC框架像许多其他MVC框架一样, **以请求为驱动** , **围绕一个中心Servlet分派请求及提供其他功能，**

**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)**。

**SpringMVC的原理如下图所示：**

​	当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处

理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返

回给中心控制器，再将结果返回给请求者。

![image-20220610190520975](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220610190520975.png)

​	

## 2.2 SpringMVC执行原理

**简要分析执行流程**

![image-20220611145922253](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611145922253.png)

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求

   并拦截请求。

   - 我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   - **如上url拆分成三部分：**

   - http://localhost:8080服务器域名

   - SpringMVC部署在服务器上的web站点

   - hello表示控制器

   - 通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找

   Handler。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。

---

​	

# 3、HelloSpringMVC程序（配置版）

> 配置版SpringMVC较为繁琐，**注解版**才是SpringMVC的精髓。

1. 新建一个Maven项目，**添加Web支持**；

2. 导入**SpringMVC依赖**；

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.5</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
       </dependencies>
   ```

3. 在**web.xml**配置文件中， **注册DispatcherServlet**；**这是一个SpringMVC的核心: 请求分发器，前端控制器**；

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
      <!--1.注册配置DispatcherServlet  这是一个SpringMVC的核心: 请求分发器，前端控制器-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--绑定一个springmvc的配置文件:【servlet-name】-servlet.xml-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--启动级别-1-->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--/ 匹配所有的请求；（不包括.jsp）-->
       <!--/* 匹配所有的请求；（包括.jsp）-->
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

4. 编写**SpringMVC 的 配置文件**【springmvc-servlet.xml】，这里的名称要求尽量按照官方来【(servletname)-servlet.xml】；

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--添加 处理器映射器-->
       <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
       <!--添加 处理器适配器-->
       <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
       <!--添加 视图解析器 DispatcherServlet提供的ModelAndView-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
           <!--前缀-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--后缀-->
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--将类交给SpringIOC容器，注册bean-->
       <bean id="/hello" class="com.xleixz.controller.HelloController"/>
   </beans>
   ```

   > 处理器映射器

   ```xml
   <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   ```

   > 处理器适配器

   ```xml
   <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
   ```

   > 视图解析器

   ```xml
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
         <!--前缀-->
         <property name="prefix" value="/WEB-INF/jsp/"/>
         <!--后缀-->
         <property name="suffix" value=".jsp"/>
   </bean>
   ```

5. 编写我们要操作业务Controller ，要么实现Controller接口，要么增加注解；需要返回一个ModelAndView，装数据，封视图；

   ```java
   import org.springframework.web.servlet.ModelAndView;
   import org.springframework.web.servlet.mvc.Controller;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   //注意：这里我们先导入Controller接口
   public class HelloController implements Controller {
   
       @Override
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           //ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
   
           //封装对象，放在ModelAndView中。Model
           mv.addObject("msg", "HelloSpringMVC!");
           //封装要跳转的视图，放在ModelAndView中
           mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
           return mv;
       }
   }
   ```

6. 将类交给SpringIOC容器【springmvc-servlet.xml】，注册bean；

   ```xml
   <!--Handler-->
   <bean id="/hello" class="com.kuang.controller.HelloController"/>
   ```

7. 写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面；

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Hello</title>
   </head>
   <body>
   测试成功！
   </body>
   </html>
   ```

​		

<font color = "red">**可能遇到的问题：访问出现404，排查步骤：**</font>

1. 查看控制台输出，看一下是不是缺少了什么jar包。

2. 如果jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖！

   - 点击FIle - Project Structure - Artifacts - 选择对应的项目 - 在**WEB-INF文件夹**下新建**lib** - 点击添加**Library Files** - 将**除了（servlet，jstl，jsp）其他的所有jar包**导入 - OK

     ![image-20220611112808532](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611112808532.png)

3. 重启Tomcat 即可解决！

<font color = "green">**原因：**</font>Maven无法扫描Webapp下的jar包！

---

​	

# 4、注解开发SpringMVC

1. 新建一个Maven项目，**添加Web支持**，导入依赖；<font color="red">（**记住配置静态资源过滤**）、（**记住要在Project Structure中的Artifacts中导入除了3个Servlet包以外的其他jar包 **）</font>

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.5</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
       </dependencies>
   
   
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
           </resources>
       </build>
   ```

2. 配置**web.xml**文件，**注册DispatchServlet前置控制器**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
       <!--1.注册DispatchServlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!-- 启动顺序，数字越小，启动越早 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--所有请求都会被springmvc拦截 -->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

3. **添加Spring MVC配置文件**【springmvc-servlet.xml】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="com.xleixz.controller"/>
       <!-- 让Spring MVC不处理静态资源 .css  .html  .js -->
       <mvc:default-servlet-handler />
       <!--
       支持mvc注解驱动
           在spring中一般采用@RequestMapping注解来完成映射关系
           要想使@RequestMapping注解生效
           必须向上下文中注册DefaultAnnotationHandlerMapping
           和一个AnnotationMethodHandlerAdapter实例
           这两个实例分别在类级别和方法级别处理。
           而annotation-driven配置帮助我们自动完成上述两个实例的注入。
        -->
       <mvc:annotation-driven />
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   
   </beans>
   ```

   > 自动扫描包

   ```xml
   <context:component-scan base-package="com.xleixz.controller"/>
   ```

   > 过滤静态资源，让Spring MVC不处理静态资源，例如：.css  .html  .js

   ```xml
   <mvc:default-servlet-handler />
   ```

   > 注册DefaultAnnotationHandlerMapping（处理器映射器），和一个AnnotationMethodHandlerAdapter（处理器适配器）实例

   ```xml
   <mvc:annotation-driven />
   ```

   > 视图解析器

   ```xml
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   ```

4. **创建Controller业务层**

   ```java
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   //实现了Controller注解，表示这是一个控制器
   @Controller
   public class HelloController {
       //实现了RequestMapping注解，表示这是一个请求映射，这是一个映射地址
       @RequestMapping("/hello")
       public String hello(Model model) {
           //封装数据
           model.addAttribute("hello", "你好");
           return "hello";//会被视图解析器解析处理到hello.jsp
       }
   }
   ```

   > 实现Controller注解，表示一个控制器

   ```xml
   @Controller
   ```

   > 实现RequestMapping注解，表示这是一个请求映射，这是一个映射地址

   ```xml
   @RequestMapping("")
   ```

   > 方法中声明Model类型的参数是为了把Action中的数据带到视图中

   ```xml
   model.addAttribute("hello", "你好");
   ```

   > 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成/**hello**.jsp

   ```xml
   return "hello"
   ```

5. 创建**视图层**【hello.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <hr>
   <h1>测试成功！</h1>
   </hr>
   </body>
   </html>
   ```

​	

使用springMVC必须配置的三大件：

<font color="green">**处理器映射器、处理器适配器、视图解析器**</font>

通常，只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大

段的xml配置。

---

​	

# 5、Controller控制器和RestFul风格

## 5.1 Controller控制器

**环境准备：**

1. 新建一个Maven项目，**添加Web支持**，导入依赖；<font color="red">（**记住配置静态资源过滤**）、（**记住要在Project Structure中的Artifacts中导入除了3个Servlet包以外的其他jar包 **）</font>

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.5</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
       </dependencies>
   
   
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
           </resources>
       </build>
   ```

2. 配置**web.xml**文件，**注册DispatchServlet前置控制器**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
       <!--1.注册DispatchServlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!-- 启动顺序，数字越小，启动越早 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--所有请求都会被springmvc拦截 -->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

3. **添加Spring MVC配置文件**【springmvc-servlet.xml】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   
   </beans>
   ```

4. 视图类【hello.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
      <title>Kuangshen</title>
   </head>
   <body>
   测试Controller接口，成功！
   </body>
   </html>
   ```

​			

搭建好环境后，开始对比**实现Controller接口**和**使用注解@Controller**的区别。

​	

### 5.1.1 方式一：实现Controller接口

```java
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//只要实现了Controller接口，说明这就是一个控制器
public class ControllerTest1 implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","Hello World! - ControllerTest1");
        mv.setViewName("test1");
        return mv;
    }
}
```

![image-20220611175901169](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611175901169.png)

​	

<font color="red">**Spring4.0 开始，不配置处理器映射器，不配置处理器适配器，Spring会使用默认配置来完成工作！**</font>

这种实现接口Controller定义控制器是较老的办法，缺点是：**一个控制器中只有一个方法，如果要多个方法则需**

**要定义多个Controller；定义的方式比较麻烦；**

​	

### 5.1.2 方式二：使用注解@Controller

- `@Controller`注解类型用于声明Spring类的实例是一个控制器

- Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需

  要在配置文件中声明组件扫描。

  ```xml
  <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
  <context:component-scan base-package="com.xleixz.controller"/>
  ```

- 增加一个`ControllerTest2`类，使用注解实现；

  ```java
  //@Controller注解的类会自动添加到Spring上下文中，会被Spring接管
  @Controller
  public class ControllerTest2{
  
     //映射访问路径
     @RequestMapping("/t2")
     public String index(Model model){
         //Spring MVC会自动实例化一个Model对象用于向视图中传值
         model.addAttribute("msg", "ControllerTest2");
         //返回视图位置
         return "test";
    }
  }
  ```

​	

**可以发现，两个请求都可以指向一个视图，但是页面结果的结果是不一样的，从这里可以看出视图是被复**

**用的，而控制器与视图之间是弱偶合关系。**

<font color="red">**注解方式是平时使用的最多的方式**</font>

​	

## 5.2 RequestMapping

**@RequestMapping**

- @RequestMapping注解用于映射url到控制器类或一个特定的处理程序方法。可用于类或方法上。用于类上，表

  示类中的所有响应请求的方法都是以该地址作为父路径。

- 为了测试结论更加准确，我们可以加上一个项目名测试 myweb

- 只注解在方法上面

  ```java
  @Controller
  public class TestController {
     @RequestMapping("/h1")
     public String test(){
         return "test";
    }
  }
  ```

  访问路径：http://localhost:8080 / 项目名 / h1

- 同时注解类与方法

  ```java
  @Controller
  @RequestMapping("/admin")
  public class TestController {
     @RequestMapping("/h1")
     public String test(){
         return "test";
    }
  }
  ```

  访问路径：http://localhost:8080 / 项目名/ admin /h1  , 需要先指定类的路径再指定方法的路径。

​		

## 5.3 RestFul风格

**概念**

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以

更简洁，更有层次，更易于实现缓存等机制。

> 相当于传统的风格：http://localhost:8080/hello?method="add"
>
> RestFul风格：http://localhost:8080/hello/add

**功能**

资源：互联网所有的事物都可以被抽象为资源；

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。

**传统方式操作资源**  ：通过不同的参数来实现不同的效果！方法单一，post 和 get

​	http://127.0.0.1/item/queryItem.action?id=1 查询，GET

​	http://127.0.0.1/item/saveItem.action 新增，POST

​	http://127.0.0.1/item/updateItem.action 更新，POST

​	http://127.0.0.1/item/deleteItem.action?id=1 删除，GET或POST

**使用RESTful操作资源** ：可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！

​	http://127.0.0.1/item/1 查询，GET

​	http://127.0.0.1/item 新增，POST

​	http://127.0.0.1/item 更新，PUT

​	http://127.0.0.1/item/1 删除，DELETE

**测试**

1. 在新建一个类 RestFulController

   ```java
   @Controller
   public class RestFulController {
   }
   ```

2. 在Spring MVC中可以使用  `@PathVariable` 注解，让方法参数的值对应绑定到一个URI模板变量上。

   ```java
   //RestFul风格的请求：http://localhost:8080/SpringMVC_04_/add2/4/5
       @RequestMapping("/add2/{a}/{b}")
       public String test2(@PathVariable int a, @PathVariable int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "结果为" + res);
           return "test";
       }
   ```

3. 测试结果

   ![image-20220611230056430](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230056430.png)

4. 思考：使用路径变量的好处？

   - 使路径变得更加简洁；

   - 获得参数更加方便，框架会自动进行类型转换。

   - 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是

     的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

   ![image-20220611230602363](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230602363.png)

7. 修改下对应的String参数类型，再次测试

   ```java
   @RequestMapping("/add4/{a}/{b}")
       public String test4(@PathVariable int a, @PathVariable String b, Model model) {
           String res = a + b;
           model.addAttribute("msg", "结果为" + res);
           return "test";
       }
   ```

   ![image-20220611230338895](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230338895.png)

**使用method属性指定请求类型**

用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, 

TRACE等

测试一下：

- 增加一个方法

  ```java
  //映射访问路径,必须是POST请求
  @RequestMapping(value = "/hello",method = {RequestMethod.POST})
  public String index2(Model model){
     model.addAttribute("msg", "hello!");
     return "test";
  }
  ```

- 我们使用浏览器地址栏进行访问默认是Get请求，会报错405：

  ![image-20220611230622970](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230622970.png)

- 如果将POST修改为GET则正常了；

  ```java
  //映射访问路径,必须是Get请求
  @RequestMapping(value = "/hello",method = {RequestMethod.GET})
  public String index2(Model model){
     model.addAttribute("msg", "hello!");
     return "test";
  }
  ```

  ![image-20220611230706543](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230706543.png)

**小结：**

Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。

**所有的地址栏请求默认都会是 HTTP GET 类型的。**

方法级别的注解变体有如下几个：组合注解

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

`@GetMapping`是一个组合注解，平时使用的会比较多！

它所扮演的是 `@RequestMapping(method =RequestMethod.GET)` 的一个快捷方式。

**<font color = "green">TIP：</font>常用的为简化注解`@RequestMapping("/add2/{a}/{b}")`**。

​	

## 5.4 扩展：小黄鸭调试法

> 场景一：我们都有过向别人（甚至可能向完全不会编程的人）提问及解释编程问题的经历，但是很多时候就
>
> 在我们解释的过程中自己却想到了问题的解决方案，然后对方却一脸茫然。
>
> 场景二：你的同行跑来问你一个问题，但是当他自己把问题说完，或说到一半的时候就想出答案走了，留下
>
> 一脸茫然的你。

其实上面两种场景现象就是所谓的小黄鸭调试法（Rubber Duck Debuging），又称橡皮鸭调试法，它是我们软件工

程中最常使用调试方法之一。

![image-20220611233412547](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611233412547.png)

此概念据说来自《程序员修炼之道》书中的一个故事，传说程序大师随身携带一只小黄鸭，在调试代码的时候会在

桌上放上这只小黄鸭，然后详细地向鸭子解释每行代码，然后很快就将问题定位修复了。

---

​	

# 6、数据处理及跳转

## 6.1 结果跳转方式

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面。

> 页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
     id="internalResourceViewResolver">
   <!-- 前缀 -->
   <property name="prefix" value="/" />
   <!-- 后缀 -->
   <property name="suffix" value=".jsp" />
</bean>
```

​	

> **ServletAPI**

通过设置ServletAPI , 不需要视图解析器：

1. 通过HttpServletResponse进行输出

2. 通过HttpServletResponse实现重定向

3. 通过HttpServletRequest实现转发

```java
@Controller
public class ModelTest1 {

    @RequestMapping("/m1/t1")
    public String test1(HttpServletRequest request, HttpServletResponse response) {

        HttpSession session = request.getSession();
        System.out.println(session.getId());
        return "test";
    }

}
```

​	

> **SpringMVC**

**方式一：通过SpringMVC来实现转发和重定向 - 无需视图解析器；**

1. **注释掉视图解析器**：

![image-20220612002310664](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612002310664.png)

2. **Controller类**

   ```java
   @Controller
   public class ModelTest1 {
   
       @RequestMapping("/m1/t2")
       public String test2(Model model) {
           model.addAttribute("msg", "无视图解析器转发   /test.jsp");
           // 转发
           return "/test.jsp";
       }
   
       @RequestMapping("/m1/t3")
       public String test3(Model model) {
           model.addAttribute("msg", "无视图解析器转发2  forward:/index.jsp");
           // 转发2
           return "forward:/test.jsp";
       }
   
       @RequestMapping("/m1/t4")
       public String test4(Model model) {
           model.addAttribute("msg", "无视图解析器重定向  redirect:/index.jsp");
           // 重定向
           return "redirect:/index.jsp";
       }
   }
   ```

​	

**方式二：通过SpringMVC来实现转发和重定向 - 有视图解析器；**

有视图解析器默认是转发，重定向需要特写！

转发会拼接，重定向不拼接

1. **增加视图解析器**

   ```xml
   <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   ```

2. **Controller类**

   ```java
   @Controller
   public class ModelTest1 {
   
       @RequestMapping("/m1/t1")
       public String test1(HttpServletRequest request, HttpServletResponse response) {
   
           HttpSession session = request.getSession();
           System.out.println(session.getId());
           return "test";
       }
       
       @RequestMapping("/m2/t1")
       public String test5(Model model) {
           model.addAttribute("msg", "有视图解析器重定向   redirect:/index.jsp");
           //重定向
           return "redirect:/test.jsp";
           //return "redirect:hello.do"; //hello.do为另一个请求/
       }
   ```

​		

## 6.2 数据处理

> **提交的域名称和处理方法的参数名一致**，http://locaohost:8080/user/t1?name=xxxx

【User.java】

```java
public class User {
    private int id;
    private String name;
    private int age;

    public User() {
    }

    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

【UserController.java】

```java
@Controller
@RequestMapping("/user")
public class UserController {
	//提交的域名称和处理方法的参数名一致
    //http://localhost:8080/user/t1?name=XXX
    @RequestMapping("/t1")
    public String test1(String name, Model model) {
        //1. 接收前端参数
        System.out.println("接收到前端的参数为：" + name);
        //2.将返回的结果传递给前端
        model.addAttribute("msg", "接收到前端的参数为：" + name);
        //3. 跳转视图
        return "test";
    }
}
```

![image-20220612102407759](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612102407759.png)

​	

> **提交的域名称和处理方法的参数名不一致**，http://locaohost:8080/user/t1?username=xxxx
>
> 需要添加注解`@RequestParam("")`

```java
    //提交的域名称和处理方法的参数名不一致
    //http://localhost:8080/user/t1?username=XXX
    @RequestMapping("/t2")
    public String test2(@RequestParam("username") String name, Model model) {
        //1. 接收前端参数
        System.out.println("接收到前端的参数为：" + name);
        //2.将返回的结果传递给前端
        model.addAttribute("msg", "接收到前端的参数为：" + name);
        //3. 跳转视图
        return "test";
    }
```

![image-20220612103043624](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612103043624.png)

​	

> **提交的是一个对象**

实体类【User.java】

```java
public class User {
    private int id;
    private String name;
    private int age;

    public User() {
    }

    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

Controller类【UserController.java】

```java
//前端接收的为一个对象
    @RequestMapping("/t3")
    public String test3(User user) {
        System.out.println(user);
        return "test";
    }
```

![image-20220612103918790](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612103918790.png)

<font color="red">**说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null。**</font>

​	

## 6.3 数据显示到前端

> **方式一 : 通过ModelAndView**

【配置版】

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```

​		

> **方式二 : 通过ModelMap**

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("name",name);
   System.out.println(name);
   return "hello";
}
```

​	

> **方式三：通过Model**【最常用】

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, Model model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("msg",name);
   System.out.println(name);
   return "test";
}
```

​	

**对比**

> **Model** 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
>
> **ModelMap** 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
>
> **ModelAndView** 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。

​	

## 6.4 乱码问题

1. 新建一个表单【form.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <form action="<%=request.getContextPath()%>/e/t1" method="post">
       <input type="text" name="name">
       <input type="submit">
   </form>
   </body>
   </html>
   ```

2. 处理类【EncodingController.java】

   ```java
   @Controller
   public class EncodingController {
   
       @RequestMapping("/e/t1")
       public String test1(String name, Model model) {
           model.addAttribute("msg", name);
           return "test";
       }
   }
   ```

3. 编写一个跳转接收页面【test.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

4. 启动测试，输入中文出现乱码

   ![image-20220612111634228](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612111634228.png)

​	

> **处理办法一**
>
> 以前乱码问题通过过滤器解决，而SpringMVC给我们提供了一个过滤器 , 可以在【web.xml】中配置。
>
> 修改了xml文件需要重启服务器！
>
> <font color="red">**注意：<filter-mapping>中的 <url-pattern>，"\\"后面带\*号**</font>

```xml
<filter>
   <filter-name>encoding</filter-name>
   <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
   <init-param>
       <param-name>encoding</param-name>
       <param-value>utf-8</param-value>
   </init-param>
</filter>
<filter-mapping>
   <filter-name>encoding</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```

解决：

![image-20220612151517474](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612151517474.png)

如果使用注解`@RequestMapping("")`，它都能接收**GET**或**POST**方法，不影响；

但是发现，有些极端情况下，这个过滤器对**GET方法**的支持很不友好，建议在使用注解`@GETMapping("")`时不要

用这个过滤器。

​	

> **处理办法二**

1. 修改Tomcat配置文件，设置编码！

   ```xml
   <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
             connectionTimeout="20000"
             redirectPort="8443" />
   ```

2. 在**filter文件夹**下自定义过滤器【GenericEncodingFilter.java】

   ```java
   import javax.servlet.*;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletRequestWrapper;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.UnsupportedEncodingException;
   import java.util.Map;
   
   /**
   * 解决get和post请求 全部乱码的过滤器
   */
   public class GenericEncodingFilter implements Filter {
   
      @Override
      public void destroy() {
     }
   
      @Override
      public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
          //处理response的字符编码
          HttpServletResponse myResponse=(HttpServletResponse) response;
          myResponse.setContentType("text/html;charset=UTF-8");
   
          // 转型为与协议相关对象
          HttpServletRequest httpServletRequest = (HttpServletRequest) request;
          // 对request包装增强
          HttpServletRequest myrequest = new MyRequest(httpServletRequest);
          chain.doFilter(myrequest, response);
     }
   
      @Override
      public void init(FilterConfig filterConfig) throws ServletException {
     }
   
   }
   
   //自定义request对象，HttpServletRequest的包装类
   class MyRequest extends HttpServletRequestWrapper {
   
      private HttpServletRequest request;
      //是否编码的标记
      private boolean hasEncode;
      //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
      public MyRequest(HttpServletRequest request) {
          super(request);// super必须写
          this.request = request;
     }
   
      // 对需要增强方法 进行覆盖
      @Override
      public Map getParameterMap() {
          // 先获得请求方式
          String method = request.getMethod();
          if (method.equalsIgnoreCase("post")) {
              // post请求
              try {
                  // 处理post乱码
                  request.setCharacterEncoding("utf-8");
                  return request.getParameterMap();
             } catch (UnsupportedEncodingException e) {
                  e.printStackTrace();
             }
         } else if (method.equalsIgnoreCase("get")) {
              // get请求
              Map<String, String[]> parameterMap = request.getParameterMap();
              if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                  for (String parameterName : parameterMap.keySet()) {
                      String[] values = parameterMap.get(parameterName);
                      if (values != null) {
                          for (int i = 0; i < values.length; i++) {
                              try {
                                  // 处理get乱码
                                  values[i] = new String(values[i]
                                         .getBytes("ISO-8859-1"), "utf-8");
                             } catch (UnsupportedEncodingException e) {
                                  e.printStackTrace();
                             }
                         }
                     }
                 }
                  hasEncode = true;
             }
              return parameterMap;
         }
          return super.getParameterMap();
     }
   
      //取一个值
      @Override
      public String getParameter(String name) {
          Map<String, String[]> parameterMap = getParameterMap();
          String[] values = parameterMap.get(name);
          if (values == null) {
              return null;
         }
          return values[0]; // 取回参数的第一个值
     }
   
      //取所有值
      @Override
      public String[] getParameterValues(String name) {
          Map<String, String[]> parameterMap = getParameterMap();
          String[] values = parameterMap.get(name);
          return values;
     }
   }
   ```

3. 在【web.xml】中配置过滤器

   ```xml
   <filter>
           <filter-name>encoding</filter-name>
           <filter-class>com.xleixz.filter.GenericEncodingFilter</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/</url-pattern>
       </filter-mapping>
   ```

---

​	

# 7、JSON交互处理

> **前后端分离时代**

- 后端部署后端，提供接口，提供数据；

  ​                            ↓

     JSON（数据交换格式，他不是语言）

  ​                            ↓

- 前端独立部署，负责渲染后端的数据。

​	

> **什么是JSON？**

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、

数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

​	

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名

写在前面并用双引号 "" 包裹，使用冒号 `: `分隔，然后紧接着值：

```json
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```

可以这么理解：

**JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。**

```json
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```

​	

**JSON 和 JavaScript 对象互转**

要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法：

```java
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
```

要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法：

```java
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```

​	

**测试**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Title</title>
    <script type="text/javascript">
        //编写一个JavaScript对象
        var user = {
            name: "张三",
            age: 18,
            sex: "男"
        };
        //将js对象转换为JSON对象
        var json = JSON.stringify(user);
        //输出信息
        console.log(json);

        //将JSON对象转换为JavaScript对象
        var obj = JSON.parse(json);
        console.log(obj);

    </script>
</head>
<body>

</body>
</html>
```

​	

## 7.1 Controller返回JSON数据

Jackson应该是目前比较好的json解析工具了

当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。

这里使用Jackson，使用它需要导入它的jar包；

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
   <groupId>com.fasterxml.jackson.core</groupId>
   <artifactId>jackson-databind</artifactId>
   <version>2.9.8</version>
</dependency>
```

**导jar包**

```xml
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
		<dependency>
   			<groupId>com.fasterxml.jackson.core</groupId>
  			 <artifactId>jackson-databind</artifactId>
  			 <version>2.9.8</version>
		</dependency>
</dependencies>

    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```

**【web.xml】配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">

    <!--1.注册DispatchServlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!-- 启动顺序，数字越小，启动越早 -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--所有请求都会被springmvc拦截 -->
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

**【springmvc-servlet.xml】Spring配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
    <context:component-scan base-package="com.xleixz.controller"/>
    <mvc:default-servlet-handler />
    <mvc:annotation-driven />

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>

</beans>
```

**实体类【User.java】，导入`lombok`依赖可使用注解添加有参构造、无参构造、get、set方法**

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private int age;
    private String sex;

}
```

**这里需要两个新东西，一个是`@ResponseBody`注解，一个是`ObjectMapper`对象；**

`@ResponseBody`注解，就不会走视图解析器，不去找页面，直接返回一个字符串；

> **编写一个Controller**

```java
@Controller
public class UserController {

    @RequestMapping("/j1")
    //添加了ResponseBody注解，就不会走视图解析器，不去找页面，直接返回一个字符串
    @ResponseBody
    public String json1() throws JsonProcessingException {
        //jackson  ObjectMapper
        ObjectMapper objectMapper = new ObjectMapper();
        //创建一个对象
        User user = new User("小雷", 22, "男");
        String str = objectMapper.writeValueAsString(user);
        return str;
    }
}
```

**启动测试**成功，但是发现有乱码

![image-20220612164114510](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612164114510.png)



需要设置一下他的编码格式为utf-8，以及它返回的类型；通过`@RequestMaping`的`produces`属性来实现，

修改下代码

```java
@RequestMapping(value = "/j1",produces = "application/json;charset=utf-8")
```

**启动测试，乱码解决**。<font color="red">**但是这种方式过于繁琐，每个都需要写，所以SpringMVC给了解决方法，见下文。**</font>

![image-20220612164309007](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612164309007.png)

​		

## 7.2 代码优化

### 7.2.1 JSON乱码统一解决

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过Spring配置统一指定，这样就不

用每次都去处理了！

另一种方法可以在spr**ingmvc的配置文件**【springmvc-servlet.xml】（【Spring-mvc.xml】）上添加一段

`StringHttpMessageConverter`转换配置！

```xml
<mvc:annotation-driven>
   <mvc:message-converters register-defaults="true">
       <bean class="org.springframework.http.converter.StringHttpMessageConverter">
           <constructor-arg value="UTF-8"/>
       </bean>
       <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
           <property name="objectMapper">
               <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                   <property name="failOnEmptyBeans" value="false"/>
               </bean>
           </property>
       </bean>
   </mvc:message-converters>
</mvc:annotation-driven>
```

**Controller**

```java
@Controller
public class UserController {

    @RequestMapping("/j1")
    //添加了ResponseBody注解，就不会走视图解析器，不去找页面，直接返回一个字符串
    @ResponseBody
    public String json1() throws JsonProcessingException {
        //jackson  ObjectMapper
        ObjectMapper objectMapper = new ObjectMapper();
        //创建一个对象
        User user = new User("小雷", 22, "男");

        String str = objectMapper.writeValueAsString(user);
        return str;
    }
}
```

![image-20220612165530978](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612165530978.png)

​	

### 7.2.2 返回json字符串统一解决前后端分离开发

在类上直接使用`@RestController `，这样子，**里面所有的方法都只会返回 json 字符串了**，不用再每一个都添加

@ResponseBody ！我们在前后端分离开发中，一般都使用 `@RestController` ，十分便捷！

```java
@RestController
public class UserController {

   @RequestMapping("/json1")
   public String json1() throws JsonProcessingException {
       //创建一个jackson的对象映射器，用来解析数据
       ObjectMapper mapper = new ObjectMapper();
       //创建一个对象
       User user = new User("小雷",23, "男");
       //将我们的对象解析成为json格式
       String str = mapper.writeValueAsString(user);
       //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
       return str;
  }
}
```

​	

### 7.2.3 测试集合输出

> 增加一个新的方法

```java
@RequestMapping("/json2")
public String json2() throws JsonProcessingException {

   //创建一个jackson的对象映射器，用来解析数据
   ObjectMapper mapper = new ObjectMapper();
   //创建一个对象
   User user1 = new User("小雷1", 3, "男");
   User user2 = new User("小雷2", 3, "男");
   User user3 = new User("小雷3", 3, "男");
   User user4 = new User("小雷4", 3, "男");
   List<User> list = new ArrayList<User>();
   list.add(user1);
   list.add(user2);
   list.add(user3);
   list.add(user4);

   //将对象解析成为json格式
   String str = mapper.writeValueAsString(list);
   return str;
}
```

​	

### 7.2.4 输出时间对象

> 增加一个新的方法

```java
@RequestMapping("/json3")
public String json3() throws JsonProcessingException {

   ObjectMapper mapper = new ObjectMapper();

   //创建时间一个对象，java.util.Date
   Date date = new Date();
   //将对象解析成为json格式
   String str = mapper.writeValueAsString(date);
   return str;
}
```

![image-20220612171855320](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612171855320.png)

默认日期格式会变成一个数字，是1970年1月1日到当前日期的毫秒数！

Jackson 默认是会把时间转成timestamps形式

**解决方案：取消timestamps形式 ， 自定义时间格式**

```java
@RequestMapping("/json4")
public String json4() throws JsonProcessingException {

   ObjectMapper mapper = new ObjectMapper();

   //不使用时间戳的方式
   mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
   //自定义日期格式对象
   SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
   //指定日期格式
   mapper.setDateFormat(sdf);

   Date date = new Date();
   String str = mapper.writeValueAsString(date);

   return str;
}
```

​	

### 7.2.5 抽取为工具类

**如果要经常使用的话，这样是比较麻烦的，可以将这些代码封装到一个工具类中；**

```java
package com.kuang.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;

public class JsonUtils {
   
   public static String getJson(Object object) {
       return getJson(object,"yyyy-MM-dd HH:mm:ss");
  }

   public static String getJson(Object object,String dateFormat) {
       ObjectMapper mapper = new ObjectMapper();
       //不使用时间差的方式
       mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
       //自定义日期格式对象
       SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
       //指定日期格式
       mapper.setDateFormat(sdf);
       try {
           return mapper.writeValueAsString(object);
      } catch (JsonProcessingException e) {
           e.printStackTrace();
      }
       return null;
  }
}
```

使用工具类，代码会更加简洁！

```java
@RequestMapping("/json5")
public String json5() throws JsonProcessingException {
   Date date = new Date();
   String json = JsonUtils.getJson(date);
   return json;
}
```

​	

## 7.3 FastJSON

fastjson.jar是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象与JavaBean对象的转换，实现

JavaBean对象与json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法很多，最后的实现结

果都是一样的。

**fastjson 的 pom依赖！**

```xml
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>fastjson</artifactId>
   <version>1.2.60</version>
</dependency>
```

 fastjson 三个主要的类：

**JSONObject  代表 json 对象** 

- JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的。

- JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用诸如size()，

  isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的。

**JSONArray  代表 json 对象数组**

- 内部是有List接口中的方法来完成操作的。

**JSON代表 JSONObject和JSONArray的转化**

- JSON类源码分析与使用
- 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化。

**新建一个FastJsonDemo 类**

```java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.kuang.pojo.User;

import java.util.ArrayList;
import java.util.List;

public class FastJsonDemo {
   public static void main(String[] args) {
       //创建一个对象
       User user1 = new User("小雷1", 3, "男");
       User user2 = new User("小雷2", 3, "男");
       User user3 = new User("小雷3", 3, "男");
       User user4 = new User("小雷4", 3, "男");
       List<User> list = new ArrayList<User>();
       list.add(user1);
       list.add(user2);
       list.add(user3);
       list.add(user4);

       System.out.println("*******Java对象 转 JSON字符串*******");
       String str1 = JSON.toJSONString(list);
       System.out.println("JSON.toJSONString(list)==>"+str1);
       String str2 = JSON.toJSONString(user1);
       System.out.println("JSON.toJSONString(user1)==>"+str2);

       System.out.println("\n****** JSON字符串 转 Java对象*******");
       User jp_user1=JSON.parseObject(str2,User.class);
       System.out.println("JSON.parseObject(str2,User.class)==>"+jp_user1);

       System.out.println("\n****** Java对象 转 JSON对象 ******");
       JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
       System.out.println("(JSONObject) JSON.toJSON(user2)==>"+jsonObject1.getString("name"));

       System.out.println("\n****** JSON对象 转 Java对象 ******");
       User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
       System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>"+to_java_user);
  }
}
```

---

​	

# 8、Ajax异步加载技术

AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。

AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

AJAX 最大的优点是**在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。**

AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。

​	

> **伪造一个Ajax页面**

新建一个HTML，伪造一个Ajax页面

```html
<!DOCTYPE html>
<html>
<head lang="en">
   <meta charset="UTF-8">
   <title>kuangshen</title>
</head>
<body>

<script type="text/javascript">
   window.onload = function(){
       var myDate = new Date();
       document.getElementById('currentTime').innerText = myDate.getTime();
  };

   function LoadPage(){
       var targetUrl =  document.getElementById('url').value;
       console.log(targetUrl);
       document.getElementById("iframePosition").src = targetUrl;
  }

</script>

<div>
   <p>请输入要加载的地址：<span id="currentTime"></span></p>
   <p>
       <input id="url" type="text" value="https://www.baidu.com/"/>
       <input type="button" value="提交" onclick="LoadPage()">
   </p>
</div>

<div>
   <h3>加载页面位置：</h3>
   <iframe id="iframePosition" style="width: 100%;height: 500px;"></iframe>
</div>

</body>
</html>
```

![image-20220615184811888](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220615184811888.png)

​			

## 8.1 了解jQuery.Ajax

> **什么是jQuery？**

Ajax的核心是XMLHttpRequest对象(XHR)。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式

从服务器获取新数据。

jQuery 提供多个与 AJAX 有关的方法。

通过 jQuery AJAX 方法，能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时

您能够把这些外部数据直接载入网页的被选元素中。

jQuery 不是生产者，而是大自然搬运工。

jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

**jQuery是一个库，库里有js的大量函数（方法）**

​	

> **jQuery的下载**

jQuery 3.6.0 传送门：[jQuery官网（3.6.0）](https://jquery.com/ "点击下载查看jQuery 3.6.0")<font color="red">**3.6.0版本偶尔有问题，建议降级使用**</font>

jQuery 3.5.1 传送门：[jQuery 3.5.1](https://blog.jquery.com/2020/05/04/jquery-3-5-1-released-fixing-a-regression/ "点击下载jQuery 3.5.1版本")

![image-20220615185503612](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220615185503612.png)

![image-20220615185718008](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220615185718008.png)

![image-20220615194029980](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220615194029980.png)

​	

```markdown
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

​	

> **使用最原始的HttpServletResponse处理 , .最简单 , 最通用**

1. 配置【web.xml】和【ApplicationContext.xml】（Spring-mvc.xml）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:ApplicationContext.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
       <filter>
           <filter-name>encoding</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   
   </web-app>
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="com.xleixz.controller"/>
       <mvc:default-servlet-handler />
       <mvc:annotation-driven />
   
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   
   </beans>
   ```

2. 编写一个jsp测试页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>$Title$</title>
       <%--<script src="https://code.jquery.com/jquery-3.5.1.js"></script>--%>
   
   </head>
   <body>
   
   <%--onblur：失去焦点触发事件--%>
   用户名:<input type="text" id="txtName" onblur="a1()"/>
   
   
   
   <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
   <script>
       function a1() {
           //$符号=jquery 相当于jquery.post
           $.post({
               url: "${pageContext.request.contextPath}/ajax2",
               data: {'name': $("#txtName").val()},
               success: function (data, status) {
                   alert(data);
                   alert(status);
               }
           });
       }
   </script>
   </body>
   </html>
   ```

3. 在**jsp中**导入**jQuery** ， 可以使用在线的CDN ， 也可以下载导入

   ```jsp
   <%--<script src="https://code.jquery.com/jquery-3.5.1.js"></script>--%>
     <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
   ```

4. 控制层【AjaxController.java】

   ```java
   @RestController
   public class AjaxController {
   
       @RequestMapping("/ajax1")
       public String test() {
           return "hello";
       }
   
       @RequestMapping("/ajax2")
       public void ajax1(String name, HttpServletResponse response) throws IOException {
           if ("admin".equals(name)) {
               response.getWriter().print("true");
           } else {
               response.getWriter().print("false");
           }
       }
   
   }
   ```
   
   ![image-20220616092237533](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220616092237533.png)
   
   ​	
   
## 8.2 Ajax异步加载数据

   1. 实体类【User.java】
   
      ```java
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class User {
      
          private String name;
          private int age;
          private String sex;
      }
      ```
   
   2. 获取一个集合展示到前端【AjaxController.java】
   
      ```java
      @RestController
      public class AjaxController {
      @RequestMapping("/ajax3")
          public List<User> ajax3() {
              List<User> userArraryList = new ArrayList<User>();
              //添加数据
              userArraryList.add(new User("小雷", 23, "男"));
              userArraryList.add(new User("小红", 24, "女"));
              userArraryList.add(new User("小明", 25, "男"));
              userArraryList.add(new User("小白", 26, "男"));
              userArraryList.add(new User("小黑", 27, "男"));
              userArraryList.add(new User("小紫", 28, "男"));
              userArraryList.add(new User("小绿", 29, "男"));
              return userArraryList;
          }
      }
      ```
   
   3. 前端页面【ajaxtest2.jsp】
   
      ```jsp
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      
          <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
          <script>
              $(function () {
                  $("#btn").click(function () {
                      $.post("${pageContext.request.contextPath}/ajax3", function (data) {
                          // console.log(data);
                          var html = "";
                          for (let i = 0; i < data.length; i++) {
                              html += "<tr>" +
                                  "<td>" + data[i].name + "</td>" +
                                  "<td>" + data[i].age + "</td>" +
                                  "<td>" + data[i].sex + "</td>" +
                                  "</tr>"
                              $("#content").html(html);
                          }
                      })
      
                  })
              });
      
          </script>
      </head>
      <body>
      <input type="button" value="加载数据" id="btn">
      <%--画页面--%>
      <table>
          <tr>姓名</tr>
          <tr>年龄</tr>
          <tr>性别</tr>
          <tbody id="content">
          <%--数据: 在后台--%>
      
          </tbody>
      </table>
      
      </body>
      </html>
      ```

   ![Ajax](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/Ajax.gif)

   ​		

   ## 8.3 Ajax验证用户名登录

1. Controller

   ```java
   @RestController
   public class AjaxController {
   @RequestMapping("/ajax4")
       public String ajax3(String name,String pwd){
           String msg = "";
           //模拟数据库中存在数据
           if (name!=null){
               if ("admin".equals(name)){
                   msg = "OK";
               }else {
                   msg = "用户名输入错误";
               }
           }
           if (pwd!=null){
               if ("123456".equals(pwd)){
                   msg = "OK";
               }else {
                   msg = "密码输入有误";
               }
           }
           return msg; //由于@RestController注解，将msg转成json格式返回
       }
   
   }
   ```

2. 前端页面【login.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>ajax</title>
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
       <script>
   
           function a1() {
               $.post({
                   url: "${pageContext.request.contextPath}/ajax4",
                   data: {'name': $("#name").val()},
                   success: function (data) {
                       if (data.toString() == 'OK') {
                           $("#userInfo").css("color", "green");
                       } else {
                           $("#userInfo").css("color", "red");
                       }
                       $("#userInfo").html(data);
                   }
               });
           }
   
           function a2() {
               $.post({
                   url: "${pageContext.request.contextPath}/ajax4",
                   data: {'pwd': $("#pwd").val()},
                   success: function (data) {
                       if (data.toString() == 'OK') {
                           $("#pwdInfo").css("color", "green");
                       } else {
                           $("#pwdInfo").css("color", "red");
                       }
                       $("#pwdInfo").html(data);
                   }
               });
           }
   
       </script>
   </head>
   <body>
   <p>
       用户名:<input type="text" id="name" onblur="a1()"/>
       <span id="userInfo"></span>
   </p>
   <p>
       密码:<input type="text" id="pwd" onblur="a2()"/>
       <span id="pwdInfo"></span>
   </p>
   </body>
   </html>
   ```

![ajax用户](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/ajax用户.gif)

​	

## 8.4 SpringMVC拦截器

> **什么是拦截器？**

SpringMVC的处理器拦截器类似于Servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理。开发者可以自

己定义一些拦截器来实现特定的功能。

**过滤器与拦截器的区别：**拦截器是AOP思想的具体应用。

**过滤器**

- servlet规范中的一部分，任何java web工程都可以使用
- 在url-pattern中配置了`/*`之后，可以对所有要访问的资源进行拦截

**拦截器** 

- 拦截器是**SpringMVC框架自己的**，**只有使用了SpringMVC框架的工程才能使用**
- <font color="red">拦截器只会拦截访问的控制器方法， 如果访问的是jsp/html/css/image/js是不会进行拦截的</font>

​	

> **自定义拦截器**

想要自定义拦截器，必须实现 `HandlerInterceptor` 接口。

1、新建一个Moudule，springmvc-07-Interceptor，添加web支持

2、配置web.xml 和 springmvc-servlet.xml 文件

3、编写一个拦截器

```java
public class MyInterceptor implements HandlerInterceptor {

    //return true: 执行下一个拦截器
    //return false: 不执行下一个拦截器
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("=============处理前=============");
        return true;
    }

    //在请求处理方法执行之后执行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("=============处理后=============");
    }

    //在dispatcherServlet处理后执行,做清理工作.
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("=============清理=============");
    }
}
```

4、在SpringMVC的配置文件中配置拦截器

```xml
  <!--拦截器配置-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--/** 包括这个请求下的所有请求-->
            <mvc:mapping path="/**"/>
            <bean class="com.xleixz.config.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

5、编写一个controller类去接收请求

```java
@RestController
public class TestController {

    @RequestMapping("/t1")
    public String test1(){
        System.out.println("TestController方法执行了");
        return "OK";
    }
}
```

6、编写index.jsp测试拦截器

```jsp
<a href="${pageContext.request.contextPath}/interceptor">拦截器测试</a>
```

7、启动测试

![image-20220616160820866](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220616160820866.png)

​	

> **拦截器作用**

例如：实现用户必须登录才能进入页面等。

<font color="red">**若执行拦截前返回为false，则无法发送请求**</font>

![image-20220616161257623](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220616161257623.png)

![image-20220616161339060](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220616161339060.png)

​	

## 8.5 拦截器实现登录验证

1. 主页面【index.jsp】，并在页面上测试不登录能否进入页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>$Title$</title>
   </head>
   
   <h1><a href="${pageContext.request.contextPath}/user/goLogin">登录页面</a></h1>
   <h1><a href="${pageContext.request.contextPath}/user/main">首页</a></h1>
   </body>
   </html>
   ```

2. 登录页面【login.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <%--在WEB-INF下的所有页面或资源，只能通过controller或Servlet进行访问--%>
   <h1>登录页面</h1>
   <form action="${pageContext.request.contextPath}/user/login" method="post">
     用户名:<input type="text" name="username"/>
     密码:<input type="text" name="passowrd"/>
     <input type="submit" value="提交">
   </form>
   </body>
   </html>
   ```

3. 登录成功页面【main.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>登录成功</title>
   </head>
   <body>
   <h1>登录成功</h1>
   <span>${username}</span>
   <hr>
   <a href="${pageContext.request.contextPath}/user/logout">注销</a>
   </body>
   </html>
   ```

4. 编写Controller处理请求【LoginController.java】

   ```java
   @Controller
   @RequestMapping("/user")
   public class LoginController {
   
       @RequestMapping("/login")
       public String login(String username, String password, HttpSession session) {
               session.setAttribute("username", username);
           return "main";
       }
   
       @RequestMapping("/main")
       public String main() {
           return "main";
       }
   
       @RequestMapping("/goLogin")
       public String goLogin() {
           return "login";
       }
       //退出登陆
       @RequestMapping("logout")
       public String logout(HttpSession session) throws Exception {
           // session 过期
           session.invalidate();
           return "login";
       }
   }
   ```

5. 编写用户登录拦截器

   ```java
   public class LoginInterceptor implements HandlerInterceptor {
   
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           // 获取session
           HttpSession session = request.getSession();
           // 如果是登陆页面则放行
           System.out.println("uri: " + request.getRequestURI());
           if (request.getRequestURI().contains("login")) {
               return true;
           }
           // 如果用户已登陆也放行
           if(session.getAttribute("user") != null) {
               return true;
           }
   
           // 用户没有登陆跳转到登陆页面
           request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);
           return false;
       }
       }
   ```

6. 在Springmvc的配置文件中注册拦截器

   ```xml
   <!--拦截器配置-->
   <mvc:interceptor>
       <!--/** 包括这个请求下的所有请求-->
       <mvc:mapping path="/user/**"/>
       <bean class="com.xleixz.config.LoginInterceptor"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

7. 启动测试

   ![拦截器拦截登录](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/拦截器拦截登录.gif)

​	

## 8.6 文件上传和下载

> **准备工作**

文件上传是项目开发中最常见的功能之一，SpringMVC 可以很好的支持文件上传，但是SpringMVC上下文中默认没

有**装配MultipartResolver**，因此默认情况下其不能处理文件上传工作。如果想使用Spring的文件上传功能，则需要

在上下文中配置MultipartResolver。

前端表单要求：为了能上传文件，必须将表单的method设置为POST，并将enctype设置为multipart/form-data。只

有在这样的情况下，浏览器才会把用户选择的文件以二进制数据发送给服务器；

**对表单中的 enctype 属性做个详细的说明：**

- application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单

  会将表单域中的值处理成 URL 编码方式。

- multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件

  的内容也封装到请求参数中，不会对字符编码。

- text/plain：除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件。

```jsp
<form action="" enctype="multipart/form-data" method="post">
   <input type="file" name="file"/>
   <input type="submit">
</form>
```

一旦设置了enctype为multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对于文件上传的处

理则涉及在服务器端解析原始的HTTP响应。在2003年，Apache Software Foundation发布了开源的Commons 

FileUpload组件，其很快成为Servlet/JSP程序员上传文件的最佳选择。

- Servlet3.0规范已经提供方法来处理文件上传，但这种上传需要在Servlet中完成。
- 而Spring MVC则提供了更简单的封装。
- Spring MVC为文件上传提供了直接的支持，这种支持是用即插即用的MultipartResolver实现的。
- Spring MVC使用Apache Commons FileUpload技术实现了一个MultipartResolver实现类：
- CommonsMultipartResolver。因此，SpringMVC的文件上传还需要依赖Apache Commons FileUpload的组件。

​	

> **文件上传**

1. 导入文件上传的jar包，commons-fileupload，Maven会自动导入他的依赖包`commons-io`包；

   ```xml
    <!--文件上传-->
           <dependency>
               <groupId>commons-fileupload</groupId>
               <artifactId>commons-fileupload</artifactId>
               <version>1.3.3</version>
           </dependency>
           <!--servlet-api导入高版本的-->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>4.0.1</version>
           </dependency>
   ```

2. 在SpringMVC核心配置文件中配置**bean：multipartResolver**

   ```xml
   <!--文件上传配置-->
       <bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
           <!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容，默认为ISO-8859-1 -->
           <property name="defaultEncoding" value="utf-8"/>
           <!-- 上传文件大小上限，单位为字节（10485760=10M） -->
           <property name="maxUploadSize" value="10485760"/>
           <property name="maxInMemorySize" value="40960"/>
       </bean>
   ```

   CommonsMultipartFile 的 常用方法：

   - **String getOriginalFilename()：获取上传文件的原名**
   - **InputStream getInputStream()：获取文件流**
   - **void transferTo(File dest)：将上传文件保存到一个目录文件中**

3. 编写前端页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
     <head>
       <title>$Title$</title>
     </head>
     <body>
   <form action="${pageContext.request.contextPath}/upload" enctype="multipart/form-data" method="post">
     <input type="file" name="file" />
       <input type="submit" value="upload" />
   </form>
     </body>
   </html>
   ```

4. Controller类

   ```java
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.web.multipart.commons.CommonsMultipartFile;
   
   import javax.servlet.http.HttpServletRequest;
   import java.io.*;
   
   @RestController
   public class FileController {
   
       //@RequestParam("file") 将name=file控件得到的文件封装成CommonsMultipartFile 对象
       //批量上传CommonsMultipartFile则为数组即可
       @RequestMapping("/upload")
       public String fileUpload(@RequestParam("file") CommonsMultipartFile file , HttpServletRequest request) throws IOException {
   
           //获取文件名 : file.getOriginalFilename();
           String uploadFileName = file.getOriginalFilename();
   
           //如果文件名为空，直接回到首页！
           if ("".equals(uploadFileName)){
               return "redirect:/index.jsp";
           }
           System.out.println("上传文件名 : "+uploadFileName);
   
           //上传路径保存设置
           String path = request.getServletContext().getRealPath("/upload");
           //如果路径不存在，创建一个
           File realPath = new File(path);
           if (!realPath.exists()){
               realPath.mkdir();
           }
           System.out.println("上传文件保存地址："+realPath);
   
           InputStream is = file.getInputStream(); //文件输入流
           OutputStream os = new FileOutputStream(new File(realPath,uploadFileName)); //文件输出流
   
           //读取写出
           int len=0;
           byte[] buffer = new byte[1024];
           while ((len=is.read(buffer))!=-1){
               os.write(buffer,0,len);
               os.flush();
           }
           os.close();
           is.close();
           return "redirect:/index.jsp";
       }
   }
   ```

5. 启动Tomcat测试

   ![文件上传测试](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/文件上传测试.gif)

​	

> **采用file.Transto 来保存上传的文件**

1. 编写前端页面【index.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
     <head>
       <title>$Title$</title>
     </head>
     <body>
   <form action="${pageContext.request.contextPath}/upload2" enctype="multipart/form-data" method="post">
     <input type="file" name="file" />
     <input type="submit" value="upload2" />
   </form>
     </body>
   </html>
   ```

2. 编写Controller类

   ```java
   @RestController
   public class FileController {   
       /*
        * 采用file.Transto 来保存上传的文件
        */
       @RequestMapping("/upload2")
       public String  fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
   
           //上传路径保存设置
           String path = request.getServletContext().getRealPath("/upload");
           File realPath = new File(path);
           if (!realPath.exists()){
               realPath.mkdir();
           }
           //上传文件地址
           System.out.println("上传文件保存地址："+realPath);
   
           //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
           file.transferTo(new File(realPath +"/"+ file.getOriginalFilename()));
   
           return "redirect:/index.jsp";
       }
   }
   ```

​	

> **文件下载**

**文件下载步骤：**

1、设置 response 响应头

2、读取文件 -- InputStream

3、写出文件 -- OutputStream

4、执行操作

5、关闭流 （先开后关）

**代码实现：**

```java
@RequestMapping(value="/download")
public String downloads(HttpServletResponse response ,HttpServletRequest request) throws Exception{
   //要下载的图片地址
   String  path = request.getServletContext().getRealPath("/upload");
   String  fileName = "基础语法.jpg";

   //1、设置response 响应头
   response.reset(); //设置页面不缓存,清空buffer
   response.setCharacterEncoding("UTF-8"); //字符编码
   response.setContentType("multipart/form-data"); //二进制传输数据
   //设置响应头
   response.setHeader("Content-Disposition",
           "attachment;fileName="+URLEncoder.encode(fileName, "UTF-8"));

   File file = new File(path,fileName);
   //2、 读取文件--输入流
   InputStream input=new FileInputStream(file);
   //3、 写出文件--输出流
   OutputStream out = response.getOutputStream();

   byte[] buff =new byte[1024];
   int index=0;
   //4、执行 写出操作
   while((index= input.read(buff))!= -1){
       out.write(buff, 0, index);
       out.flush();
  }
   out.close();
   input.close();
   return null;
}
```

前端

```jsp
<a href="/download">点击下载</a>
```

​	

​	

<font color="green">**完结：2022-06-10**</font>









