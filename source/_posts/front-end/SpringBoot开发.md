---
title: SpringBoot开发
tag:
  - springBoot
  - Vue
categories: front_end
abbrlink: d3394559
---



# Web 入门

+ Spring Boot 将传统 Web 开发的 MVC, JSON, Tomcat 等框架整合, 提供了 spring-boot-starter-web 组件, 简化了 Web 应用的配置
+ 创建 SpringBoot 项目 勾选 Spring Web 选项后, 会自动将 spring-boot-starter-web 组件加入到项目中
+ webmvc 是 Web 开发的基础框架, json 为 JSON 数据解析组件, tomcat 为自带容器依赖



+ `Spring Boot` 提供了 `@Controller` 和 `@RestController` 两种注解
  1. `@Controller` 返回页面和数据
  2. `@RestController` 返回数据, 自动将返回值转换为 JSON 格式返回



## URL 映射

+ `@RequestMapping` 注解主要负责 URL 的路由映射, 可以添加在 `Controller` 类或者具体方法上
  1. 若添加在 `Controller` 类上, 则这个 `Controller` 中的所有路由映射都将加上此映射规则
  2. 若添加在方法上, 则只对当前方法有效
+ 参数
  + `value`: 请求 URL 的路径, 支持 URL 模板、正则表达式
  + `method`: HTTP 请求方法
  + `consumes`: 请求的媒体类型(Content-Type), 如 `application/json`
  + `produces`: 响应的媒体类型
  + `params`: 请求参数
  + `headers`: 请求头的值

## 参数传递

+ `@RequestParam` 可以将请求参数绑定到控制器的方法参数上, 介绍的参数来着 HTTP 请求体或请求 URL 的QueryString, 当请求的参数名称与 Controller 的业务方法参数名称一致时, `@RequestParam` 可省略

```java
// 若使用了 @RequestParam 注解则请求中必须携带该参数
public String getName(@RequestParam("nikename") String name){
    System.out.println(name);
    return name;
}
```



+ `@PathVaraible` 用于处理动态的 URL, URL 的值可作为控制器中处理方法的参数
+ `@RequestBody` 接受的参数是来着 requestBody (请求体), 一般是用于处理非 `Content-Type: application/x-www-form-urlencoded` 编码格式的数据, 例如`application/json`, `application/xml`