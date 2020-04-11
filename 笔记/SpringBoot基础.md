# SpringBoot

## Chapter1.Spring Boot入门

### 1. Spring Boot简介

> SpringBoot对是整个Spring技术栈的整合

- SpringBoot基于底层Spring Data
- Spring Boot来简化Spring应用开发，**约定大于配置**，去繁从简，just run就能创建一个独立的，产品级别的应用
- 归根揭底SpringBoot或者SpringMVC应用本质上依然是**基于Spring的应用**
- 背景：
  - J2EE笨重的开发、繁多的配置、低下的开发效率、复杂的部署流程、第三方技术集成难度大
- 解决：
  - Spring全家桶时代
  - Spring Boot ->J2EE一站式解决方案
  - Spring Cloud ->分布式整体解决方案
- 优点
  - 快速创建独立运行的Spring项目以及与主流框架集成
  - 使用嵌入式的Servlet容器，应用无需打成WAR包
  - starters自动依赖与版本控制
  - 大量的自动配置，简化开发，也可以修改默认值
  - 无需配置XML，无代码生成，开箱即用
  - 准生产环境的运行时应用监控
  - 与云计算的天然集成
- **缺点：入门容易精通难(SpringBoot是基于Spring封装的)**

### 2.微服务简介

- 微服务：一种架构风格
- 一个应用应该是一组小型服务；可以通过HTTP的方式进行互通

- 每一个功能元素最终都是一个可独立替换和独立升级的软件单元

[官方文档](https://www.martinfowler.com/microservices/)

- 使用SpringBoot快速构建微服务单元，用SpringCloud进行分布式构建

- mvn settings

```xml
<profile>
	<id>jdk-1.8</id>
	<activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	</activation>
	<properties>
		<maven.compiler.source>1.8</maven.comiler.source>
		<maven.compiler.targer>1.8</maven.comiler.targer>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	</properties>
</profile>
```
### 3.Spring Boot HelloWorld

- 浏览器发送hello请求，服务器接收请求并处理，响应Hello World字符串



#### 1.创建Maven工程（jar)

#### 2.导入Spring Boot相关依赖

#### 3.编写一个主程序；启动Spring Boot应用

- ```@SpringBootApplication```来标注一个主程序类，说明是一个Spring Boot应用

- ```java
  SpringApplication.run(主程序类名.class,args);//启动主程序
  ```

#### 4.编写相关的Controller,Service

```java
@Controller
public class HelloController {
    //把hello world写给浏览器
    @ResponseBody
    //接收浏览器的hello请求
    @RequestMapping("/hello")
    public String hello(){
        return "Hello World!";
    }
}
```

- `@ResponseBody`的作用其实是将java对象转为json格式的数据。
- `@responseBody`注解的作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据。
  注意：在使用此注解之后不会再走视图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。
- ·```@RestController```=```@Controller+@ResponseBody```

- 使用`@RequestMapping` 注解映射请求路径

#### 5.运行主程序

#### 6.简化部署

```xml
<!--这个插件可以将应用打包成一个可执行的jar包-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

- 在maven的lifecycle中点击package就能打成jar包使用java -jar运行

- SpringBoot注入了嵌入式的Tomcat

### Hello World探究

#### 1.POM文件

##### 1.1父项目

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

它的父项目是
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.6.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
这个父项目来真正管理Spring Boot应用里面的所有依赖版本
Spring Boot的版本仲裁中心--->以后我们导入依赖默认是不需要写版本的(没有在dependecies里面管理的依赖项目则需要声明版本号)
```

##### 1.2启动器(starter)

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
starter-web里面是:
 <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.2.6.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.2.6.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.2.6.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
      <version>2.2.6.RELEASE</version>
      <scope>compile</scope>
      <exclusions>
        <exclusion>
          <artifactId>tomcat-embed-el</artifactId>
          <groupId>org.apache.tomcat.embed</groupId>
        </exclusion>
      </exclusions>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.2.5.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
```

- spring-boot-starter-web:
  - spring-boot-starter：spring-boot场景启动器：帮我们导入了web模块正常运行所以来的组件

- Spring Boot将所有的功能场景都抽取出来，做成了一个个的starters(启动器)，只需要在项目里面引入这些starter，相关场景的所有依赖都会导入进来。要用什么功能就导入什么starter

#### 2.Application主程序类(主入口类)

```java
@SpringBootApplication
public class SpringBoot01HelloworldApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot01HelloworldApplication.class, args);
    }

}
```

- ```@SpringBootApplication```标注在某个类上，说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用
- ```传入的SpingBoot01HelloworldApplication.class类```必须是由```@SpringBootApplication```所标注的类

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.springframework.boot.autoconfigure;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.context.TypeExcludeFilter;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};

    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```

***```@SpringBootConfiguration```：SpringBoot的配置类***

- 标注在某个类上，表示这是一个SpringBoot的配置类

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```

***它的底层```@Configuration```：Spring的配置类***

> - `@SpringBootConfiguration`来源于`@Configuration`
> - 二者功能都是将`当前类`标注为`配置类`
> - 并将`当前类`以`@Bean`注解标记的方法的实例注入到`spring容器中`，实例名即为方法名
> - `@Configuration`配置Spring容器，即JavaConfig形式的`Spring IoC`容器的配置类所使用
> - ```@Component```：配置类也是容器中的一个组件

- 配置类 --------- 配置文件

***```@EnableAutoConfiguration```：开启自动配置功能：***

- 以前我们需要配置的东西，Spring Boot帮我们自动配置
- 这个注解告诉SpringBoot开启自动配置的功能
- 点进去

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

> `@EnableAutoConfiguration`注解启用`自动配置`
>
> 其可以帮助`SpringBoot`应用将所有符合条件的`@Configuration`配置都加载到当前`IoC容器`之中

![](D:\JAVA基础\笔记\EnableConfiguration注解.png)

- `@AutoConfigurationPackage`：自动配置包
  - Spring的底层注解```@Import({Registrar.class})```，给容器中导入一个组件
  - 由```Import({AutoConfigurationImportSelector.class})```:指定导入哪些组件
    - 将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中
    - 最终会给容器中导入非常多的自动配置类(xxxAutoConfiguration)：就是给容器中导入这个场景需要的所有组件，并配置好这些组件。
  - 有了自动配置类，免去了手动配置的麻烦。
    - **SpringBoot 在启动的时候从类路径下的```META-INF/spring.factories```中获取```EnableAutoConfiguration```指定的值，并将`spring.factories`中的`EnableAutoConfiguration`对应的配置项通过`反射机制`实例化为对应标注了`@Configuration`的形式的`IoC容器配置类`，然后注入`IoC容器`.**

![SpringBoot自动配置](D:\SpringBoot\SpringBoot自动配置.PNG)

- **```@Import({Registrar.class})```将主配置类(```@SpringApplication```标注的类)的所在包及下面所有子包里面的所有组件扫描到Spring容器中。**

- 断点打到下面这个方法里可以看拿到的包信息

- ```java
  public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
      AutoConfigurationPackages.register(registry, (new AutoConfigurationPackages.PackageImport(metadata)).getPackageName());
  }
  ```

####  使用Spring Initializer创建Spring Boot项目

- resources文件夹目录结构

  - static：保存所有的静态资源：js,css,images

  - templates：保存所有的模板页面；(Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面)；可以使用模板引擎(freemarker、thymeleaf)

  - application.propertiles：Spring Boot应用的配置文件,可以修改一些默认设置

    

## Chapter2.Spring Boot配置

### 配置文件

- 配置文件的作用：(全局配置文件可以对一些默认配置值进行修改)
  - 修改SpringBoot自动配置的默认值；
  - SpringBoot在底层都给我们自动配置好

- Spring Boot使用一个全局的配置文件
  - application.properties
  - application.yml
- 配置文件放在```src/main/resources目录或者类路径/config```下
- .yml是YAML语言的文件，以数据为中心，比json、xml等更适合左配置文件

```yml
server:
  port: 8090
```

```xml
<server>
	<port>8090</port>
</server>
```

### YML语法

#### 1.基本语法

- k:(空格)v ：表示一对键值对(空格必须有)
- 以空格的缩进来控制层级关系：只要是左对齐的一列数据都是同一层级的

```yml
server: 
     port: 8081
     path: /index
```

- 属性和值也是大小写敏感

##### #### 2.值的写法

- 字面量：普通的值(数字，字符串，布尔)：
  - key:(空格)v：字面量直接写
  - 字符串默认不用加上单引号双引号
  - ""：双引号：不会转义字符串里面的特殊字符，特殊字符会作为本身想表示的意思
    - name: "zhangsan \n lisi"：输出：zhangsan 换行 lisi
  - ''：单引号：会转义特殊字符，特殊字符最终只是一个普通的字符串输出
    - name: "zhangsan \n lisi"：输出：zhangsan \n lisi

- 对象、Map(属性和值-->键值对)：
  - key:(空格)v：对象还是键值对的方式，在下一行来写对象的属性和值的关系：注意缩进

```yml
- friends: 
-       lastName: zhangsan
-       age: 20
```

- 行内写法：

```yaml
friends: {lastName: zhangsan,age: 20}
```

- 数组(List、Set)：
  - 用-(空格) 值表示数组中的一个元素

```yaml
pets: 
  - cat
  - dog
  - pig
```

```yaml
pets: [cat,dog,pig]
```

#### 3.配置文件值注入(获取配置文件)

- 配置文件

```yaml
server:
  port: 8090

person:
  lastNmae: zhangsan
  age: 20
  boss: false
  birth: 2017/12/12
  maps: {k1: v1,k2: 12}
  lists:
    - lisi
    - wangwu
  dog:
    name: 小狗
    age: 3
```

JavaBean

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
 * prefix = "person"配置文件中哪个下面的所有属性进行一一映射
 * 只有这个组件时容器中的组件，才能使用容器提供的功能(@Component加载进容器)
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
```

##### @Vlaue获取值和@ConfigurationProperties获取值比较

|                    | @ConfigurationProperties | @Value       |
| ------------------ | ------------------------ | ------------ |
| 功能               | 批量注入配置文件中的属性 | 一个一个指定 |
| 松散绑定(松散语法) | 支持                     | 不支持       |
| ```SpEL```         | 不支持                   | 支持         |
| JSR303数据校验     | 支持                     | 不支持       |
| 复杂类型封装       | 支持                     | 不支持       |

- 如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用```@Value```
- 如果说，我们专门编写了一个javaBean来和配置文件进行映射，使用```@ConfigurationProperties```，默认从全局配置文件中获取值

##### 配置文件注入值数据校验

```@Validated```

```@Email```

##### 4.@PropertySource & @ImportResource

##### @PropertySouce

- 加载指定的配置文件、

```java
@PropertySource(value = {"classpath:person.properties"})
```

##### @ImportResouce

- 导入Spring的配置文件，让配置文件里面 的内容生效

```java
//注入ioc容器
@Autowired
ApplicationContext ioc;

@Test
public void testHelloService(){
    boolean b = ioc.containsBean("helloService");
    System.out.println(b);
    //output:false
}
```

- Spring Boot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别
- 如果想让Spring的配置文件生效，加载进来
- `@ImportResouce`标记在配置类上

```java
@ImportResource(locations = {"classpath:beans.xml"})
```

- **SpringBoot推荐给容器中添加组件的方式：推荐全注解的方式**
  - 配置类(相当于Spring配置文件)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="helloservice" class="cn.cqu.ccc1z.springboot.springboot02config.service.HelloService"></bean>
</beans>
```

```java
package cn.cqu.ccc1z.springboot.springboot02config.config;

import cn.cqu.ccc1z.springboot.springboot02config.service.HelloService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 指明当前类是一个配置类；就是来替代之前的Spring配置文件
 *
 *  在配置文件中用<bean></bean>标签加组件
 */
@Configuration
public class MyAppConfig {

    //将方法 的返回值添加到容器中；容器中这个组件默认的id就是方法名
    @Bean
    public HelloService helloService(){
        return new HelloService();
    }
}
```

`@Configuration @Bean`

#### 4.配置文件占位符

- 随机数 

> `random.value`、`{random.int}`、`${random.long}`
>
> `randon.int(10)`、`random.int[1024,655336]`

- 占位符获取之前配置的值，如果没有可以使用:指定默认值

```properties
person.last-name=张三${random.uuid}
person.age=${random.int}
person.dog.name=${peroson.hello:hello}_dog
```

### 5.Profile

- `Profile`是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

#### 1.多Profile文件

- 我们在主配置文件编写的时候，文件名可以是 `application-{profile}.properties/yml`

>`application-dev.properties`：开发环境
>
>`application-prod.properties`：生产环境

- 默认使用`application.properties`的配置

#### 2.yaml支持 多文档快方式

- 以`---`分割文档块

```yaml
server:
  port: 8090
spring:
  profiles:
    active: dev
#上面这段代码用于激活
---
server:
  port: 8083
spring:
  profiles:dev

---
server:
  port: 8084
spring:
  profiles: prod #指定该文档块属于哪一个环境
```

#### 3.激活指定Profile

- 在配置文件中指定`spring.profiles.active=dev`

- 命令行：`--spring.profiels.active=dev`(在RunConfiguration里的Program arguments里输入命令行或者打成jar包后在cmd里输入)
- 虚拟机参数
  - -Dspring.profiles.active=dev(在RunConfiguration里的VM Options里输入命令行)

### 6.配置文件加载位置

- spring boot启动会扫描一以下位置的`application.properties`或者`application.yml`文件作为Spring boot的`默认配置文件`
  - `-file:./config`
  - `-file:./`
  - `-classpath:/config/`
  - `-classpath:/`
- 以上是按照`优先级从高到底`的顺序，**所有位置的文件都会被加载，互补配置**

- `高优先级配置内容会覆盖低优先级配置内容`
- 也可以通过配置`spring.config.location`来改变默认配置
  - 项目打好包后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件 的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置

#### 7.外部配置加载顺序(需要的时候再来看)

#### 8.自动配置原理

- [配置文件能配置的属性参照](https://docs.spring.io/spring-boot/docs/2.0.0.RC1/reference/htmlsingle/#common-application-properties)

##### **自动配置原理(非常关键)**

##### `@EnbaleAutoConfiguration`

- SpringBoot启动的时候加载`主配置类`，开启了`自动配置功能@EnableAutoConfiguration`

- `@EnableAutoConfiguration`作用:
  - 利用`@Import({AutoConfigurationImportSelector.class})`给容器中导入一些组件
  - 可以查看`selectImport`方法内容

```java
public String[] selectImports(AnnotationMetadata annotationMetadata) {
    if (!this.isEnabled(annotationMetadata)) {
        return NO_IMPORTS;
    } else {
        AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader.loadMetadata(this.beanClassLoader);
        AutoConfigurationImportSelector.AutoConfigurationEntry autoConfigurationEntry = this.getAutoConfigurationEntry(autoConfigurationMetadata, annotationMetadata);
        return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
    }
}
```

```java
return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
```

```java
public List<String> getConfigurations() {
    return this.configurations;
}
```

- 1.x SpringBoot

```java
SpringFactoriesLoader.loadFactoryNames()
扫描所有jar包类路径下的 META-INF/spring.factories
把扫描到的这些文件的内容包装成properties对象
从properties中获取到EnableAutoConfiguration.class类(类名)对应的值，然后把他们添加在容器中
```

- **将类路径下 `META-INF/spring.factories`里面配置的所有`EnableAutoConfiguration`的值加入到了容器中**

```properties
# Initializers
org.springframework.context.ApplicationContextInitializer=\
org.springframework.boot.autoconfigure.SharedMetadataReaderFactoryContextInitializer,\
org.springframework.boot.autoconfigure.logging.ConditionEvaluationReportLoggingListener

# Application Listeners
org.springframework.context.ApplicationListener=\
org.springframework.boot.autoconfigure.BackgroundPreinitializer

# Auto Configuration Import Listeners
org.springframework.boot.autoconfigure.AutoConfigurationImportListener=\
org.springframework.boot.autoconfigure.condition.ConditionEvaluationReportAutoConfigurationImportListener

# Auto Configuration Import Filters
org.springframework.boot.autoconfigure.AutoConfigurationImportFilter=\
org.springframework.boot.autoconfigure.condition.OnBeanCondition,\
org.springframework.boot.autoconfigure.condition.OnClassCondition,\
org.springframework.boot.autoconfigure.condition.OnWebApplicationCondition

# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudServiceConnectorsAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveRestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.jest.JestAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.rest.RestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketRequesterAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketServerAutoConfiguration,\
org.springframework.boot.autoconfigure.rsocket.RSocketStrategiesAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.rsocket.RSocketSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.saml2.Saml2RelyingPartyAutoConfiguration,\
org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.servlet.OAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.reactive.ReactiveOAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.servlet.OAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.reactive.ReactiveOAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskSchedulingAutoConfiguration,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration

# Failure analyzers
org.springframework.boot.diagnostics.FailureAnalyzer=\
org.springframework.boot.autoconfigure.diagnostics.analyzer.NoSuchBeanDefinitionFailureAnalyzer,\
org.springframework.boot.autoconfigure.flyway.FlywayMigrationScriptMissingFailureAnalyzer,\
org.springframework.boot.autoconfigure.jdbc.DataSourceBeanCreationFailureAnalyzer,\
org.springframework.boot.autoconfigure.jdbc.HikariDriverConfigurationFailureAnalyzer,\
org.springframework.boot.autoconfigure.session.NonUniqueSessionRepositoryFailureAnalyzer

# Template availability providers
org.springframework.boot.autoconfigure.template.TemplateAvailabilityProvider=\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.mustache.MustacheTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafTemplateAvailabilityProvider,\
org.springframework.boot.autoconfigure.web.servlet.JspTemplateAvailabilityProvider

```

- 每一个这样的`xxxAutoConfiguration`类都是容器中的一个组件，都加入到容器中
  - 用他们来做自动配置

- 3).每一个自动配置类进行自动配置功能

- 4).以`HttpEncodingAutoConfiguration(Http编码自动配置)`为例解释自动配置原理

```java
@Configuration(
    proxyBeanMethods = false
)//表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@EnableConfigurationProperties({HttpProperties.class})//启动指定类(HttpProperties)的ConfigurationProperties功能，将配置文件中对应的值和HttpProperties绑定起来

@ConditionalOnWebApplication(
    type = Type.SERVLET
)//Spring底层@Conditional注解，根据不同的条件，如果满足指定的条件整个配置类里面的配置就会生效。		判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnClass({CharacterEncodingFilter.class})
//判断当前项目有没有这个类`CharacterEncodingFilter`-->SpringMVC中进行解决乱码的过滤器
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)//判断配置文件中是否存在某个配置 前缀："spring.http.encoding"里是否存在enabled,如果不存在，判断也是true.
public class HttpEncodingAutoConfiguration {
    
    //它已经和SpringBoot的配置文件映射了
    private final Encoding properties;
	
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }

    @Bean//给容器中添加一个组件 return filter
    @ConditionalOnMissingBean
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }

```

- 根据当前不同的条件判断，决定这个配置类是否生效?
  - **一旦这个配置类生效，这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的`properties`类中获取的，这些类里面的每一个属性又是和配置文件绑定的**。
- 5).所有在配置文件中能配置的属性都是在`xxxxProperties`类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties(
    prefix = "spring.http"
)//从配置文件中获取指定的值和bean的属性进行绑定
public class HttpProperties {
```

**SpringBoot精髓：**

- **1.SpringBoot启动会加载大量的自动配置类**
- **2.我们看我们需要的功能有没有SpringBoot默认写好的自动配置类**
- **3.我们再来看这个自动配置类中到底配置了哪些组件**
  - **只要我们要用的组件由，我们就不用配了**
- **4.给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这些属性的值。**

**`xxxxAutoConfiguration`：自动配置类->给容器中添加组件**

**`xxxxProperties`：封装配置文件中相关的属性**

##### 2.细节

- `@Conditional派生注解`：进行相应的条件判断
  
- 必须是`@Conditional`指定的条件成立，才给容器中添加组件，配置类里面的所有内容才生效
  
- 自动配置类必须在一定的条件下才能生效，所以有些自动配置类不一定生效。
- 我们怎么知道哪些自动配置类生效了?

- 在配置文件中

- ```properties
  #开启SpringBoot的debug功能
  debug=true
  ```

- 启动`debug=true`让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效

## Chapter3.Spring Boot与日志

### 1.日志框架选择

- 目前市面上的日志框架
  - JUL，JCL，Jboss-logging，logback，log4j，log4j2，slf4j...

| 日志门面(接口,日志的一个抽象层)                              | 日志实现                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| JCL(Jakarta Commons Logging) SLF4j(Simple Logging Facade for java) jboss-logging | Log4j JUL(java.util.logging) Log4j2 Logback |

- 左边选一个门面(抽象层)，右边选一个实现
  - **门面：SLF4j**
  - **实现：Logback**

- SpringBoot：底层是Spring框架，Spring框架默认使用JCL
  - SpringBoot选用SLF4和logback

### 2.SLF4j使用

#### 1.如何在系统中使用SLF4j

- [官网](http://www.slf4j.org/)

- 以后开发的时候，日志记录方法的调用
- 不应该`直接调用之日的实现类`，而是`调用日志抽象层面的方法`

**给系统里面到日slf4j的jar和logback的事件jar**

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

### 2.遗留问题

- a系统(slf4j+logback)：Spring(commors-logging)、Hibernate(jboss-logging).....

**统一日志记录**

- 即使是别的框架，也得和我一起使用slf4j进行输出

- [原理和使用方法](http://www.slf4j.org/legacy.html)

![](D:\SpringBoot\legacy.PNG)

**如何让系统中所有的日志都统一到slf4j**

> - **1.将系统中其他日志框架先排除出去**
> - **2.用中间包来替换原有的日志框架**
>
> - **3.再导入slf4j其他的实现**

### 3.SpringBoot日志关系

```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.2.6.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

- SpringBoot使用它来做日志功能

```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
      <version>2.2.6.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

- SpringBoot日志底层依赖关系

![](D:\SpringBoot\SpringBoot日志关系.PNG)

**总结**

> - 1.SpringBoot底层也是使用`slf4j`+`logback`的方式进行日志记录
> - 2.SpringBoot也把其他的日志都替换成了slf4j

***如果我们要引入其他框架，一定要把这个框架的默认日志依赖移除掉***

***SpringBoot能自动适配所有日志，而且底层使用`slf4j`+`logback`的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉***



### 4.日志使用

#### 1.默认配置

- Springboot默认帮我们配置好了日志

```java
@SpringBootTest
class SpringBoot03LoggingApplicationTests {
    //日志记录器
    Logger logger = LoggerFactory.getLogger(getClass());

    @Test
    void contextLoads() {
        //System.out.println();

        //日志的级别
        //由低到高 trace->debug->info->warn->error
        //可以调整输出的日志级别：日志就只会在这个级别及以后的高级别生效
        logger.trace(() -> "这是trace日志...");
        logger.debug(() -> "这是debug日志...");
        //SpringBoot默认给我们使用的是info级别，没有在properties指定级别的就用SpringBoot默认规定级别：root级别
        logger.info(() -> "这是info日志...");
        logger.warn(() -> "这是warn日志...");
        logger.error(()->"这是error日志...");
    }

}
```

```properties
#调整com.ccc1z包路径下日志的级别
logging.level.com.ccc1z=trace
#在当前磁盘的根路径下创建spring文件夹和里面的log文件，使用spring.log作为默认文件
logging.file.path=/spring/log
#在控制台输出的日志的格式
logging.pattern.console=
#在文件中日志输出的格式
logging.pattern.file=
```

## 4.Spring Boot与Web开发

#### 1.使用SpringBoot

- 1.创建SpringBoot应用
- 2.SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来
- 3.自己编写业务代码

***每引入一个场景，都要去考虑SpringBoot帮我们配置了什么?能不能修改?能修改哪些配置?能不能扩展***

```java
@xxxxxxAutoConfiguration：帮我们向容器中自动配置组件
@xxxxxxProperties：配置类来封装配置文件的内容
```

#### 2.SpringBoot对静态资源的映射规则

- SpringBoot关于静态资源的映射规矩都在`WebMvcAutoConfiguration`里

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
    } else {
        Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
        CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
        if (!registry.hasMappingForPattern("/webjars/**")) {
            this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{"/webjars/**"}).addResourceLocations(new String[]{"classpath:/META-INF/resources/webjars/"}).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
        }

        String staticPathPattern = this.mvcProperties.getStaticPathPattern();
        if (!registry.hasMappingForPattern(staticPathPattern)) {
            this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{staticPathPattern}).addResourceLocations(WebMvcAutoConfiguration.getResourceLocations(this.resourceProperties.getStaticLocations())).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
        }

    }
}
```

- 1.`if (!registry.hasMappingForPattern("/webjars/**"))`所有`webjars`都去`classpath:/META-INF/resources/webjars/`找资源
  - `webjars`：以jar包的方式引入静态资源
  - [官网](https://www.webjars.org/)

```xml
<!--引入jquery-webjar-->在访问的时候只需要写webjars下面的资源名称即可
<dependency>
    <groupId>org.webjars.bower</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.0</version>
</dependency>
```

- 2."/**"：访问当前项目的任何资源(静态资源的文件夹)

```
"classpath:/META-INF/resouces/"
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
"/"：当前项目的根路径
```

`http://localhost:8080/asserts/js/Chart.min.js`

- 3.欢迎页：静态资源文件夹下的所有Index.html页面；被`/**`映射

#### 3.模板引擎

- JSP，Velocity，Freemarker，Thymeleaf

![](D:\SpringBoot\01尚硅谷SpringBoot核心技术篇\Spring Boot 笔记+课件\images\template-engine.png)

**SpringBoot推荐的Thymeleaf**

- 语法更简单，功能更强大

##### 1.引入thymeleaf(引入starter)

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

##### 2.Thymeleaf使用&语法

- `autoconfiguration/thymeleaf/properties`:

```java
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";
    private Charset encoding;
    private boolean cache;
    private Integer templateResolverOrder;
    private String[] viewNames;
    private String[] excludedViewNames;
    private boolean enableSpringElCompiler;
    private boolean renderHiddenMarkersBeforeCheckboxes;
    private boolean enabled;
    private final ThymeleafProperties.Servlet servlet;
    private final ThymeleafProperties.Reactive reactive;
```

```java
public static final String DEFAULT_PREFIX = "classpath:/templates/";
```

- 只要把HTML页面放在`"classpath:/templates/"`，Thymeleaf就能帮你自动渲染了

**语法**

[官方文档](https://www.thymeleaf.org/)

- 1.导入`thmeleaf`的名称空间

```html
<html xmlns:th="http://www.thymeleaf.org">
```

- 2.使用式例

```java
@Controller
public class HelloController {

    //查出一些数据，在页面展示
    @RequestMapping("/success")
    public String success(Map<String,Object> map){
        map.put("hello","你好");
        return "success";
    }
```



```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>成功</h1>
    <!--th:text 将div里面的本文内容设置为指定的数据-->
    <div th:text="${hello}">这是显示欢迎信息</div>
</body>
</html>
```

- **语法规则**

> 1.`th:text`：改变当前元素里面的文本内容
>
> `th:任意属性`：来替换原生属性的值

![](D:\SpringBoot\01尚硅谷SpringBoot核心技术篇\Spring Boot 笔记+课件\images\2018-02-04_123955.png)

**表达式**

```properties
Simple expressions:(表达式语法)
	Variable Expressions: ${...}：获取变量值：底层OGNL 
		1).获取对象的属性，调用方法
		2).使用内置的基本对象
			#ctx : the context object.
			#vars: the context variables.
			#locale : the context locale.
			#request : (only in Web Contexts) the HttpServletRequest object.
			#response : (only in Web Contexts) the HttpServletResponse object.
			#session : (only in Web Contexts) the HttpSession object.
			#servletContext : (only in Web Contexts) the ServletContext object.
	Selection Variable Expressions: *{...}
	Message Expressions: #{...}
	Link URL Expressions: @{...}
	Fragment Expressions: ~{...}
Literals:(字面量)
	Text literals: 'one text' , 'Another one!' ,…
	Number literals: 0 , 34 , 3.0 , 12.3 ,…
	Boolean literals: true , false
	Null literal: null
	Literal tokens: one , sometext , main ,…
Text operations:(文本操作)
	String concatenation: +
	Literal substitutions: |The name is ${name}|
Arithmetic operations:(数学运算)
	Binary operators: + , - , * , / , %
	Minus sign (unary operator): -
Boolean operations:(布尔运算)
	Binary operators: and , or
	Boolean negation (unary operator): ! , not
	Comparisons and equality:
Comparators: > , < , >= , <= ( gt , lt , ge , le )
	Equality operators: == , != ( eq , ne )
Conditional operators:(条件运算)
	If-then: (if) ? (then)
	If-then-else: (if) ? (then) : (else)
	Default: (value) ?: (defaultvalue)
Special tokens:
Page 17 of 106
No-Operation
```

#### SpringMVC自动配置

### 27.1.1 Spring MVC Auto-configuration

- ### 1. Spring MVC auto-configuration

  Spring Boot 自动配置好了SpringMVC

  以下是SpringBoot对SpringMVC的默认配置:**==（WebMvcAutoConfiguration）==**

  - Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.

    - 自动配置了ViewResolver（视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？））
    - ContentNegotiatingViewResolver：组合所有的视图解析器的；
    - ==如何定制：我们可以自己给容器中添加一个视图解析器；自动的将其组合进来；==

  - Support for serving static resources, including support for WebJars (see below).静态资源文件夹路径,webjars

  - Static `index.html` support. 静态首页访问

  - Custom `Favicon` support (see below).  favicon.ico

    

  - 自动注册了 of `Converter`, `GenericConverter`, `Formatter` beans.

    - Converter：转换器；  public String hello(User user)：类型转换使用Converter
    - `Formatter`  格式化器；  2017.12.17===Date；

  ```java
  		@Bean
  		@ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")//在文件中配置日期格式化的规则
  		public Formatter<Date> dateFormatter() {
  			return new DateFormatter(this.mvcProperties.getDateFormat());//日期格式化组件
  		}
  ```

  ​	==自己添加的格式化器转换器，我们只需要放在容器中即可==

  - Support for `HttpMessageConverters` (see below).

    - HttpMessageConverter：SpringMVC用来转换Http请求和响应的；User---Json；

    - `HttpMessageConverters` 是从容器中确定；获取所有的HttpMessageConverter；

      ==自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中（@Bean,@Component）==

      

  - Automatic registration of `MessageCodesResolver` (see below).定义错误代码生成规则

  - Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).

    ==我们可以配置一个ConfigurableWebBindingInitializer来替换默认的；（添加到容器）==

    ```
    初始化WebDataBinder；
    请求数据=====JavaBean；
    ```

  **org.springframework.boot.autoconfigure.web：web的所有自动场景；**

  If you want to keep Spring Boot MVC features, and you just want to add additional [MVC configuration](https://docs.spring.io/spring/docs/4.3.14.RELEASE/spring-framework-reference/htmlsingle#mvc) (interceptors, formatters, view controllers etc.) you can add your own `@Configuration` class of type `WebMvcConfigurerAdapter`, but **without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter` or `ExceptionHandlerExceptionResolver` you can declare a `WebMvcRegistrationsAdapter` instance providing such components.

  If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`.

  ### 2、扩展SpringMVC

  ```xml
      <mvc:view-controller path="/hello" view-name="success"/>
      <mvc:interceptors>
          <mvc:interceptor>
              <mvc:mapping path="/hello"/>
              <bean></bean>
          </mvc:interceptor>
      </mvc:interceptors>
  ```

  **==编写一个配置类（@Configuration），是WebMvcConfigurerAdapter类型；不能标注@EnableWebMvc==**;

  既保留了所有的自动配置，也能用我们扩展的配置；

  ```java
  //使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
  @Configuration
  public class MyMvcConfig extends WebMvcConfigurerAdapter {
  
      @Override
      public void addViewControllers(ViewControllerRegistry registry) {
         // super.addViewControllers(registry);
          //浏览器发送 /atguigu 请求来到 success
          registry.addViewController("/atguigu").setViewName("success");
      }
  }
  ```

  原理：

  ​	1）、WebMvcAutoConfiguration是SpringMVC的自动配置类

  ​	2）、在做其他自动配置时会导入；@Import(**EnableWebMvcConfiguration**.class)

  ```java
      @Configuration
  	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration {
        private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
  
  	 //从容器中获取所有的WebMvcConfigurer
        @Autowired(required = false)
        public void setConfigurers(List<WebMvcConfigurer> configurers) {
            if (!CollectionUtils.isEmpty(configurers)) {
                this.configurers.addWebMvcConfigurers(configurers);
              	//一个参考实现；将所有的WebMvcConfigurer相关配置都来一起调用；  
              	@Override
               // public void addViewControllers(ViewControllerRegistry registry) {
                //    for (WebMvcConfigurer delegate : this.delegates) {
                 //       delegate.addViewControllers(registry);
                 //   }
                }
            }
  	}
  ```

  ​	3）、容器中所有的WebMvcConfigurer都会一起起作用；

  ​	4）、我们的配置类也会被调用；

  ​	效果：SpringMVC的自动配置和我们的扩展配置都会起作用；

  ### 3、全面接管SpringMVC；

  SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己配置；所有的SpringMVC的自动配置都失效了

  **我们需要在配置类中添加@EnableWebMvc即可；**

  ```java
  //使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
  @EnableWebMvc
  @Configuration
  public class MyMvcConfig extends WebMvcConfigurerAdapter {
  
      @Override
      public void addViewControllers(ViewControllerRegistry registry) {
         // super.addViewControllers(registry);
          //浏览器发送 /atguigu 请求来到 success
          registry.addViewController("/atguigu").setViewName("success");
      }
  }
  ```

  原理：

  为什么@EnableWebMvc自动配置就失效了；

  1）@EnableWebMvc的核心

  ```java
  @Import(DelegatingWebMvcConfiguration.class)
  public @interface EnableWebMvc {
  ```

  2）、

  ```java
  @Configuration
  public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
  ```

  3）、

  ```java
  @Configuration
  @ConditionalOnWebApplication
  @ConditionalOnClass({ Servlet.class, DispatcherServlet.class,
  		WebMvcConfigurerAdapter.class })
  //容器中没有这个组件的时候，这个自动配置类才生效
  @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
  @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
  @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class,
  		ValidationAutoConfiguration.class })
  public class WebMvcAutoConfiguration {
  ```

  4）、@EnableWebMvc将WebMvcConfigurationSupport组件导入进来；

  5）、导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能；

  

  ## 5、如何修改SpringBoot的默认配置

  模式：

  ​	1）、SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来；

  ​	2）、在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置

  ​	3）、在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制配置

## 6. RestfulCRUD

- 1. 默认访问首页

```java
package com.ccc1z.springboot.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry){
        registry.addViewController("/").setViewName("success");
    }

    //所有WebConfiguraerAdapter组件都会一起起作用
    @Bean//将组件注册在容器
    public WebMvcConfigurer webMvcConfigurer(){
        WebMvcConfigurer webMvcConfigurer = new WebMvcConfigurer(){
            @Override
            public void addViewControllers(ViewControllerRegistry registry){
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/login.html").setViewName("login");
            }
        };
        return webMvcConfigurer;
    }
}
```

- 2.国际化
  1).