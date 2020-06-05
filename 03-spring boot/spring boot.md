## 1. Hello World入门程序

浏览器发送hello请求，服务器接受请求并处理，响应Hello World字符串；

1. 创建maven工程
2. 导入spring boot相关依赖

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

3. 编写一个主程序：启动Spring Boot应用

```java
/**
 *  @SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {

        // Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

4. 编写相关的Controller、Service

```java
@Controller
public class HelloController {

    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return "Hello World!";
    }
}
```

5. 运行并在浏览器访问

## 2. Hello World探究

### POM文件

- 1、父项目

  ```xml
  <parent>
  	<groupId>org.springframework.boot</groupId>
  	<artifactId>spring-boot-starter-parent</artifactId>
  	<version>1.5.9.RELEASE</version>
  </parent>
  他的父项目是
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>1.5.9.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
  
  他来真正管理Spring Boot应用里面的所有依赖版本---版本锁定
  ```

- 2、启动器

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  ```

  - spring-boot-starter：spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件；

  - Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器

### 主程序类，主入口类

```java
/**
 *  @SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {

        // Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

- `@SpringBootApplication`
  - Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；点进去发现包含如下注解：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
/**
* @SpringBootConfiguration->@Configuration->@Component
* Spring Boot的配置类：标注在某个类上，表示这是一个Spring Boot的配置类
* @Configuration：配置类上来标注这个注解
* 配置类-----配置文件；配置类也是容器中的一个组件；@Component
**/
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
      @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

- `@EnableAutoConfiguration`
  - 开启自动配置功能----------以前我们需要配置的东西，Spring Boot帮我们自动配置；@**EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置才能生效；

```java
@AutoConfigurationPackage
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
```

- @**AutoConfigurationPackage**：自动配置包

- @**Import**(AutoConfigurationPackages.Registrar.class)：Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器

- @**Import**(EnableAutoConfigurationImportSelector.class)：给容器中导入组件**EnableAutoConfigurationImportSelector**：导入哪些组件的选择器；

  将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中

  会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景需要的所有组件，并配置好这些组件；		

  ![1590414641156](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\1590414641156.png)

  有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；

- SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader)；

  Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；==以前我们需要自己配置的东西，自动配置类都帮我们；

  J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-1.5.9.RELEASE.jar；

## 3. 使用Spring Initializer快速创建Spring Boot项目

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目；

选择我们需要的模块；向导会联网创建Spring Boot项目；

默认生成的Spring Boot项目；

- 主程序已经生成好了，我们只需要我们自己的逻辑
- resources文件夹中目录结构
  - static：保存所有的静态资源； js css  images；
  - templates：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）；
  - application.properties：Spring Boot应用的配置文件；可以修改一些默认设置；

## 4. 配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的

- application.properties

- application.yml

配置文件的作用：修改SpringBoot自动配置的默认值---SpringBoot在底层都给我们自动配置好；

### YAML语法

#### 基本语法：

- `k:(空格)v`：表示一对键值对（空格必须有）；
  - 以**空格**的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的；
  - 属性和值都是大小写敏感；

#### 值的写法：

- 字面量：普通的值（数字，字符串，布尔）

  - 字面量直接写，字符串默认不用加上单引号或者双引号

  ```yaml
  server:
  	port: 8081
  	path: /hello
  ```

- 对象、Map（属性和值）（键值对）：在下一行来写对象的属性和值的关系

  ```YAML
  person:
  	name: arnold
  	age: 18
  # 行内写法
  person: {name: arnold,age: 18}
  ```

- 数组（List、Set）：用- 值表示数组中的一个元素

  ```yaml
  pets:
  	- cat
  	- dog
  	- pig
  # 行内写法
  pets: [cat,dog,pig]
  ```

## 5. 配置文件注入

1. 配置文件

```yaml
person:
    lastName: hello
    age: 18
    boss: false
    birth: 2017/12/12
    maps: {k1: v1,k2: 12}
    lists:
      - 李素
      - 王八
    dog:
      name: 小狗
      age: 12
```

2. 对应的javaBean：将配置文件中配置的每一个属性的值，映射到这个组件中

- @Component: 只有这个组件是容器中的组件，才能容器提供的@ConfigurationProperties功能；
  @ConfigurationProperties(prefix = "person"): 告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定；prefix = "person"：配置文件中哪个下面的所有属性进行一一映射

```java
@Data
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
}
```

3. 导入配置文件处理器，配置文件进行绑定就会有提示

```xml
<!--导入配置文件处理器，配置文件进行绑定就会有提示-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>
```

4. 用`@Value`获取值或用`@ConfigurationProperties`获取值

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

- 如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value；

```java
@RestController
public class GetValueController {

    @Value("${person.lastName}")
    private String name;

    @RequestMapping("/hello")
    public String hello(){
        return "hello "+name;
    }
}
```

- 如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties；

```java
@RunWith(SpringRunner.class)
@SpringBootTest
class SpringbootQuickstartApplicationTests {

    @Autowired
    Person person;
    
    @Test
    public void contextLoads() {
        System.out.println(person);
    }
}
// 输出：
// Person(lastName=hello, age=18, boss=false, birth=Tue Dec 12 00:00:00 CST 2017, maps={k1=v1, k2=12}, lists=[李素, 王八], dog=Dog(name=小狗, age=12))
```

- 配置文件中数据校验

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {
   //lastName必须是邮箱格式
    @Email
    private String lastName;
    private Integer age;
    private Boolean boss;

    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
```

5. `@PropertySource`：加载指定的配置文件

```java
@Data
@PropertySource("classpath:person.yml")
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
}
```

6. `@ImportResource`：导入Spring的配置文件，让配置文件里面的内容生效；

- 想让Spring的配置文件生效，加载进来；@**ImportResource**标注在一个配置类上

  ```java
  //导入Spring的配置文件让其生效
  @ImportResource(locations = {"classpath:beans.xml"})
  @SpringBootApplication
  public class SpringbootQuickstartApplication {
  
      public static void main(String[] args) {
          SpringApplication.run(SpringbootQuickstartApplication.class, args);
      }
  
  }
  ```

- SpringBoot推荐给容器中添加组件的方式；推荐使用全注解的方式

  1、配置类`@Configuration`------>Spring配置文件

  2、使用`@Bean`给容器中添加组件

  ```java
  /**
   * @Configuration：指明当前类是一个配置类；就是来替代之前的Spring配置文件
   * 在配置文件中用<bean><bean/>标签添加组件
   */
  @Configuration
  public class MyAppConfig {
      //将方法的返回值添加到容器中；容器中这个组件默认的id就是方法名
      @Bean
      public HelloService helloService02(){
          System.out.println("配置类@Bean给容器中添加组件了...");
          return new HelloService();
      }
  }
  ```

## 6. Profile

1. 多Profile文件

- 我们在主配置文件编写的时候，文件名可以是   application-{profile}.properties/yml
  - application-dev.yml
  - application-prod.yml

- 默认使用application.properties的配置；

```yml
server:
  port: 8081
spring:
  profiles:
    active: prod
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 8084
spring:
  profiles: prod  #指定属于哪个环境
```

2. 激活指定profile

- 在配置文件中指定  `spring.profiles.active=dev`

- 命令行：

  ​		`java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev`

  ​		可以直接在测试的时候，配置传入命令行参数

- 虚拟机参数

  ​		`-Dspring.profiles.active=dev`

## 7. 配置文件加载的位置

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

- file:./config/

- file:./

- classpath:/config/

- classpath:/

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

SpringBoot会从这四个位置全部加载主配置文件；**互补配置**；

我们还可以通过spring.config.location来改变默认的配置文件位置

**项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；**

- java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=G:/application.properties

## 8. 自动配置原理

配置文件到底能写什么？怎么写？自动配置原理；

[配置文件能配置的属性参照](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common-application-properties)

**精髓：**

​	**1、SpringBoot启动会加载大量的自动配置类**

​	**2、我们看我们需要的功能有没有SpringBoot默认写好的自动配置类；**

​	**3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）**

​	**4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这些属性的值；**

给某个配置类配置属性的步骤：

1. 找到组件对应的`xxxAutoConfiguration`：自动配置类，给容器中添加组件

![1590496531934](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\1590496531934.png)

2. 找到对应配置类中的`xxxProperties`：封装配置文件中相关属性---根据前缀和相应属性在配置文件中指定值

![1590496751775](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\1590496751775.png)

## 9. 日志系统

**市面上的日志框架：**

JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....

| 日志门面  （日志的抽象层）                                   | 日志实现                                             |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| ~~JCL（Jakarta  Commons Logging）~~    SLF4j（Simple  Logging Facade for Java）    **~~jboss-logging~~** | Log4j  JUL（java.util.logging）  Log4j2  **Logback** |

- 左边选一个门面（抽象层）、右边来选一个实现

- 日志门面：  SLF4J

- 日志实现：Logback

SpringBoot：底层是Spring框架，Spring框架默认是用JCL；‘

​	**SpringBoot选用 SLF4j和logback**

### SLF4j使用

- 开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法；

  给系统里面导入slf4j的jar和  logback的实现jar

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

- 每一个日志的实现框架都有自己的配置文件。使用slf4j以后，**配置文件还是做成日志实现框架自己本身的配置文件；**

![image-20200529152642584](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200529152642584.png)

- a（slf4j+logback）: Spring（commons-logging）、Hibernate（jboss-logging）、MyBatis、xxxx

  统一日志记录，即使是别的框架和我一起统一使用slf4j进行输出？

![image-20200529152817015](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200529152817015.png)

**如何让系统中所有的日志都统一到slf4j**

1. 将系统中其他日志框架先排除出去

2. 用中间包来替换原有的日志框架

3. 我们导入slf4j其他的实现

### spring boot日志系统

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
```

SpringBoot使用它来做日志功能：

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-logging</artifactId>
		</dependency>
```

底层依赖关系：

![image-20200529153225606](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200529153225606.png)

总结：

​	1. SpringBoot底层也是使用slf4j+logback的方式进行日志记录

​	2. SpringBoot也把其他的日志都替换成了slf4j；

3. 中间替换包？

![image-20200529153428427](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200529153428427.png)

4. 若我们要引入其他框架（如Spring框架用的是commons-logging），一定要把这个框架的默认日志依赖移除掉

   ```xml
   <dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-core</artifactId>
   			<exclusions>
   				<exclusion>
   					<groupId>commons-logging</groupId>
   					<artifactId>commons-logging</artifactId>
   				</exclusion>
   			</exclusions>
   		</dependency>
   ```

> SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可

### spring boot日志使用

1. **默认配置**

```java
	//记录器
	Logger logger = LoggerFactory.getLogger(getClass());
	@Test
	public void contextLoads() {
		//System.out.println();
		//日志的级别；
		//由低到高   trace<debug<info<warn<error
		//可以调整输出的日志级别；日志就只会在这个级别以以后的高级别生效
		logger.trace("这是trace日志...");
		logger.debug("这是debug日志...");
		//SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
		logger.info("这是info日志...");
		logger.warn("这是warn日志...");
		logger.error("这是error日志...");

	}
```

- 日志的级别
  - 由低到高   trace<debug<info<warn<error

> SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别

- SpringBoot修改日志的默认配置

```properties
logging.level.com.atguigu=trace
#logging.path=
# 不指定路径在当前项目下生成springboot.log日志
# 可以指定完整的路径；
#logging.file=G:/springboot.log

# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log 作为默认文件
logging.path=/spring/log

#  在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%n
```

2. **指定配置**

给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用他默认配置的了

| Logging System          | Customization                                                |
| ----------------------- | ------------------------------------------------------------ |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

- 如类路径下有logback.xml：直接就被日志框架识别了

- **logback-spring.xml**：日志框架就不直接加载日志的配置项，由SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能

```xml
<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
			%d表示日期时间，
			%thread表示线程名，
			%-5level：级别从左显示5个字符宽度
			%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
			%msg：日志消息，
			%n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>
```

> 如果使用logback.xml作为日志配置文件，还要使用profile功能，会有以下错误 :`no applicable action for [springProfile]`

## 10. web开发

### spring boot对静态资源的映射规则

```java
@ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
public class ResourceProperties implements ResourceLoaderAware {
  //可以设置和静态资源有关的参数，缓存时间等
```

```java
WebMvcAuotConfiguration：
		@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
			Integer cachePeriod = this.resourceProperties.getCachePeriod();
			if (!registry.hasMappingForPattern("/webjars/**")) {
				customizeResourceHandlerRegistration(
						registry.addResourceHandler("/webjars/**")
								.addResourceLocations(
										"classpath:/META-INF/resources/webjars/")
						.setCachePeriod(cachePeriod));
			}
			String staticPathPattern = this.mvcProperties.getStaticPathPattern();
          	//静态资源文件夹映射
			if (!registry.hasMappingForPattern(staticPathPattern)) {
				customizeResourceHandlerRegistration(
						registry.addResourceHandler(staticPathPattern)
								.addResourceLocations(
										this.resourceProperties.getStaticLocations())
						.setCachePeriod(cachePeriod));
			}
		}

        //配置欢迎页映射
		@Bean
		public WelcomePageHandlerMapping welcomePageHandlerMapping(
				ResourceProperties resourceProperties) {
			return new WelcomePageHandlerMapping(resourceProperties.getWelcomePage(),
					this.mvcProperties.getStaticPathPattern());
		}

       //配置喜欢的图标
		@Configuration
		@ConditionalOnProperty(value = "spring.mvc.favicon.enabled", matchIfMissing = true)
		public static class FaviconConfiguration {

			private final ResourceProperties resourceProperties;

			public FaviconConfiguration(ResourceProperties resourceProperties) {
				this.resourceProperties = resourceProperties;
			}

			@Bean
			public SimpleUrlHandlerMapping faviconHandlerMapping() {
				SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
				mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
              	//所有  **/favicon.ico 
				mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
						faviconRequestHandler()));
				return mapping;
			}

			@Bean
			public ResourceHttpRequestHandler faviconRequestHandler() {
				ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
				requestHandler
						.setLocations(this.resourceProperties.getFaviconLocations());
				return requestHandler;
			}

		}
```

1. 所有 /webjars/，都去classpath:/META-INF/resources/webjars/找资源

   > webjars：以jar包的方式引入静态资源----->http://www.webjars.org/

![image-20200530223520745](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200530223520745.png)

- 在pom文件中引入坐标即可

```xml
<!--引入jquery-webjar-->在访问的时候只需要写webjars下面资源的名称即可
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery</artifactId>
			<version>3.3.1</version>
		</dependency>
```

- 访问：localhost:8080/webjars/jquery/3.3.1/jquery.js

2. "/**" 访问当前项目的任何资源，都去（静态资源的文件夹）找映射

   > 1、"classpath:/META-INF/resources/"
   > 2、"classpath:/resources/"
   > 3、"classpath:/static/"
   > 4、"classpath:/public/" 
   > 5、"/"：当前项目的根路径

3. 欢迎页：静态资源文件夹下的所有`index.html`页面；被"/**"映射

4. 头像：所有的 **/`favicon.ico`  都是在静态资源文件下找

### 模版引擎

JSP、Velocity、Freemarker、Thymeleaf ------> SpringBoot推荐的Thymeleaf

1. 引入thymeleaf

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
    2.1.6    	
</dependency>

切换thymeleaf版本
<properties>
		<thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>
		<!-- 布局功能的支持程序  thymeleaf3主程序  layout2以上版本 -->
		<!-- thymeleaf2   layout1-->
		<thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>
  </properties>
```

>  thymeleaf3主程序  layout2以上版本
>
> thymeleaf2-----layout1

2. 使用thymeleaf

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = Charset.forName("UTF-8");

	private static final MimeType DEFAULT_CONTENT_TYPE = MimeType.valueOf("text/html");

	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";
  	//
```

> 只要我们把HTML页面放在`classpath:/templates/`，thymeleaf就能自动渲染

```html
<!DOCTYPE html>
<!--导入thymeleaf的名称空间-->
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>成功！</h1>
    <!--th:text 将div里面的文本内容设置为 -->
    <div th:text="${hello}">这是显示欢迎信息</div>
</body>
</html>
```

3. thymeleaf语法

   ![image-20200530230752194](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200530230752194.png)

   - `th:text`：改变当前元素里面的文本内容
   - `th:任意html属性`：来替换原生属性的值
   - `${...}`：获取变量值；OGNL
     - 获取对象的属性、调用方法
     - 使用内置的基本对象
     - 内置的一些工具对象
   - `*{...}`：选择表达式：和${}在功能上是一样
   - `#{...}`：获取国际化内容
   - `@{...}`：定义URL
   - `~{...}`：片段引用表达式

### SpringMVC自动配置

1. Spring Boot 自动配置好了SpringMVC ----- `WebMvcAutoConfiguration`

   - Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans

     - 自动配置了ViewResolver（视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？））
     - ContentNegotiatingViewResolver：组合所有的视图解析器的；
     - 如何定制？我们可以自己给容器中添加一个视图解析器；自动的将其组合进来

   - Support for serving static resources, including support for WebJars (see below).静态资源文件夹路径,webjars

   - Static `index.html` support. 静态首页访问

   - Custom `Favicon` support (see below).  favicon.ico

   - 自动注册了 of `Converter`, `GenericConverter`, `Formatter` beans.

     - Converter：转换器；  public String hello(User user)：类型转换使用Converter
     - `Formatter`  格式化器；  2017.12.17===Date
     - 自己添加的格式化器转换器，我们只需要放在容器中即可

     ```java
     @Bean
     @ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")//在文件中配置日期格式化的规则
     public Formatter<Date> dateFormatter() {
     	return new DateFormatter(this.mvcProperties.getDateFormat());//日期格式化组件
     }
     ```

- - Support for `HttpMessageConverters`
    - HttpMessageConverter：SpringMVC用来转换Http请求和响应的；User---Json；
    - `HttpMessageConverters` 是从容器中确定；获取所有的HttpMessageConverter；
    - 自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中（@Bean,@Component）
  - Automatic registration of `MessageCodesResolver` (see below).定义错误代码生成规则
  - Automatic use of a `ConfigurableWebBindingInitializer` bean

### 扩展SpringMVC

将如下配置拓展到springboot中

```xml
 <mvc:view-controller path="/hello" view-name="success"/>
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>
            <bean></bean>
        </mvc:interceptor>
    </mvc:interceptors>
```

`编写一个配置类（@Configuration），是WebMvcConfigurerAdapter类型`

> 不标注@EnableWebMvc，既保留了所有的自动配置，也能用我们扩展的配置
>
> 标注@EnableWebMvc，全面接管springMVC，所有都是我们自己配置；所有的SpringMVC的自动配置都失效了

```java
//使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       // super.addViewControllers(registry);
        //浏览器发送 /atguigu 请求来到 success
        registry.addViewController("/hello").setViewName("success");
    }
}
```

### 修改SpringBoot的默认配置方法总结

1. SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；如果有些组件可以有多个（ViewResolver），则将用户配置的和自己默认的组合起来；
2. 在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置
3. 在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制配置

## 11. Restful CRUD

### 设置默认访问首页

https://www.hangge.com/blog/cache/detail_2528.html

### Restful的请求架构

| 实验功能                             | 请求URI  | 请求方式 |
| ------------------------------------ | -------- | -------- |
| 查询所有员工                         | emps     | GET      |
| 查询某个员工(来到修改页面)           | emp/{id} | GET      |
| 来到添加页面                         | emp      | GET      |
| 添加员工                             | emp      | POST     |
| 来到修改页面（查出员工进行信息回显） | emp/{id} | GET      |
| 修改员工                             | emp      | PUT      |
| 删除员工                             | emp/{id} | DELETE   |

```java
//员工删除
    @DeleteMapping("/emp/{id}")
    public String deleteEmployee(@PathVariable("id") Integer id){
        employeeDao.delete(id);
        return "redirect:/emps";
    }
```

```html
<button th:attr="del_uri=@{/emp/}+${emp.id}" class="btn btn-sm btn-danger deleteBtn">删除</button>
```

## 12. 错误处理机制

### spring boot默认错误处理效果

1. 浏览器：返回一个默认的错误页面

![image-20200602115119365](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200602115119365.png)

浏览器的请求头：`text/html-`----标志是浏览器访问

![image-20200602115226857](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200602115226857.png)

2. 如果是其他客户端：默认响应一个json数据（请求头为"`*/*`"）

![image-20200602115351972](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200602115351972.png)

![image-20200602115422457](C:\Users\Arnold\AppData\Roaming\Typora\typora-user-images\image-20200602115422457.png)

### spring boot默认错误处理效果的原理

参照`ErrorMvcAutoConfiguration`；错误处理的自动配置-------给容器中添加了以下组件：

- `DefaultErrorAttributes`：帮我们在页面共享信息；

```java
@Override
	public Map<String, Object> getErrorAttributes(RequestAttributes requestAttributes,
			boolean includeStackTrace) {
		Map<String, Object> errorAttributes = new LinkedHashMap<String, Object>();
		errorAttributes.put("timestamp", new Date());
		addStatus(errorAttributes, requestAttributes);
		addErrorDetails(errorAttributes, requestAttributes, includeStackTrace);
		addPath(errorAttributes, requestAttributes);
		return errorAttributes;
	}
```

- `BasicErrorController`：处理默认`/error`请求

```java
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {
    
    @RequestMapping(produces = "text/html")//产生html类型的数据；浏览器发送的请求来到这个方法处理
	public ModelAndView errorHtml(HttpServletRequest request,
			HttpServletResponse response) {
		HttpStatus status = getStatus(request);
		Map<String, Object> model = Collections.unmodifiableMap(getErrorAttributes(
				request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
		response.setStatus(status.value());
        
        //去哪个页面作为错误页面；包含页面地址和页面内容
		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
		return (modelAndView == null ? new ModelAndView("error", model) : modelAndView);
	}

	@RequestMapping
	@ResponseBody    //产生json数据，其他客户端来到这个方法处理；
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		Map<String, Object> body = getErrorAttributes(request,
				isIncludeStackTrace(request, MediaType.ALL));
		HttpStatus status = getStatus(request);
		return new ResponseEntity<Map<String, Object>>(body, status);
	}
```

- `ErrorPageCustomizer`：错误页面跳转规则

```java
	@Value("${error.path:/error}")
	private String path = "/error";  系统出现错误以后来到error请求进行处理；（web.xml注册的错误页面规则）
```

- `DefaultErrorViewResolver`：跳转到某个页面---`"error/" + viewName`

```java
@Override
	public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status,
			Map<String, Object> model) {
		ModelAndView modelAndView = resolve(String.valueOf(status), model);
		if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
			modelAndView = resolve(SERIES_VIEWS.get(status.series()), model);
		}
		return modelAndView;
	}

	private ModelAndView resolve(String viewName, Map<String, Object> model) {
        //默认SpringBoot可以去找到一个页面？  error/404
		String errorViewName = "error/" + viewName;
        
        //模板引擎可以解析这个页面地址就用模板引擎解析
		TemplateAvailabilityProvider provider = this.templateAvailabilityProviders
				.getProvider(errorViewName, this.applicationContext);
		if (provider != null) {
            //模板引擎可用的情况下返回到errorViewName指定的视图地址
			return new ModelAndView(errorViewName, model);
		}
        //模板引擎不可用，就在静态资源文件夹下找errorViewName对应的页面   error/404.html
		return resolveResource(errorViewName, model);
	}
```

总结：

> 一但系统出现4xx或者5xx之类的错误：`ErrorPageCustomizer`就会生效（定制错误的响应规则）；就会来到`/error`请求，被`BasicErrorController`处理；响应页面，去哪个页面是由`DefaultErrorViewResolver`解析得到的。

### 定制错误响应

自定义异常

```java
public class UserNotExistException extends RuntimeException {
    public UserNotExistException() {
        super("用户不存在");
    }
}
```

#### **1. 定制错误页面**

- 有模板引擎的情况下：`error/状态码`
  - 将错误页面命名为  `错误状态码.html` ，放在模板引擎文件夹里面的 error文件夹下，发生此状态码的错误就会来到对应的页面

> 我们可以使用4xx和5xx作为错误页面的文件名来匹配这种类型的所有错误，但精确优先（优先寻找精确的状态码.html）

- 没有模板引擎（模板引擎找不到这个错误页面），静态资源文件夹下找
- 以上都没有错误页面，就是默认来到SpringBoot默认的错误提示页面

- 页面能获取的信息：

​				timestamp：时间戳

​				status：状态码

​				error：错误提示

​				exception：异常对象

​				message：异常消息

​				errors：JSR303数据校验的错误都在这里

#### 2. 定制错误的json数据

1. 自定义异常处理&返回定制json数据

```java
@ControllerAdvice
public class MyExceptionHandler {
    @ResponseBody
    @ExceptionHandler(UserNotExistException.class)
    public Map<String,Object> handleException(Exception e){
        Map<String,Object> map = new HashMap<>();
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        return map;
    }
}
```

2. 转发到/error进行自适应响应效果处理

```java
 @ExceptionHandler(UserNotExistException.class)
    public String handleException(Exception e, HttpServletRequest request){
        Map<String,Object> map = new HashMap<>();
        //传入我们自己的错误状态码  4xx 5xx，否则就不会进入定制错误页面的解析流程
        /**
         * Integer statusCode = (Integer) request
         .getAttribute("javax.servlet.error.status_code");
         */
        request.setAttribute("javax.servlet.error.status_code",500);
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        //转发到/error
        return "forward:/error";
    }
```

3. 将我们的定制数据携带出去

> 响应出去可以获取的数据是由getErrorAttributes得到的（是`AbstractErrorController`（`ErrorController`）规定的方法）

​	1、完全来编写一个ErrorController的实现类【或者是编写AbstractErrorController的子类】，放在容器中；

​	2、页面上能用的数据，或者是json返回能用的数据都是通过errorAttributes.getErrorAttributes得到；

​			容器中DefaultErrorAttributes.getErrorAttributes()；默认进行数据处理的；

```java
//给容器中加入我们自己定义的ErrorAttributes
@Component
public class MyErrorAttributes extends DefaultErrorAttributes {
    @Override
    public Map<String, Object> getErrorAttributes(RequestAttributes requestAttributes, boolean includeStackTrace) {
        Map<String, Object> map = super.getErrorAttributes(requestAttributes, includeStackTrace);
        map.put("company","atguigu");
        return map;
    }
}
```

## 13. spring boot数据访问

### spring boot默认配置

1. 数据库驱动和JDBC依赖

```xml
 		<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
 		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jdbc</artifactId>
        </dependency>
```

2. 配置数据源：数据源的相关配置都在`DataSourceProperties`里面

```yml
# 配置数据源
spring:
  datasource:
    username: root
    password: xiang123456
    url: jdbc:mysql://39.99.170.35:3306/jdbc
    driver-class-name: com.mysql.jdbc.Driver
```

3. 查看使用的默认数据源

```java
@SpringBootTest
class SpringbootMybatisApplicationTests {
    @Autowired
    DataSource dataSource;
    @Test
    void contextLoads() throws SQLException {
        System.out.println(dataSource.getClass()); 
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        connection.close();
    }

}
```

> 默认使用class com.zaxxer.hikari.HikariDataSource数据源

### spring boot默认配置原理

参考`org.springframework.boot.autoconfigure.jdbc`

1. DataSourceConfiguration

```java
SpringBoot默认可以支持以下数据源：
org.apache.tomcat.jdbc.pool.DataSource、HikariDataSource、BasicDataSource、dbcp2

自定义数据源类型:    
/**
 * Generic DataSource configuration.
 */
@ConditionalOnMissingBean(DataSource.class)
@ConditionalOnProperty(name = "spring.datasource.type")
static class Generic {

   @Bean
   public DataSource dataSource(DataSourceProperties properties) {
       //使用DataSourceBuilder创建数据源，利用反射创建响应type的数据源，并且绑定相关属性
      return properties.initializeDataSourceBuilder().build();
   }

}
```

2. **DataSourceInitializer：ApplicationListener**

   作用：

   ​		1）、runSchemaScripts()：运行建表语句；

   ​		2）、runDataScripts()：运行插入数据的sql语句；

   默认只需要将文件命名为：

   ```yml
   schema-*.sql、data-*.sql
   默认规则：schema.sql，schema-all.sql；
   可以使用   
   	schema:
         - classpath:department.sql
         指定位置
   ```

   > 应用启动时执行相应的sql建表语句

### 整合Druid数据源

```java
// 导入druid数据源
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druid(){
       return  new DruidDataSource();
    }

    //配置Druid的监控
    //1、配置一个管理后台的Servlet
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String,String> initParams = new HashMap<>();

        initParams.put("loginUsername","admin");
        initParams.put("loginPassword","123456");
        initParams.put("allow","");//默认就是允许所有访问
        initParams.put("deny","192.168.15.21");

        bean.setInitParameters(initParams);
        return bean;
    }


    //2、配置一个web监控的filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());

        Map<String,String> initParams = new HashMap<>();
        initParams.put("exclusions","*.js,*.css,/druid/*");

        bean.setInitParameters(initParams);

        bean.setUrlPatterns(Arrays.asList("/*"));

        return  bean;
    }
}
```

### 整合mybatis

```xml
		<dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.1</version>
        </dependency>
```

步骤：

1. 配置数据源相关属性
2. 配置数据库
3. 创建javaBean

#### 注解版

- mapper

```java
@Mapper
public interface DepartmentMapper {

    @Select("select * from department where id=#{id}")
    public Department getDeptById(Integer id);

    @Options(useGeneratedKeys = true,keyProperty = "id")//设置主键、自增
    @Insert("insert into department(departmentName) values (#{departmentName})")
    public int addDept(Department department);

    @Delete("delete from department where id=#{id}")
    public int deleteDept (Department department);

    @Update("update department set department_name = #{departmentName} where id = #{id}")
    public int updateDept(Department department);
}
```

- 业务层-控制层

```java
@Controller
public class DepartmentController {

    @Autowired
    DepartmentMapper departmentMapper;

    @GetMapping("/dept/{id}")
    @ResponseBody
    public Department get(@PathVariable("id") Integer id){
        Department department = departmentMapper.getDeptById(id);
        return department;
    }
    
    @GetMapping("/dept")
    @ResponseBody
    public Department add(Department department){
        departmentMapper.addDept(department);
        return department;
    }
}
```

- 自定义MyBatis的配置规则：给容器中添加一个`ConfigurationCustomizer`

```java
@org.springframework.context.annotation.Configuration
public class MybatisConfig {
    @Bean
    public ConfigurationCustomizer configurationCustomizer() {
        return new ConfigurationCustomizer() {
            @Override
            public void customize(Configuration configuration) {
                // 开启驼峰命名法（nick_name====nickName）
                configuration.setMapUnderscoreToCamelCase(true);
            }
        };
    }
}
```

#### 配置文件版

- 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--开启驼峰命名法-->
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
</configuration>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.arnold.mapper.EmployeeMapper">

    <select id="getEmpById" resultType="cn.arnold.pojo.Employee">
        SELECT * FROM employee WHERE id=#{id}
    </select>

    <insert id="addEmp">
        INSERT INTO employee(lastName,email,gender,d_id) VALUES (#{lastName},#{email},#{gender},#{dId})
    </insert>
</mapper>
```

- application配置文件中配置配置文件的路径

```yml
# 配置mybatis
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml   #指定全局配置文件的位置
  mapper-locations: classpath:mybatis/mapper/*.xml        #指定sql映射文件的位置
```

- 业务层-控制层

```java
@Controller
public class EmployeeController {
    
    @Autowired
    EmployeeMapper employeeMapper;

    @GetMapping("/emp/{id}")
    @ResponseBody
    public Employee getEmpById(@PathVariable("id") Integer id){
        Employee employee = employeeMapper.getEmpById(id);
        return employee;

    }

    @GetMapping("/emp")
    @ResponseBody
    public Employee add(Employee employee){
        employeeMapper.addEmp(employee);
        return employee;
    }
}
```

[更多参考](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)