Spring三件套:
Spring Core：提供了 IoC（Inverse of Control）容器，用于对象之间的解耦，通过容器自动将对象之间的依赖注入。同时，Spring Core 还提供了对 AspectJ 的集成，以及对基于注解的 Spring Bean 的支持。
Spring AOP：提供了基于 AOP（Aspect Oriented Programming）的编程方式，能够在不修改源代码的情况下，通过代理机制对对象进行增强，比如添加事务、日志、安全检查等功能。
Spring MVC：是 Spring 框架的 Web 模块，提供了 MVC（Model-View-Controller）模式的支持，用于处理 Web 请求和响应。Spring MVC 通过 DispatcherServlet、HandlerMapping、Controller 和 ViewResolver 等组件构成了完整的 MVC 模式的实现。

Spring Boot: a simple version of Spring
web server: Tomcat, which runs war files
	Spring Boot takes jar and output war files for tomcat 
# Init
https://start.spring.io/
Dependencies:
Spring Web, Spring Data JPA, PostgreSQL Driver
# File Structure
`target/` contains files generated during build
`pom.xml` Project Object Model
`src/` source code
	`src/main/java/` java code
	`src/main/resources/` static resources and templates
	`src/test` tests
# Database
JDBC (Java Database Connectivity) API
## ORM frameworks built on top of JDBS
ORM Object Relational Mapping: a technique to match object classes and tables in relational database
### JPA(Java Persistence API)/Hibernate (Hibernate implements JPA specification)
UserController uses UserService through dependency injection; UserService uses UserRepository through dependency injection.
UserRepository is an developer-defined interface, which will be implemented by JPA. 
### MyBatis
