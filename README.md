# swagger-bootstrap-ui



## 简介



`swagger-bootstrap-ui`是`Swagger`的前端UI实现,目的是替换`Swagger`默认的UI实现`Swagger-UI`,规范前后端开发流程,减少交流成本,提高工作开发效率.



`swagger-bootstrap-ui` 只是`Swagger`的UI实现,并不是替换`Swagger`功能,所以后端模块依然是依赖`Swagger`的,需要配合`Swagger`的注解达到效果,[注解说明](swagger-annotation.md)





## 功能


* 接口文档说明,效果图如下：



![](https://xiaoymin.github.io/cloud-sdk-doc/assets/docs.jpg)

* 在线调试功能,效果图如下:

![](https://xiaoymin.github.io/cloud-sdk-doc/assets/debug1.jpg)

## Swagger简介



Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。总体目标是使客户端和文件系统作为服务器以同样的速度来更新。文件的方法，参数和模型紧密集成到服务器端的代码，允许API来始终保持同步。Swagger 让部署管理和使用功能强大的API从未如此简单。



Swagger-UI默认效果图如下：



![](https://xiaoymin.github.io/cloud-sdk-doc/assets/swagger.png)



## 下载



`swagger-bootstrap-ui`下载地址：[下载](http://192.168.11.110:8081/nexus/service/local/repositories/thirdparty/content/com/drore/cloud/cloud-document-ui/1.0/cloud-document-ui-1.0.jar)



## 使用说明





* 首先需要引入swagger的配置包信息,如下：



```java

<dependency>

 <groupId>io.springfox</groupId>

 <artifactId>springfox-swagger2</artifactId>

 <version>2.2.2</version>

</dependency>



<!-- 这里swagger-ui是swagger的默认实现,这个jar可以不用引入,使用下面的swagger-bootstrap-ui替代--->

<dependency>

 <groupId>io.springfox</groupId>

 <artifactId>springfox-swagger-ui</artifactId>

 <version>2.2.2</version>

</dependency>



```






* maven项目中引用`swagger-bootstrap-ui`的jar包依赖,如下：



```java

<dependency>

 <groupId>com.drore.cloud</groupId>

 <artifactId>swagger-bootstrap-ui</artifactId>

 <version>1.0</version>

</dependency>



```



* Spring项目中启用swagger,代码如下：



##XML方式



```java



<bean class="com.drore.configuration.SwaggerConfiguration"/>

<mvc:annotation-driven content-negotiation-manager="contentNegotiationManager" />

<bean id="contentNegotiationManager" class="org.springframework.web.accept.ContentNegotiationManagerFactoryBean">

 <property name="favorPathExtension" value="false" />

 <property name="favorParameter" value="false" />

 <property name="ignoreAcceptHeader" value="false" />

 <property name="mediaTypes" >

 <value>

 atom=application/atom+xml

 html=text/html

 json=application/json

 *=*/*

 </value>

 </property>

</bean>



```





##注解方式



```java

@Configuration

@EnableSwagger2

public class SwaggerConfiguration {



 @Bean

 public Docket createRestApi() {

 return new Docket(DocumentationType.SWAGGER_2)

 .apiInfo(apiInfo())

 .select()

 .apis(RequestHandlerSelectors.basePackage("com.drore.cloud"))

 .paths(PathSelectors.any())

 .build();

 }



 private ApiInfo apiInfo() {

 return new ApiInfoBuilder()

 .title("swagger-bootstrap-ui RESTful APIs")

 .description("swagger-bootstrap-ui")

 .termsOfServiceUrl("http://api.test.com/")

 .contact("developer@mail.com")

 .version("1.0")

 .build();

 }



}



```



* `swagger-bootstrap-ui`默认访问地址是：`http://${host}:${port}/doc.html`



## 注意事项



* swagger封装给出的请求地址默认是`/v2/api-docs`,所以`cloud-document-ui`调用后台也是`/v2/api-docs`,不能带后缀,且需返回json格式数据,框架如果是spring boot的可以不用修改,直接使用,如果是Spring MVC在web.xml中配置了`DispatcherServlet`,则需要追加一个url匹配规则,如下：



```java

<servlet>

 <servlet-name>cmsMvc</servlet-name>

 <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

 <init-param>

 <param-name>contextConfigLocation</param-name>

 <param-value>classpath:config/spring.xml</param-value>

 </init-param>

 <load-on-startup>1</load-on-startup>

</servlet>



<!--默认配置,.htm|.do|.json等等配置-->

<servlet-mapping>

 <servlet-name>cmsMvc</servlet-name>

 <url-pattern>*.htm</url-pattern>

</servlet-mapping>

<!-- 配置cloud-document-ui的url请求路径-->

<servlet-mapping>

 <servlet-name>cmsMvc</servlet-name>

 <url-pattern>/v2/api-docs</url-pattern>

</servlet-mapping>



```
