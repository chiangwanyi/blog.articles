# 1. 导入依赖

- 父项目`pom.xml`

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.4.RELEASE</version>
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

- 子项目`pom.xml`

```xml
<dependencies>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.10.0</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.68</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.10</version>
    </dependency>
</dependencies>
```

# 2. 将项目设置为 Web 项目

![image-20200405113857205](http://q8aqauxg5.bkt.clouddn.com/blog/image-20200405113857205.png)

# 3. `web.xml`核心配置

在`web.xml`中需要配置

- `DispatchServlet`，这是 SpringMVC 的核心
- `CharacterEncodingFilter`，用于处理乱码问题

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- DispatchServlet -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--  处理乱码问题 -->
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

# 4. `springmvc-servlet`核心配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 开启注解扫描 -->
    <context:component-scan base-package="com.controller"/>

    <!-- 不处理静态资源 -->
    <mvc:default-servlet-handler/>

    <!-- 处理 JSON 乱码问题 -->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <property name="supportedMediaTypes" value="text/html;charset=utf-8"/>
            </bean>
            <bean id="jacksonMessageConverter"
                  class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="supportedMediaTypes" value="application/json;charset=utf-8"/>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

# 5. `Controller`

## 5.1 普通`Controller`

- `@Controller`：标记类为一个`Controller`
- `@RequestMapping()`：设置请求映射路径，具有父子嵌套特性
- `@RequestParam`：标记属性为请求时传入的属性

```java
@Controller
@RequestMapping("/user")
public class UserController {

    @RequestMapping("/u1")
    public String test1(@RequestParam("username") String name, Model model) {
        model.addAttribute("msg", new User(name, 12));
        return "test";
    }

    @RequestMapping("/u2")
    public String test2(User user, Model model) {
        model.addAttribute("msg", user);
        return "test";
    }

    @RequestMapping("/u3")
    public String test2(@RequestParam("name") String name, Model model) {
        model.addAttribute("msg", name);
        return "test";
    }
}
```

## 5.2 RESTful 风格`Controller`

- `@PathVariable`：配合`@RequestMapping`使用，代表`url`中的`{}`
- `@PostMapping`：直接设置方法为`POST`方法，其他类型类似

```java
@Controller
public class RestfulController {

    // localhost:8080/add?a=1&b=2
    @RequestMapping("/add")
    public String test1(int a, int b, Model model) {
        int res = a + b;
        model.addAttribute("msg", "结果为" + res);
        return "test";
    }

    // RESTful: localhost:8080/add/1/2
    @RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.POST)
    public String test2(@PathVariable int a, @PathVariable int b, Model model) {
        int res = a + b;
        model.addAttribute("msg", "结果为" + res);
        return "test";
    }

    @PostMapping("/add2/{a}/{b}")
    public String test3(@PathVariable int a, @PathVariable int b, Model model) {
        int res = a + b;
        model.addAttribute("msg", "结果为" + res);
        return "test";
    }
}
```

## 5.3 返回 JSON 数据 `Controller`

- `@RestController`：标记`controller`所有方法只返回字符串，不会被`ViewResolver`处理
- `@ResponseBody`：标记方法只返回字符串，不会被`ViewResolver`处理
- `@RequestMapping(value = "/xxx", produces = "application/json;charset=utf-8")`：`produces`属性可以直接设置方法返回值的**类型**和**字符集**

```java
@RestController // 该类下的所有方法只会返回字符串，不会返回给视图解析器
public class UserController {

//    @RequestMapping(value = "/user/add", produces = "application/json;charset=utf-8")
    @RequestMapping("/user/add")
//    @ResponseBody
    public String test1() throws JsonProcessingException {
        User user = new User("蒋万艺", 1, "男");

        ObjectMapper mapper = new ObjectMapper();
        return mapper.writeValueAsString(user);
    }

    @RequestMapping(value = "/user/now")
    public String test2() throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper();

        mapper.configure(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS, false);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss:SSS");

        mapper.setDateFormat(sdf);

        return mapper.writeValueAsString(new Date());
    }

    @RequestMapping(value = "/user/list")
    public String test3(){
        List<User> users = new ArrayList<User>();
        users.add(new User("蒋万艺", 1, "男"));
        users.add(new User("蒋万艺", 1, "男"));
        users.add(new User("蒋万艺", 1, "男"));
        users.add(new User("蒋万艺", 1, "男"));
        users.add(new User("蒋万艺", 1, "男"));

        return JSON.toJSONString(users);
    }
}

```

