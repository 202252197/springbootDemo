# SpringBoot

## 为什么要使用SpringBoot

传统项目:整合SSH或者SSM,需要大量的配置,好需要考虑jar包 冲突问题，整合太繁琐。

打包方式:打包成一个war放入tomcatwebapps目录下进行执行

SpringBoot是一个快速开发框架,能够帮助我们快速整合第三方框架,内置嵌入Http服务器(Tomcat,Jetty)服务器,默认嵌入Tomcat服务器,完全采用注解化,简化xml配置,最终以Java应用程序进行执行。

SpringBoot项目中没有web.xml

底层原理(使用Maven依赖关系Maven继承)

spring3.x以后采用注解方式启动SpringMVC,内嵌入Http服务器java创建Tomcat

## SpringBoot与SpringCloud关系

SpringBoot其实是一个快速开发框架,能够帮助我们快速整合(第三方常用框架）,完全采用注解简化，简化xml配置,最终以java应用程序运行执行。

SpringCloud是一套目前完整微服务框架，自从SpringCloud有了以后,Dubbo开始更新。

SpringCloud功能非常强大有注册中心,客户端调用工具,服务治理(负载均衡,断路器,分布式配置中心,网关,服务链路等。)

SpringBoot+SpringCloud是微服务开发

SpringBoot实现快速开发

SpringBoot Web组件默认集成SpringMVC，SpringCloud依赖于SpringBoot实现微服务,使用SpringMVC编写微服务接口。

## SpringBoot关系与SpringMVC关系

SpriingBoot Web组件集成了SpringMVC框架   

为什么SpringBoot启动SpringMVC的时候没有传统配置文件?

因为SpringMVC在3.x以后就支持了注解启动SpringMVC(也就是使用java代码启动SpringMVC)

## SpringBoot项目所需要导入的pom

pom导入

```
<!--整合第三方框架依赖信息（各种依赖信息）-->
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.4.RELEASE</version>
</parent>
<dependencies>
        <!--是SpringBoot整合SpringMVC web 实现原理,Maven依赖继承关系
            相当于把第三方常用maven依赖信息,在parent 项目中已经封装好了,使用springboot	提供依赖信息关联整合的jar包	
        -->
   		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<!--为什么不需要写版本号,因为parent里面已经封装好了版本号-->
		</dependency>
  </dependencies>
  <build>
  	<plugins>
  		<plugin>
  				<!--导入以maven运行springboot的插件-->
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
		<plugin>
				<!--导入springboot内置的tomcat插件-->
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.2</version>
		</plugin>
  	</plugins>
  </build>
	
```

## 其他注解解释

@RestController

```
@RestController注解 表示该类中的所有运行方法返回json格式 等同于@Controller+@ResponseBody
@RestController不是springboot带的,是spring4.0就有的注解
```

## SpringBoot启动和扫包注解

@EnableAutoConfiguration

```
@EnableAutoConfiguration注解 作用：扫包(扫包范围默认在当前类里面),在springboot启动类用
```

@ComponentScan

```
@ComponentScan("需要扫描的包名") 作用:扫包(指定需要扫描的包) 在springboot启动类用
扫描多个包使用   @ComponentScan(basePackages= {"","",""})
缺点：就是包过多的话写起来就会特别麻烦
```

@SpringBootApplication

```
@SpringBootApplication注解 作用:扫包(当前包+同级包)
@SpringBootApplication注解 等于@EnableAutoConfiguration+@ComponentScanto
```

## SpringBoot静态资源访问

```
1.静态资源:访问 js.css.图片
springboot 要求将静态资源存放在resource目录下面,并且文件夹名称必须符合如下规则
/static    /public   /resources  /META-INF/resources
举例：我们可以在src/main/resources/目录下创建static,在该位置放置一个图片文件,启动程序后,尝试访问http://localhost:8080/D.jpg。 如果显示图片,配置成功。不要加static这个目录,否则会找不到.
```

## SpringBoot整合Freemarker视图层

**默认配置**

```
<!--Freemarker配置-->
spring.freemarker.suffix=.ftl
spring.freemarker.templateEncoding=UTF-8
spring.freemarker.templateLoaderPath=classpath:/templates/
```

在src/main/resources/创建一个templates文件夹,后缀为*.ftl  

**1.导入依赖**

```
    <!-- 引入freeMarker的依赖包. -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>  
```

**2.后台代码**

```
@Controller
public class ceshi {
	@RequestMapping("/")
	public String ss() {
		return "lvshihao";
	}
}
```

## SpringBoot整合JSP视图层

注意:创建springboot整合jsp,一定要为war类型,否则会找不到页面。

**1.导入依赖**

```
	<!--导入jstl库-->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
	<!--整合api操作servlet所用到的jar包-->
		<dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
        </dependency>
	<!--springboot整合jsp所用到的jar包-->
        <dependency>
		    <groupId>org.apache.tomcat.embed</groupId>
		    <artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
```

**2.配置**

```
<!--springmvc的试图解析器-->
spring.view.prefix=/WEB-INF/jsp/  
spring.view.suffix=.jsp  
```

## Springboot自定义Banner

启动Spring Boot项目后会看到这样的图案： 

![1536667065619](图片储蓄\1536667065619.png)

这个图片其实是可以自定义的：

1.打开网站：<http://patorjk.com/software/taag/#p=display&h=3&v=3&f=4Max&t=itcast%20Spring%20Boot>

![1536667120637](图片储蓄\1536667120637.png)2.拷贝生成的字符到一个文本文件中，并且将该文件命名为banner.txt,

3.将banner.txt拷贝到项目的resources目录中：

![1536667183750](图片储蓄\1536667183750.png)

4.重新启动程序，查看效果：   

![1536667201758](图片储蓄\1536667201758.png)

5.**全局配置详情**

```
#配置banner的编码格式
spring.banner.charset=UTF-8 
#配置banner的路径地址
spring.banner.location=classpath:banner.txt 
#配置banner的图片路径地址
spring.banner.image.location=classpath:banner.gif 
#配置banner的图片宽度
spring.banner.image.width=100  
#配置banner的图片高度
spring.banner.image.height=100
#配置banner的图片margin距离
spring.banner.image.margin=20
```

6.附带图片

```
                _ooOoo_
                  o8888888o
                  88" . "88
                  (| -_- |)
                  O\  =  /O
               ____/`---'\____
             .'  \\|     |//  `.
            /  \\|||  :  |||//  \
           /  _||||| -:- |||||-  \
           |   | \\\  -  /// |   |
           | \_|  ''\---/''  |   |
           \  .-\__  `-`  ___/-. /
         ___`. .'  /--.--\  `. . __
      ."" '<  `.___\_<|>_/___.'  >'"".
     | | :  `- \`.;`\ _ /`;.`/ - ` : | |
     \  \ `-.   \_ __\ /__ _/   .-` /  /
======`-.____`-.___\_____/___.-`____.-'======
                   `=---='
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
         佛祖保佑       永无BUG

```

## SpringBoot整合全局捕获异常

全局捕获异常使用AOP技术  采用异常通知   以前有异常都是使用try-cath麻烦

全局捕获异常有两种案例

1.捕获异常返回 json格式的   @ResponseBody //返回json格式的

2.捕获异常返回页面    ModleAndView 返回页面

**注解解释**

@ControllerAdvice

```
代表全局捕获异常,也就是只要是basePackages扫描的包出了异常都会进入这个类里，扫描多个包使用@ControllerAdvice(basePackages={"","",""})   标注在类上的
```

@ExceptionHandler

```
代表异常的类型    如@ExceptionHandler(Exception.class)   标注在方法上的,就是出了这个异常就会走这个方法
```



```
@ControllerAdvice(basePackages="com.lvshihao") //代表全局捕获异常,也就是只要是basePackages扫描的包出了异常都会进入这个异常处理，扫描多个包使用@ControllerAdvice(basePackages={"","",""})
public class Error {
	@ExceptionHandler(Exception.class)
	@ResponseBody //返回json格式的
	public Map<String, Object> error(){
		Map<String,Object> map=new HashMap<String, Object>();
		map.put("lvshihao", "1");
		return map;
	}
}

```

## SpringBoot整合log4j记录日志

**1.导入依赖**

```
 <!-- springboot整合log4j所需要的依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j</artifactId>
        <version>1.3.8.RELEASE</version>
    </dependency>
```

**2.在src/main/resource新增log4j.properties配置文件:**

```
# LOG4J配置
log4j.rootCategory=INFO, stdout, file
 
# 控制台输出
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L - %m%n
 
# root日志输出到文件
log4j.appender.file=org.apache.log4j.DailyRollingFileAppender
log4j.appender.file.file=/data/logs/springboot-log4j-all.log
log4j.appender.file.DatePattern='.'yyyy-MM-dd
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L - %m%n
# 按不同package进行输出
# com.micai包下的日志配置
log4j.category.com.micai=DEBUG, didifile
# com.micai下的日志输出
log4j.appender.didifile=org.apache.log4j.DailyRollingFileAppender
log4j.appender.didifile.file=/data/logs/springboot-log4j-my.log
log4j.appender.didifile.DatePattern='.'yyyy-MM-dd
log4j.appender.didifile.layout=org.apache.log4j.PatternLayout
log4j.appender.didifile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L ---- %m%n
 
# ERROR级别输出到特定的日志文件中
log4j.logger.error=errorfile
# error日志输出
log4j.appender.errorfile=org.apache.log4j.DailyRollingFileAppender
log4j.appender.errorfile.file=/data/logs/springboot-log4j-error.log
log4j.appender.errorfile.DatePattern='.'yyyy-MM-dd
log4j.appender.errorfile.Threshold = ERROR
log4j.appender.errorfile.layout=org.apache.log4j.PatternLayout
log4j.appender.errorfile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L - %m%n
```

**3.后台代码**

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController{

    private static final Logger logger = LoggerFactory.getLogger(HelloController.class);

    @RequestMapping("hello")
    public String hello() throws JsonProcessingException {
        logger.info("hello world");
        return "hello world";
    }
}
```

## SpringBoot 使用AOP统一处理Web请求日志

**1.导入依赖**

```
<!--springboot整合aop所需要的jar包-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

**2.实现Web层的日志切面**

```
package com.aop;
import java.util.Arrays;
import javax.servlet.http.HttpServletRequest;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

@Aspect
@Component
public class WebLogAspect {

	private final static Logger logger=LoggerFactory.getLogger(WebLogAspect.class);
	//代表拦截的方法com.lvshihao.controller也就是拦截web或者controller层
    @Pointcut("execution(public * com.lvshihao..*.*(..))")
    public void webLog(){}
    
    //使用apo的前置通知拦截请求的参数信息
    @Before("webLog()")
    public void doBefore(JoinPoint joinPoint) throws Throwable {
        // 接收到请求，记录请求内容
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        //将这个拿到的这个ServletRequestAttributes转化成 HttpServletRequest
        HttpServletRequest request = attributes.getRequest();

        // 记录下请求内容
        //打印请求的路径
        logger.info("URL : " + request.getRequestURL().toString());
        //打印请求的方法
        logger.info("HTTP_METHOD : " + request.getMethod());
        //打印请求的IP地址
        logger.info("IP : " + request.getRemoteAddr());
        //打印请求的IP地址
        logger.info("CLASS_METHOD : " + joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName());
        logger.info("ARGS : " + Arrays.toString(joinPoint.getArgs()));

    }
    //使用aop返回通知
    @AfterReturning(returning = "ret", pointcut = "webLog()")
    public void doAfterReturning(Object ret) throws Throwable {
        // 处理完请求，返回内容
        logger.info("RESPONSE : " + ret);
    }

}
```

**3.访问映射方法就OK了在控制台你会看到以下信息**

```
2018-09-12 09:08:06.527  INFO 9168 --- [nio-8082-exec-1] com.aop.WebLogAspect                     : URL : http://localhost:8082/
2018-09-12 09:08:06.527  INFO 9168 --- [nio-8082-exec-1] com.aop.WebLogAspect                     : HTTP_METHOD : GET
2018-09-12 09:08:06.528  INFO 9168 --- [nio-8082-exec-1] com.aop.WebLogAspect                     : IP : 0:0:0:0:0:0:0:1
2018-09-12 09:08:06.536  INFO 9168 --- [nio-8082-exec-1] com.aop.WebLogAspect                     : CLASS_METHOD : com.lvshihao.Ceshi.result
2018-09-12 09:08:06.536  INFO 9168 --- [nio-8082-exec-1] com.aop.WebLogAspect                     : ARGS : []
2018-09-12 09:08:06.541  INFO 9168 --- [nio-8082-exec-1] com.aop.WebLogAspect                     : RESPONSE : lvshihao

```

## SpringBoot整合lombok

**1.安装lombok插件**

eclipse（myeclipse,sts一样） 安装 lombok

```
1.下载 lombok.jar，https://projectlombok.org/download.html)，将 lombok.jar 放在 eclipse 安装目录下，和 eclipse.ini 文件平级的。

2.运行 lombok.jar，在 lombok.jar 的目录下，运行：java -jar lombok.jar

3.运行后会弹框，直接点确定

4.点 specify location 按钮，选择 eclipse 的安装目录，选择到 eclipse 层即可，点击 install 即可

5.成功后会黑框框会多了很多log：

6.如果想看看是否真的安装成功，可以在 eclipse.ini 中看看，我的环境是多了一行(-javaagent:D:\Program Files\eclipse\lombok.jar)

7.重启 eclipse，再 clean project。
```

**2.导入依赖**

```
<!-- SpringBoot对lomboke插件做了支持所以不用写版本号，整合lombok所需要的jar包 -->
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
</dependency>
```

**lombok的@Getter@Setter注解的用法**

```
package com.pojo;

import lombok.Getter;
import lombok.Setter;
//@Data  这个注解代表=@Getter+@Setter所以有时直接可以使用@Data即可
@Getter
@Setter
public class User {
	private int id;
	private int age;
	private String name;
	//lombok底层使用了字节码技术ASM修改字节码文件,生成get和set方法
	//在类上加上@Getter@Setter注解的话会将所有变量都会生成get和set方法,也可以在属性上面加单独生成get和set方法
	
}

```

**lombok的@Slf4j日志注解的用法**

```
@Slf4j 	//日志注解等同于之前的private final static Logger logger=LoggerFactory.getLogger(WebLogAspect.class);
public class rizhi {
	public static void main(String[] args) {
		log.info("方法");
	}
}

```

**lombok其他注解**

```
@NonNull : 注解在参数上, 如果该类参数为 null , 就会报出异常,  throw new NullPointException(参数名)

@Cleanup : 注释在引用变量前, 自动回收资源 默认调用 close() 方法

@Getter/@Setter : 注解在类上, 为类提供读写属性

@ToString : 注解在类上, 为类提供 toString() 方法

@EqualsAndHashCode : 注解在类上, 为类提供 equals() 和 hashCode() 方法

@NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor : 注解在类上, 为类提供无参,有指定必须参数, 全参构造函数

@Data : 注解在类上, 为类提供读写属性, 此外还提供了 equals()、hashCode()、toString() 方法

@Value : 注解在属性上给属性注入值

@Builder : 注解在类上, 为类提供一个内部的 Builder

@Synchronized : 注解在方法上, 为方法提供同步锁

@Log4j : 注解在类上, 为类提供一个属性名为 log 的 log4j 的日志对象

@Slf4j : 注解在类上, 为类提供一个属性名为 log 的 log4j 的日志对象


```

## SpringBoot中的@Async异步执行方法 

**1.App启动类代码**

```
@SpringBootApplication //代表@EnableAutoConfiguration +@ComponentScan(扫描本包+子包)
@EnableAsync //使用异步方法必须要在启动类上加上这个注解否者异步无法使用,这就注解就是在启动的时候会扫描带有@Async注解的类或者方法
public class App {
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

**2.Controller类代码**

```
@Controller
@Slf4j
public class Ceshi {
	@Autowired
	Ht ht;
	@ResponseBody
	@RequestMapping("/")
	public String result() {
		//打印1
		log.info("1");
		
		//调用service类的aaa方法
		String aa=ht.aaa();
		
		//打印4
		log.info("4");
		return aa;
	}
}
```

**3.Service类代码**

```
@Service
@Slf4j
public class Ht {
	@Async //标注这个方法是异步,也就是另外开辟一个线程执行
	public String aaa() {
		//打印2
		log.info("2");
		try {
		//休眠5秒
			Thread.sleep(5000);
		} catch (Exception e) {
			// TODO: handle exception
		}
		//打印3
		log.info("3");
		
		//返回结果ss
		return "ss";
	}
}

```

## SpringBoot中的@Value自定义参数

**1.在全局配置文件中定义数据**（application.properties）

```
lvshihao=吕世昊
```

**2.用@Value获取全局配置文件中的数据**

```
@Controller
public class Ceshi {
	@Value("${lvshihao}") //获取全局配置文件中的lvshihao这个属性值,复制给name(注意:这是全局配置文件)
	private String name;
	
	@ResponseBody
	@RequestMapping("/")
	public String resulet() {
		return name;
	}
}
```

## SpringBoot中的区分不同环境配置文件

**在全局配置文件中可以指定使用开发环境**

```
spring.profiles.active=pre
```

就相当于引用哪个文件就使用哪个文件的配置

**配置文件名规范**（可以自定义dev,test,prod不是规定的）

```
application-dev.properties:开发环境
application-test.properties:测试环境
application-prod.properties:生成环境
```

## SpringBoot整合Mybatis

**1.导入依赖**

```
		<!-- SpringBoot整合Mybatis所需要的依赖 -->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.0</version>
	     </dependency>
		<!-- 导入MySQL所需要的依赖 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
		<!-- 使用alibaba的druid数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.0</version>
        </dependency>	
        <!-- 使用Jdbc的数据库连接池 -->
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>


```

**2.全局配置文件配置**（application.yml）

```
server:
  port: 8080

spring:
	# 配置数据库连接池
    datasource:
        url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8
        username: root
        password: admin123
        # 使用druid数据源
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.jdbc.Driver
    # 配置SprigMvc的试图解析器
    mvc:
        view:
          prefix: /jsp/
          suffix: .jsp
        
# 该配置节点为独立的节点，有很多同学容易将这个配置放在spring的节点下，导致配置无法被识别
mybatis:
  mapper-locations: classpath:mapping/*.xml  #注意：一定要对应mapper映射xml文件的所在路径
  type-aliases-package: com.lvshihao.entity  # 注意：对应实体类的包,扫描之后只需要写实体类的类名字即可

```

**3.启动类的代码**

```
@SpringBootApplication //代表@EnableAutoConfiguration +@ComponentScan(扫描本包+子包)
@MapperScan(basePackages="com.lvshihao.dao")//这个注解代表扫描mapper接口的（一定要对应mapper接口文件的所在路径）
public class App {
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

## SpringBoot整合@Transactional注解

**Spring的事务分类**

```
1.声明事务
2.编程事务
```

**Spring事务原理**

```
AOP技术使用环绕通知进行拦截    
```

**使用Spring事务注意事项**

```
不能try,如果try的话就不会出现异常,也就是说就不是执行事务了
```

**@Transactional**

```
这个注解代表开启事务管理器,在service操作增删改的时候在方法上添加此注解,如果出现异常则会回滚事务。
也可以添加到类上。(注意如果有查的方法的话就建议不要加在类上，应为查不会影响数据库)
```

**Service后台代码**

```
	@Transactional
	@Override
	public void add(User user) {
		// TODO Auto-generated method stub
		userMapper.add(user);
		System.out.println(1/0);
	}
```

## SpringBoot使用分包方式配置多数据源

**全局配置文件配置（application.properties）**

```
#数据库test1
spring.datasource.test1.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.test1.jdbc-url=jdbc:mysql://localhost:3306/test1?useUnicode=true&characterEncoding=utf-8
spring.datasource.test1.username=root
spring.datasource.test1.password=admin123
spring.datasource.test1.type=com.alibaba.druid.pool.DruidDataSource
#数据库test2
spring.datasource.test2.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.test2.jdbc-url=jdbc:mysql://localhost:3306/test2?useUnicode=true&characterEncoding=utf-8
spring.datasource.test2.username=root
spring.datasource.test2.password=admin123
spring.datasource.test2.type=com.alibaba.druid.pool.DruidDataSource
#配置视图解析器
spring.mvc.view.prefix=/jsp/
spring.mvc.view.suffix=.jsp

```

注意：其中test1和test2是自定义的

**新建一个com.datasource包**

**在里面再新建一个DataSource1Config.java,代码如下**

```
package com.datasource;
import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.SqlSessionTemplate;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;

@Configuration // 注入到spring容器中..
@MapperScan(basePackages = "com.lvshihao.test1.mapper", sqlSessionFactoryRef ="test1SqlSessionFactory")//扫描test1对应的mapper接口
public class DataSource1Config {

    /**
     * @methodDesc: 功能描述:(配置test1数据库)
     */
    @Bean(name = "test1DataSource")
    @ConfigurationProperties(prefix = "spring.datasource.test1")//读取全局配置文件中的前缀为spring.datasource.test1配置
    public DataSource testDataSource() {
        return DataSourceBuilder.create().build();
    }

    /**
     * @methodDesc: 功能描述:(test1 sql会话工厂)
     */
    @Bean(name = "test1SqlSessionFactory")
    public SqlSessionFactory testSqlSessionFactory(@Qualifier("test1DataSource")DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
        //扫描mapper.xml所在的目录，有mapper.xml就扫描,用注解的时候可以将这个注释掉
//        bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper1/*.xml"));
        return bean.getObject();
    }
/**
 * 当存在多于1个数据源的时候，必须选择一个作为主数据源（Primary DataSource），
 * 即如果数据库操作没有指明使用哪个数据源的时候，默认使用主数据源。
 * 同时，把数据源绑定到不同的JdbcTemplate上。
 * 用@Primary把其中某一个Bean标识为“主要的”，使用@Autowired注入时会首先使用被标记为@Primary的Bean。
 */
    /**
     * @methodDesc: 功能描述:(test1 事物管理)
     */
    @Bean(name = "test1TransactionManager")
    public DataSourceTransactionManager test1TransactionManager(@Qualifier("test1DataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    /**
     * 使用@Autowired注释进行byType注入，如果需要byName（byName就是通过id去标识）注入，
     * 增加@Qualifier注释。一般在候选Bean数目不为1时应该加@Qualifier注释。
     * 在默认情况下使用 @Autowired 注释进行自动注入时，Spring 容器中匹配的候选 Bean 数目必须有且仅有一个。
     * 当找不到一个匹配的 Bean 时，Spring 容器将抛出
     * BeanCreationException 异常，并指出必须至少拥有一个匹配的 Bean。
     * 和找不到一个
     * 配 Bean 相反的一个错误是：如果 Spring 容器中拥有多个候选 Bean，
     * Spring 容器在启动时也会抛出 BeanCreationException 异常。
     * Spring 允许我们通过 @Qualifier 注释指定注入 Bean 的名称，这样歧义就消除了，可以通过下面的方法解决异常：
     *
     * @param sqlSessionFactory
     * @return
     * @throws Exception
     */
    @Bean(name = "test1SqlSessionTemplate")
    public SqlSessionTemplate testSqlSessionTemplate(@Qualifier("test1SqlSessionFactory") SqlSessionFactory sqlSessionFactory)throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }

}
```

**在里面再新建一个DataSource2Config.java,代码如下**

```
package com.datasource;
import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.SqlSessionTemplate;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;

@Configuration // 注入到spring容器中..
@MapperScan(basePackages = "com.lvshihao.test2.mapper", sqlSessionFactoryRef ="test2SqlSessionFactory")//扫描test1对应的mapper接口
public class DataSource2Config {

    /**
     * @methodDesc: 功能描述:(配置test2数据库)
     */
    @Bean(name = "test2DataSource")
    @ConfigurationProperties(prefix = "spring.datasource.test2")//读取全局配置文件中的前缀为spring.datasource.test2配置
    public DataSource testDataSource() {
        return DataSourceBuilder.create().build();
    }

    /**
     * @methodDesc: 功能描述:(test2 sql会话工厂)
     */
    @Bean(name = "test2SqlSessionFactory")
    public SqlSessionFactory testSqlSessionFactory(@Qualifier("test2DataSource")DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
        //扫描mapper.xml所在的目录，有mapper.xml就扫描,用注解的时候可以将这个注释掉
//        bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper2/*.xml"));
        return bean.getObject();
    }
/**
 * 当存在多于1个数据源的时候，必须选择一个作为主数据源（Primary DataSource），
 * 即如果数据库操作没有指明使用哪个数据源的时候，默认使用主数据源。
 * 同时，把数据源绑定到不同的JdbcTemplate上。
 * 用@Primary把其中某一个Bean标识为“主要的”，使用@Autowired注入时会首先使用被标记为@Primary的Bean。
 */
    /**
     * @methodDesc: 功能描述:(test2 事物管理)
     */
    @Bean(name = "test2TransactionManager")
    public DataSourceTransactionManager test2TransactionManager(@Qualifier("test2DataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    /**
     * 使用@Autowired注释进行byType注入，如果需要byName（byName就是通过id去标识）注入，
     * 增加@Qualifier注释。一般在候选Bean数目不为1时应该加@Qualifier注释。
     * 在默认情况下使用 @Autowired 注释进行自动注入时，Spring 容器中匹配的候选 Bean 数目必须有且仅有一个。
     * 当找不到一个匹配的 Bean 时，Spring 容器将抛出
     * BeanCreationException 异常，并指出必须至少拥有一个匹配的 Bean。
     * 和找不到一个
     * 配 Bean 相反的一个错误是：如果 Spring 容器中拥有多个候选 Bean，
     * Spring 容器在启动时也会抛出 BeanCreationException 异常。
     * Spring 允许我们通过 @Qualifier 注释指定注入 Bean 的名称，这样歧义就消除了，可以通过下面的方法解决异常：
     *
     * @param sqlSessionFactory
     * @return
     * @throws Exception
     */
    @Bean(name = "test2SqlSessionTemplate")
    public SqlSessionTemplate testSqlSessionTemplate(@Qualifier("test2SqlSessionFactory") SqlSessionFactory sqlSessionFactory)throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }

}
```

**再分别创建两个包**

第一个是com.lvshihao.test1.mapper 也就是上面文件test1所扫描的数据库对应的mapper

第二个是com.lvshihao.test2.mapper 也就是上面文件test2所扫描的数据库对应的mapper

**导入的依赖有**

```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- SpringBoot对lomboke插件做了支持所以不用写版本号，整合lombok所需要的jar包 -->
		<dependency>
		  <groupId>org.projectlombok</groupId>
		  <artifactId>lombok</artifactId>
		</dependency>
		<!-- SpringBoot整合Mybatis所需要的依赖 -->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.0</version>
	     </dependency>
		<!-- 导入MySQL所需要的依赖 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
		<!-- alibaba的druid数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.0</version>
        </dependency>
		<!--导入jstl库-->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		<!--整合api操作servlet所用到的jar包-->
		<dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
        </dependency>
		<!--springboot整合jsp所用到的jar包-->
        <dependency>
		    <groupId>org.apache.tomcat.embed</groupId>
		    <artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
		<!--导入springboot的热部署jar包 -->
		<dependency>
	       <groupId>org.springframework.boot</groupId>
	       <artifactId>spring-boot-devtools</artifactId>
	       <optional>true</optional>
	       <scope>true</scope>
	   </dependency>
```

**然后正常的启动类,正常的逻辑代码**

## SpringBoot多数据源事务管理机制

只需要根据上面的代码在Service层的方法上加上@Transactional(transactionManager="test1TransactionManager")

其中的**test1TransactionManager**，就是在配置**DataSource1Config.java**的时候，将事务管理器的bean名字叫做**test1TransactionManager**

**比如：**

数据库test1的就是@Transactional(transactionManager="test1TransactionManager")

数据库test2的就是@Transactional(transactionManager="test2TransactionManager")

**Service代码如下：**

```
@Service
public class Test1Service {
	
	@Autowired
	Test1Mapper test1Mapper;
	#引入某一个事务管理器
	@Transactional(transactionManager="test1TransactionManager")
	public void add(User user) {
		test1Mapper.add(user);
		System.out.println(1/0);
	}
}
```

## SpringBoot整合PageHelper(分页插件)

**1.导入依赖**

```
<!-- 导入SpringBoot整合mybatis的分页插件 -->
<dependency>
       <groupId>com.github.pagehelper</groupId>
       <artifactId>pagehelper-spring-boot-starter</artifactId>
       <version>1.2.3</version>
</dependency>
```

**2.全局配置文件配置(application.yml)**

```
server:
  port: 8080

spring:
    datasource:
        url: jdbc:mysql://localhost:3306/test
        username: root
        password: admin123
        # 使用druid数据源
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.jdbc.Driver
    mvc:
        view:
          prefix: /jsp/
          suffix: .jsp
        
## 该配置节点为独立的节点，有很多同学容易将这个配置放在spring的节点下，导致配置无法被识别
mybatis:
  mapper-locations: classpath:mapping/*.xml  #注意：一定要对应mapper映射xml文件的所在路径
  type-aliases-package: com.lvshihao.entity  # 注意：对应实体类的路径
  
  #配置分页插件pagehelper
pagehelper:
    helperDialect: mysql
    reasonable: true
    supportMethodsArguments: true
    params: count=countSql


```

**3.Service层代码**

```
@Service
public class UserService implements UserApi{
	@Autowired
	UserMapper userMapper;
	
	@Override
	public PageInfo<User> selectlimt(int pageNum,int pageSize) {//pageNum代表当前页数,pageSize代表要显示的条数
		//帮我们生成分页语句,底层帮我们修改sql语句
		PageHelper.startPage(pageNum, pageSize);
		ArrayList<User> arr=userMapper.select();
		//返回给客户端展示的数据
		PageInfo<User> pageInfo=new PageInfo<>(arr);
		return pageInfo;
	}

}
```

## SpringBoot集成热部署Devtools

**1.导入依赖**

```
         <!--导入springboot的热部署jar包 -->
		<dependency>
	       <groupId>org.springframework.boot</groupId>
	       <artifactId>spring-boot-devtools</artifactId>
	       <optional>true</optional>
	       <scope>true</scope>
	   </dependency>
```

完成，建议不要在原本的方法上修改测试,新建一个方法测试，应为在原本方法上修改他是不会重启的。

## SpringBoot性能优化之扫包优化

**扫包优化代表的是启动的时候优化**

@SpringBootApplication 

缺点：会将不需要扫描的包也扫描了，这样启动会比较慢

优点：不需要一个一个指定扫描包

@EnableAutoConfiguration+@ComponentScan 

缺点：需要一个一个指定扫描包

优点：启动会比较快,节省了扫描不需要的包的时间

## SpringBoot将Servlet容器变成Undertow

**Undertow是Jboss旗下**  

Tomcat运行吞吐量是5000，Undertow运行吞吐量是8000

SpringBoot默认容器是Tomcat，以后微服务主流服务器就是Undertow

**1.排除Tomcat依赖**

```
<dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
       <!-- 从依赖信息里移除 Tomcat配置 -->
       <exclusions> 
            <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
       </exclusions> 
    </dependency>
```

**2.添加依赖**

```
 <!-- 添加 SpringBoot整合Undertow依赖 -->
    <dependency> 
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-undertow</artifactId>
    </dependency> 
```

## SpringBoot打包方式（jar+war）

**1.手动打包**（jar+war通用）

--用cmd进入项目的根目录,输入 mvn package 即可打包 

--使用 java - jar  包名

**2.通过Maven打jar包**（jar+war通用）

输入Maven命令： `mvn install`

**3.IDEA打包方式**（jar+war通用）

IDEA中右击选择Run as -> maven install

### **打完包运行jar包遇到的坑**

**1.Spring Boot:jar中没有主清单属性问题**

在这里有一个问题就是主清单属性是什么?以SpringBoot为例，jar包中包含了三个文件夹：BOOT-INF，META-INF，org，可以把jar包解压到文件夹下查看，其中META-INF文件夹下有一个MANIFEST.MF文件，该文件指明了程序的入口以及版本信息等内容，如下 

```
Manifest-Version: 1.0
Implementation-Title: spring-xxx-xxx
Implementation-Version: 0.0.1-SNAPSHOT
Archiver-Version: Plexus Archiver
Built-By: XXXX
Implementation-Vendor-Id: com.huyikang.practice
Spring-Boot-Version: 1.5.9.RELEASE
Implementation-Vendor: Pivotal Software, Inc.
Main-Class: org.springframework.boot.loader.JarLauncher
Start-Class: com.huyikang.practice.eureka.Application
Spring-Boot-Classes: BOOT-INF/classes/
Spring-Boot-Lib: BOOT-INF/lib/
Created-By: Apache Maven 3.5.2
Build-Jdk: 1.8.0_151
Implementation-URL: http://maven.apache.org
```

**Main-Class**代表了Spring Boot中启动jar包的程序

**Start-Class**属性就代表了Spring Boot程序的入口类，这个类中应该有一个main方法

**Spring-Boot-Classes**代表了类的路径，所有编译后的class文件，以及配置文件，都存储在该路径下

**Spring-Boot-Lib**表示依赖的jar包存储的位置

这些值都是SpringBoot打包插件会默认生成的，如果没有这些属性，SpringBoot程序自然不能运行，就会报错：jar中没有主清单属性，也就是说没有按照SpringBoot的要求，生成这些必须的属性。

**解决办法**：在pom中添加一个SpringBoot的构建的插件，然后重新运行 mvn install即可。

```
			<plugin>
				<!-- SpringBoot的构建的插件 打包时需要用到-->
		  		<groupId>org.springframework.boot</groupId>
		 		<artifactId>spring-boot-maven-plugin</artifactId>
		 		<configuration>
		 			<!-- 指定java -jar 主函数执行入口-->
		 			<mainClass>com.App</mainClass>
		 		</configuration>
		 		<executions>
		 			<execution>
		 				<goals>
		 					<goal>repackage</goal>
		 				</goals>
		 			</execution>
		 		</executions>
  			</plugin>
```

在运行**mvn install**的时候，自动生成这些主清单属性，运行**java -jar xxx.jar**时会根据主清单属性找到启动类，从而启动程序。 

**2.缺少maven-surefire-plugin插件**

Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2.21.0

**解决方案**在pom中添加一个maven-surefire-plugin的插件，然后重新运行 mvn install即可。

```
<plugin>  
		        <groupId>org.apache.maven.plugins</groupId>  
		        <artifactId>maven-surefire-plugin</artifactId>  
		        <version>2.4.2</version>  
		        <configuration>  
		        	<skipTests>true</skipTests>  
      			</configuration>  
</plugin>  
```

## 使用外部Tomcat运行SpringBoot项目

**1.打包之后包的类型必须是war**

**2.打包完成之后放到Tomcat目录中的webapps目录下**

**3.启动Tomcat即可**

### 坑能遇到的坑

因为SpringBoot2.0.4内置的Tomcat版本是8.5，所以打包之后的war包必须放在Tomcat8.5,或者更高版本.否者会报错。

## SpringBoot监控中心概述

**1.什么是SpringBoot监控中心**

```
针对微服务器监控,服务器内存变化（堆内存,线程,日志管理等），检测服务配置连接地址是否可用(模拟访问,懒加载),统计现在有多少个bean(是Spring容器中bean),统计SpringMVC@RequestMapping(统计http接口)。
Actuator 监控应用(没有界面,返回json格式)。
AdminUi  底层使用Actuator监控应用,实现可视化界面。
应用场景：适用于生成环境
默认情况下,监控中心提供三个接口权限.
在SpringBoot2.0之后 监控中心   接口地址发生变化
2.0之前访问是  http://localhost:8080/health
2.0之后访问是  http://localhost:8080/actuator/health
```

**2.为什么要用SpringBoot监控中心**

```
Actuator是Spring Boot的一个附加功能,可帮助你在应用程序生产环境时和管理应用程序。可以使用Http的各种请求来监管,审计,收集应用的运行情况,特别对于微服务管理十分有意义,缺点：没有可视化界面。
```

## SpringBoot监控中心之搭建Actuator监控中心

**1.导入依赖**

```
	<!-- 引入SpringBoot整合actuator监控中心的依赖 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
```

**2.全局配置文件配置(application.yml)**

```
###通过下面的配置启用所有的监控端点,默认情况下,这些端点是有禁用的,"*"号代表监控所有的接口,actuator默认会开启3个监控接口的权限
management:
    endpoints:
     web:
        exposure:
          include: "*"
```

可以进行测试，不加此配置的时候 运行启动类查看加载的日志,加上此配置再次查看加载的日志。

**不加配置的时候运行信息**

```
Mapped "{[/actuator/health],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}" 
Mapped "{[/actuator/info],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}" 
Mapped "{[/actuator],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"
```

**加配置的时候运行信息**

```
  Mapped "{[/actuator/auditevents],methods=[GET],produces=[application/vnd.spring- boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/beans],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/health],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/conditions],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/configprops],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/env],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/env/{toMatch}],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/info],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/loggers],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/loggers/{name}],methods=[POST],consumes=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/loggers/{name}],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/heapdump],methods=[GET],produces=[application/octet-stream]}"  
  Mapped "{[/actuator/threaddump],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/metrics],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/metrics/{requiredMetricName}],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/scheduledtasks],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/httptrace],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator/mappings],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}"  
  Mapped "{[/actuator],methods=[GET],produces=[application/vnd.spring-boot.actuator.v2+json || application/json]}" 
```

**3.Actuator访问路径解释**

![](图片储蓄\sss.png)

http://localhost:8080/actuator/health代表帮你查看配置文件是否正确

```
如Mysql的账号密码是否正确如果配置正确会显示up，错误则显示down。等还可以redis,orcal数据库
```

http://localhost:8080/actuator/info代表配置文件新增，配置例子如下

```
info:
     name: lvshihao
     age: 18
```

配置完成会访问此页面会以json的形式返回到页面上。

## AdminUI平台原理和AdminUIServer端搭建

AdminUi底层使用了Actuator监控应用,实现可视化界面。

Server端就相当于你想部署界面的项目,而Client端启动的时候会注册到Server端。

**原理:**AdminUi原理

将所有服务的监控中心管理存放在adminUi上

**1.导入依赖**

```
<!--  SpringBoot Actuator对外暴露应用的监控信息,Jolokia提供使用HTTP接口获取JSON格式的数据-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
		    <groupId>com.googlecode.json-simple</groupId>
		    <artifactId>json-simple</artifactId>
		</dependency>
		<dependency>
		    <groupId>org.jolokia</groupId>
		    <artifactId>jolokia-core</artifactId>
		</dependency>
		<!-- 依赖adminUi的服务端-->
		<dependency>
		    <groupId>de.codecentric</groupId>
		    <artifactId>spring-boot-admin-starter-server</artifactId>
		    <version>2.0.2</version>
		</dependency>
		<!--  引入了webflux就不需要再引入web了应为他底层依赖的有web-->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-webflux</artifactId>
		</dependency>
```

**2.全局配置文件配置(applicatio.yml)**

```
spring:
    application:
      name: spring-boot-admin-server 
server:
    port: 8080
```

**3.启动类代码**

```
@SpringBootApplication //代表@EnableAutoConfiguration +@ComponentScan(扫描本包+子包)
@EnableAdminServer //代表开启AdminUi监控
@MapperScan(basePackages="com.lvshihao.dao")
public class App {
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}

}
```

**4.Maven依赖需要注意的问题**

```
如果你的maven依赖的jar包出现失败,然后pom文件头别协议报错,说明Maven中央仓库已经不支持下载这个jar包，可以进入http://mvnrepository.com/根据jar包的名字查询此jar包,就说明Maven已经不支持下载此版本的jar包了。
```

## SpringBoot-AdminUIClient使用

Client端就相当于你想监控的项目,而下面的全局配置就是将这个Client端注册到Server上

**1.导入依赖**

```
<!--  SpringBoot Actuator对外暴露应用的监控信息,Jolokia提供使用HTTP接口获取JSON格式的数据-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
		    <groupId>com.googlecode.json-simple</groupId>
		    <artifactId>json-simple</artifactId>
		</dependency>
		<dependency>
		    <groupId>org.jolokia</groupId>
		    <artifactId>jolokia-core</artifactId>
		</dependency>
		<!-- 依赖adminUi的客户端-->
		<dependency>
		    <groupId>de.codecentric</groupId>
		    <artifactId>spring-boot-admin-starter-client</artifactId>
		    <version>2.0.2</version>
		</dependency>
		<!--  引入了webflux就不需要再引入web了应为他底层依赖的有web-->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-webflux</artifactId>
		</dependency>

```

**2.全局配置文件配置(application.yml)**

```
spring:  ##配置admin ui client端 注册到admin ui server平台  url代表的是注册的server地址的链接
    boot:
      admin:
        client:
          url: http://localhost:8081
###通过下面的配置启用所有的监控端点,默认情况下,这些端点是有禁用的,"*"号代表监控所有的接口,actuator默认会开启3个监控接口的权限
management:
    endpoints:
     web:
        exposure:
          include: "*"
```

**3.启动类代码**

```
@SpringBootApplication //代表@EnableAutoConfiguration +@ComponentScan(扫描本包+子包)
@MapperScan(basePackages="com.lvshihao.dao")
public class App {
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}

}
```

## SpringBoot整合Redis

**1.导入依赖**

```
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
		    <groupId>com.alibaba</groupId>
		    <artifactId>fastjson</artifactId>
		    <version>1.2.47</version>
</dependency>
```

**2.全局配置文件配置(application.yml)**

```
spring:
    # 配置SprigMvc的试图解析器
    mvc:
        view:
          prefix: /jsp/
          suffix: .jsp
    redis:
        host: 127.0.0.1
        port: 6379
        database: 1
        timeout: 5000
```

**3.Service业务逻辑代码**

```
package com.lvshihao.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.ValueOperations;
import org.springframework.stereotype.Service;
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.TypeReference;
import com.lvshihao.api.UserApi;
import com.lvshihao.dao.UserMapper;
import com.lvshihao.entity.User;
import java.util.ArrayList;
import javax.annotation.Resource;


@Service
public class UserService implements UserApi{

    @Autowired
    StringRedisTemplate stringRedisTemplate;


    @Resource(name = "stringRedisTemplate")
    ValueOperations<String, String> valOpsStr;

    @Autowired
    RedisTemplate<Object, Object> redisTemplate;

    @Resource(name = "redisTemplate")
    ValueOperations<Object, Object> valOpsObj;

    @Autowired
    UserMapper userMapper;
	@Override
	public void insetallAll() {
		 ArrayList<User> arrayList=userMapper.selectAll();
		 String jsonuser=JSON.toJSONString(arrayList);
		 redisTemplate.opsForValue().set("userjson", jsonuser);
	}
	@Override
	public ArrayList<User> selectAll() {
		String redisjson=(String) redisTemplate.opsForValue().get("userjson");
		//将json字符串转换成ArrayList
		ArrayList<User> mapList = JSON.parseObject(redisjson, new TypeReference<ArrayList<User>>(){});
		return mapList;
	}

}
```

## SpringBoot整合Solr

**1.导入依赖**

```
    <properties>
        <spring.data.solr.version>2.1.1.RELEASE</spring.data.solr.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.data</groupId>
                <artifactId>spring-data-solr</artifactId>
                <version>${spring.data.solr.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>  
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-solr</artifactId>
        </dependency>

        <!-- 默认 starter 会加载 solrj 进来, 下面这个可不引-->
        <dependency>
            <groupId>org.apache.solr</groupId>
            <artifactId>solr-solrj</artifactId>
            <version>6.6.2</version>
        </dependency>
    </dependencies>
```

**2.全局配置文件配置(application.yml)**

```
spring:
    # 配置SprigMvc的试图解析器
    mvc:
        view:
          prefix: /
          suffix: .jsp
    # 配置solr
    data:
        solr:
          host: http://localhost:8080/solr/collection1
server:
    port: 8082
```

**3.Controller**

```
@Controller
public class Producer {
	@Autowired
	SolrClient solrClient;
	@RequestMapping("/solr")
	@ResponseBody
	public String select() throws SolrServerException, IOException {
		SolrDocument byId = solrClient.getById("1");
		String name = (String) byId.get("product_name");
		return name;
	}
}
```

## SpringBoot整合ActiveMQ

**1.导入依赖**

```
    <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-activemq</artifactId>
	</dependency>
	如果pool:enabled设置为true,相当于就是使用active的连接池,还需要导入一下依赖包,否者会自动配置失败
    <dependency>
        <groupId>org.apache.activemq</groupId>
        <artifactId>activemq-pool</artifactId>
    </dependency>
```

**2.Controller**

```
package com.lvshihao.controller;


import javax.jms.Destination;
import org.apache.activemq.command.ActiveMQQueue;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jms.annotation.JmsListener;
import org.springframework.jms.core.JmsMessagingTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class Controller {
	@Autowired
	private JmsMessagingTemplate jmsMessagingTemplate;
	//发送消息
	@RequestMapping("sendmessage")
	public void sendMessage() {
		Destination destination=new ActiveMQQueue("lvshihao");
		jmsMessagingTemplate.convertAndSend(destination, "lvshihaolove");
	}
	//消费消息
	@RequestMapping("receptionmessage")
	@JmsListener(destination="lvshihao")
	public void receptionmessage(String text) {
		System.out.println(text);
	}
}
```

**3.全局配置文件配置(application.yml)**

```
spring:
  activemq:
    broker-url: tcp://localhost:61616
    in-memory: true
    user: system
    password: manager
    pool:
       enabled: false
```

## SpringBoot整合Email

**1.导入依赖**

```
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
```

**2.全局配置文件配置(application.yml)**

```
spring:
  mail:
    host: smtp.qq.com #smtp.163.com
    username: 202252197@qq.com
    password: kucgiuasejvocaae  
    default-encoding: UTF-8
    properties:
      mail:
        smth:
          auth: true
          starttls:
            enable: true
            required: true
#若使用QQ邮箱发邮件,则需要修改spring.mail.host=smtp.qq.com,同时spring.mail.password改为QQ邮箱的授权码
#QQ邮箱->设置->账户->POP3/SMTP服务:开启服务后会获得QQ的授权码

#出现认证失败的解决方案:因为JDK1.8中jre\lib\security中两个jar包替换的缘故。
#将下载后的local_policy.jar和US_export_policy.jar替换到jdk1.8的jre\lib\security文件夹即可。
```

**3.简单发送邮件的Service业务逻辑代码**

```
package com.lvshihao.controller;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.stereotype.Service;
@Service
public class UserService{	
	@Autowired
	JavaMailSender javaMailSender;
	//获取发件人邮箱
	@Value("${spring.mail.username}")
	String i;
	public String getI() {
		return i;
	}
	public void setI(String i) {
		this.i = i;
	}
	
	//发生简单的邮件
	public void sendEmail(String receive, String title, String Content) {
		SimpleMailMessage simpleMailMessage=new SimpleMailMessage();
		//发件人邮箱
		simpleMailMessage.setFrom(i);
		//接收邮件人
		simpleMailMessage.setTo("202252197@qq.com"); //receive
		//发送邮件的标题
		simpleMailMessage.setSubject("你好"); //title
		//发送邮件的内容
		simpleMailMessage.setText("成功了"); //Content
		//发送
		javaMailSender.send(simpleMailMessage);
	}
}
```

**4.发送带附件的邮件Service业务逻辑代码**

```
package com.lvshihao.controller;

import java.io.File;

import javax.mail.internet.MimeMessage;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.FileSystemResource;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.stereotype.Service;
@Service
public class UserService implements UserApi{	
	@Autowired
	JavaMailSender javaMailSender;
	//获取发件人邮箱
	@Value("${spring.mail.username}")
	String i;
	public String getI() {
		return i;
	}
	public void setI(String i) {
		this.i = i;
	}
	
	//发送带附件的邮件
	@Override
	public void sendEmail(String receive, String titile, String Content,File file) {
		MimeMessage msg=javaMailSender.createMimeMessage();
		try {
			MimeMessageHelper messageHelper=new MimeMessageHelper(msg,true);
			//发件人邮箱
			messageHelper.setFrom(i);
			//接收邮件人
			messageHelper.setTo("202252197@qq.com");
			//发送邮件的标题
			messageHelper.setSubject("你好");
			//发送邮件的内容
			messageHelper.setText("成功了");
			//发送文件
			FileSystemResource fileSystemResource=new FileSystemResource(file);
			messageHelper.addAttachment("lucene.md", fileSystemResource);
		} catch (Exception e) {
			e.printStackTrace();
		}
		//发送
		javaMailSender.send(msg);
	}
}
```

```
其中传入的File文件可以这样写
File file=new File("src/main/resources/static/lucene.md");
```

**5.发送模板邮件的Service业务逻辑代码**

```
package com.lvshihao.controller;

import java.util.HashMap;
import java.util.Map;

import javax.mail.internet.MimeMessage;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.stereotype.Service;
import org.springframework.ui.freemarker.FreeMarkerTemplateUtils;
import org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer;

import freemarker.template.Template;
@Service
public class UserService implements UserApi{	
	@Autowired
	JavaMailSender javaMailSender;
	@Autowired
	FreeMarkerConfigurer freeMarkerConfigurer;
	//获取发件人邮箱
	@Value("${spring.mail.username}")
	String i;
	public String getI() {
		return i;
	}
	public void setI(String i) {
		this.i = i;
	}
	
	//发送模板邮件
	@Override
	public void sendEmail(String receive, String titile, String Content,String info) {
		MimeMessage msg=javaMailSender.createMimeMessage();
		try {
			MimeMessageHelper messageHelper=new MimeMessageHelper(msg,true);
			//发件人邮箱
			messageHelper.setFrom(i);
			//接收邮件人
			messageHelper.setTo("202252197@qq.com");
			//发送邮件的标题
			messageHelper.setSubject("你好");
			//封装模板使用的数据
			Map<String, Object> model=new HashMap<>();
			model.put("username", "lvshihao");
			//获取到模板
			Template template=freeMarkerConfigurer.getConfiguration().getTemplate(info);
			//将模板中的代码转换成字符串
			String processTemplateIntoString = FreeMarkerTemplateUtils.processTemplateIntoString(template, model);
			//发送邮件的内容
			messageHelper.setText(processTemplateIntoString, true);
		} catch (Exception e) {
			e.printStackTrace();
		}
		//发送
		javaMailSender.send(msg);
	}
}

```

```
1.其中String processTemplateIntoString = FreeMarkerTemplateUtils.processTemplateIntoString(template, model);中的template代表要传入的模板名称,如index.html，SpringBoot默认读取的是Template文件下的静态资源，
model代表的就是要传入的数据可以用${username}接收。
2.还需要导入Freemarker依赖。
```

## SpringBoot整合定时任务

**1.导入依赖**

```
	    <!-- 添加Scheduled依赖 -->
	    <dependency>
	        <groupId>org.springframework</groupId>
	        <artifactId>spring-context-support</artifactId>
	    </dependency>  
```

**2.App启动类代码**

```
@SpringBootApplication
//开启Scheduling支持
@EnableScheduling
public class App {
	public static void main(String[] args) throws Exception {
		SpringApplication.run(App.class, args);
	}
}
```

**3.Controller类代码**

```
	//代表2秒执行一次业务逻辑
	@Scheduled(fixedRate=2000)
	public void sendmail() {
		System.out.println("当前时间:"+System.currentTimeMillis());
	}
```

## SpringBoot整合HDFS

```
   // 构造一个配置参数对象，设置一个参数：我们要访问的hdfs的URI
    // 从而FileSystem.get()方法就知道应该是去构造一个访问hdfs文件系统的客户端，以及hdfs的访问地址
    // new Configuration();的时候，它就会去加载jar包中的hdfs-default.xml
    // 然后再加载classpath下的hdfs-site.xml
    Configuration conf = new Configuration();
    conf.set("fs.defaultFS", "hdfs://mini1:9000");
    /**
     * 参数优先级： 1、客户端代码中设置的值 2、classpath下的用户自定义配置文件 3、然后是服务器的默认配置
     */
    conf.set("dfs.replication", "3");
    // 获取一个hdfs的访问客户端，根据参数，这个实例应该是DistributedFileSystem的实例
    // fs = FileSystem.get(conf);

    // 如果这样去获取，那conf里面就可以不要配"fs.defaultFS"参数，而且，这个客户端的身份标识已经是hadoop用户
    fs = FileSystem.get(new URI("hdfs://mini1:9000"), conf, "root");
```

**1.导入依赖**

```
     <!-- 添加操作Hadoop的依赖 -->
     <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-common</artifactId>
            <version>2.8.5</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.8.5</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-hdfs</artifactId>
            <version>2.8.5</version>
        </dependency>
```

**2.查看目录信息，只显示文件**

```
	FileSystem fs = null;
	// 思考：为什么返回迭代器，而不是List之类的容器
    RemoteIterator<LocatedFileStatus> listFiles = fs.listFiles(new Path("/"), true);

    while (listFiles.hasNext()) {
        LocatedFileStatus fileStatus = listFiles.next();
        System.out.println(fileStatus.getPath().getName());
        System.out.println(fileStatus.getBlockSize());
        System.out.println(fileStatus.getPermission());
        System.out.println(fileStatus.getLen());
        BlockLocation[] blockLocations = fileStatus.getBlockLocations();
        for (BlockLocation bl : blockLocations) {
            System.out.println("block-length:" + bl.getLength() + "--" + "block-offset:" + bl.getOffset());
            String[] hosts = bl.getHosts();
            for (String host : hosts) {
                System.out.println(host);
            }
        }
        System.out.println("--------------为angelababy打印的分割线--------------");
    }
```

## SpringBoot整合Flume日志

**1.导入依赖**

```
        <dependency>
            <groupId>org.apache.flume.flume-ng-clients</groupId>
            <artifactId>flume-ng-log4jappender</artifactId>
            <version>1.6.0</version>
        </dependency>
```

**2.log4j.properties配置**

```
log4j.rootLogger=INFO,stdout,flume
log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.target = System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
#log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:778777ss,SSS} [%t] [%c] [%p] - %m%n
#flume
log4j.appender.flume = org.apache.flume.clients.log4jappender.Log4jAppender
log4j.appender.flume.Hostname = master
log4j.appender.flume.Port = 44444
log4j.appender.flume.layout= org.apache.log4j.PatternLayout
log4j.appender.flume.UnsafeMode = true
```

**3.介绍**

```
如果有日志产生他就会向master机子的44444端口发送消息。
```

