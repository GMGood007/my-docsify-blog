## 05_父工程Project空间新建

spring boot 的特点： 约定 > 配置 > 编码

创建微服务cloud整体聚合父工程Project，有8个关键步骤：

1.New Project - maven工程 - create from archetype: maven-archetype-site

2.聚合总父工程名字

3.Maven选版本

4.工程名字

5.字符编码 - Settings - File encoding

6.注解生效激活 - Settings - Annotation Processors

7.Java编译版本选8

8.File Type过滤 - Settings - File Type

## 06_父工程pom文件

```java
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.gm.springcolud</groupId>
  <artifactId>cloud2020</artifactId>
  <version>1.0-SNAPSHOT</version>

  <modules>
    <module>cloud-provider-payment8001</module>
  </modules>
  <packaging>pom</packaging>

  <!-- 统一管理jar包版本 -->
  <properties>
    <junit.version>4.12</junit.version>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring-boot.version>2.2.2.RELEASE</spring-boot.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.16.18</lombok.version>
    <mysql.version>8.0.23</mysql.version>
    <druid.version>1.2.5</druid.version>
    <mybatis-plus.version>3.4.2</mybatis-plus.version>
    <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
  </properties>

  <!-- 子模块继承之后，提供作用：
      锁定版本+子modlue不用写groupId和version -->
  <dependencyManagement>
    <dependencies>
      <!--spring boot 2.2.2-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud Hoxton.SR1-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
<!--        <version>${spring-cloud.version}</version>-->
        <version>Hoxton.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud alibaba 2.1.0.RELEASE-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
<!--        <version>${spring-cloud-alibaba.version}</version>-->
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
      </dependency>
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
      <!--mybatis plus extension,包含了mybatis plus core-->
      <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-extension</artifactId>
        <version>${mybatis-plus.version}</version>
      </dependency>
      <!--mybatis-->
      <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>${mybatis-plus.version}</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
      </dependency>
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <version>2.1.7.RELEASE</version>
        <configuration>
          <fork>true</fork>
          <addResources>true</addResources>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>

```

## 07_复习Maven

## 08_支付模块构建(上)

### 2.1建立子模块

![image-20220805114452444](/Users/gmgood007/Library/Application Support/typora-user-images/image-20220805114452444.png)

1. 建module
2. 改POM
3. 写YML
4. 主启动
5. 业务类

#### 1.建立**子模块的Maven工程**

#### 2.改POM

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud2020</artifactId>
        <groupId>com.gm.springcolud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8001</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-extension</artifactId>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
        <!--mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
</project>
```

#### 3.写YML

```yml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: com.mysql.cj.jdbc.Driver            # mysql驱动包
    url: jdbc:mysql://127.0.0.1:3306/mystudy?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456

```



#### 4.主启动

```java
@SpringBootApplication
public class PaymentMain001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain001.class, args);
    }
}
```

## 09_支付模块构建(中)

#### 5.业务类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment implements Serializable {
    private Long id;
    private String serial;
}
```

> 这里往下就是 Mapper ，Service ,Controller 一系类常用增删改查

## 10_支付模块构建(下)

#### 6.测试

- 浏览器 - http://localhost;8001/payment/get/1

![结果](https://gmgood007-1259801250.cos.ap-nanjing.myqcloud.com/study/结果.png)

#### 7.补充

`接口响应`

```java
package com.gm.springcloud.common;

import com.gm.springcloud.constant.CommonConstants;
import lombok.*;
import lombok.experimental.Accessors;

/**
 * @author gmgood007
 */


@ToString
@Accessors(chain = true)
@AllArgsConstructor
@NoArgsConstructor
public class Result<T> {
    /**
     * 返回标记：成功标记=0，失败标记=1
     */
    @Getter
    @Setter
    private int code;
    /**
     * 返回信息
     */
    @Getter
    @Setter
    private String msg;
    /**
     * 数据
     */
    @Getter
    @Setter
    private T data;



    public static <T> Result<T> ok() {
        return restResult(null, CommonConstants.SUCCESS, null);
    }

    public static <T> Result<T> ok(T data) {
        return restResult(data, CommonConstants.SUCCESS, null);
    }

    public static <T> Result<T> ok(T data, String msg) {
        return restResult(data, CommonConstants.SUCCESS, msg);
    }

    public static <T> Result<T> failed() {
        return restResult(null, CommonConstants.FAIL, null);
    }

    public static <T> Result<T> failed(String msg) {
        return restResult(null, CommonConstants.FAIL, msg);
    }

    public static <T> Result<T> failed(T data) {
        return restResult(data, CommonConstants.FAIL, null);
    }

    public static <T> Result<T> failed(T data, String msg) {
        return restResult(data, CommonConstants.FAIL, msg);
    }

    private static <T> Result<T> restResult(T data, int code, String msg) {
        Result<T> apiResult = new Result<>();
        apiResult.setCode(code);
        apiResult.setData(data);
        apiResult.setMsg(msg);
        return apiResult;
    }


}
##枚举响应
public interface CommonConstants {
    /**
     * 成功标记
     */
    Integer SUCCESS = 0;
    /**
     * 失败标记
     */
    Integer FAIL = 1;
}


```

## 11_热部署Devtools

这里直接跳过。

## 12_消费者订单模块(上)

### 1.建Module

创建名为cloud-consumer-order80的maven工程。

2.改POM

```java
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0">
    <parent>
        <artifactId>cloud2020</artifactId>
        <groupId>com.gm.springcolud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <modelVersion>4.0.0</modelVersion>
    <artifactId>cloud-consumer-order80</artifactId>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-extension</artifactId>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
        <!--mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
</project>

```

### 3.写YML

```yml
server:
  port: 80
```

### 4.主启动

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 *
 */
@SpringBootApplication
public class OrderMain80
{
    public static void main( String[] args ){
        SpringApplication.run(OrderMain80.class, args);
    }
}
```

### 5.业务类

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment implements Serializable {
    private Long id;
    private String serial;
}

```

```java
package com.gm.springcolud.common;

import com.gm.springcolud.constant.CommonConstants;
import lombok.*;
import lombok.experimental.Accessors;

/**
 * @author gmgood007
 */


@ToString
@Accessors(chain = true)
@AllArgsConstructor
@NoArgsConstructor
public class Result<T> {
    /**
     * 返回标记：成功标记=0，失败标记=1
     */
    @Getter
    @Setter
    private int code;
    /**
     * 返回信息
     */
    @Getter
    @Setter
    private String msg;
    /**
     * 数据
     */
    @Getter
    @Setter
    private T data;



    public static <T> Result<T> ok() {
        return restResult(null, CommonConstants.SUCCESS, null);
    }

    public static <T> Result<T> ok(T data) {
        return restResult(data, CommonConstants.SUCCESS, null);
    }

    public static <T> Result<T> ok(T data, String msg) {
        return restResult(data, CommonConstants.SUCCESS, msg);
    }

    public static <T> Result<T> failed() {
        return restResult(null, CommonConstants.FAILURE, null);
    }

    public static <T> Result<T> failed(String msg) {
        return restResult(null, CommonConstants.FAILURE, msg);
    }

    public static <T> Result<T> failed(T data) {
        return restResult(data, CommonConstants.FAILURE, null);
    }

    public static <T> Result<T> failed(T data, String msg) {
        return restResult(data, CommonConstants.FAILURE, msg);
    }

    private static <T> Result<T> restResult(T data, int code, String msg) {
        Result<T> apiResult = new Result<>();
        apiResult.setCode(code);
        apiResult.setData(data);
        apiResult.setMsg(msg);
        return apiResult;
    }
}
```

控制层：

```java
package com.gm.springcolud.controller;

import com.gm.springcolud.common.Result;
import com.gm.springcolud.entity.Payment;
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.client.RestTemplate;

@Slf4j
@RestController
@RequestMapping(value = "/order")
@AllArgsConstructor
public class OrderController {
    public static final String PAYMENT_URL = "http://localhost:8001";

    private  final  RestTemplate restTemplate;

    @GetMapping("/create")
    public Result create(@RequestBody Payment payment){
        return   restTemplate.postForObject(PAYMENT_URL + "/payment/create", payment, Result.class);

    }

    @GetMapping("/get/{id}")
    public Result getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id, Result.class);
    }
}

```

6.测试

运行cloud-consumer-order80与cloud-provider-payment8001两工程

- 浏览器 - http://localhost/consumer/payment/get/1

![结果](https://gmgood007-1259801250.cos.ap-nanjing.myqcloud.com/study/20220819104333.png)

### **RestTemplate**

[官网地址](https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html)

## 13_消费者订单模块(下)

开启`Run Dashboard`

1. 打开工程路径下的.idea文件夹的workspace.xml
2. 在`<component name="RunDashboard">`中修改或添加以下代码

```xml
  <component name="RunDashboard">
    <option name="configurationTypes">
      <set>
        <option value="SpringBootApplicationConfigurationType" />
      </set>
    </option>
  </component>
```

![效果图](https://gmgood007-1259801250.cos.ap-nanjing.myqcloud.com/study/20220819102613.png)

## 14_工程重构

