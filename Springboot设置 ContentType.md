## Springboot设置 ContentType
选择使用设置默认 ContentType为application/json的方式。
```
@Override
    public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        configurer
                .favorPathExtension(true)
                .favorParameter(false)
                .ignoreAcceptHeader(true)
                .defaultContentType(MediaType.APPLICATION_JSON_UTF8)
                .mediaTypes(negotiationMediaTypes);
    }
```

将ignoreAcceptHeader设置成 true,则通过 RequestMapping中的 produces 来指定 contentType 失效，保留了请求后缀来决定 contentType的方式。
但是需要打开配置项
```
spring.mvc.pathmatch.use-registered-suffix-pattern=true
```
或者
```
spring.mvc.pathmatch.use-suffix-pattern=true
```
第一项只支持项目配置的mediaType

整体设置参考[Content Negotiation using Spring MVC](https://spring.io/blog/2013/05/11/content-negotiation-using-spring-mvc)