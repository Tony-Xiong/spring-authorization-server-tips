## 生态变更

- springboot 2.X 时代的 oauth 相关的包不再更新，3.0开始并入了 spring security 主版本
- 配置写法全变了

## Spring Security 6 下，配置的变更

- Spring Security 6 下，做配置不再使用 Adapter，需要直接配置相关的Bean来替代默认Bean
- 需要配置SecurityWebFilterChain的话，直接返回SecurityWebFilterChain接口类型的Bean
- mvc 和 webflux 的security开启，共用一个注解 @EnableWebSecurity

## Spring Security OAuth 2

Group：org.springframework.boot

### authorization server
- spring-boot-starter-oauth2-authorization-server
- 功能主要就是jwt生成，存放，撤回，用户信息查询等功能

### resource server
- spring-boot-starter-oauth2-resource-server
- 最少配置 spring.security.oauth2.resourceserver.jwt.issuer-uri
- resource 服务，只校验jwt，并根据jwt的有效性，来进行业务
- resource server 可以不是一个具有web接口的站点，可以是一个消息处理器这样的一个纯服务

### client server
- spring-boot-starter-oauth2-client
- 需要配置如下参数
	- spring.security.oauth2.client.registration.spring.provider=spring  
	- spring.security.oauth2.client.provider.spring.issuer-uri=http://localhost:8080  
	- spring.security.oauth2.client.registration.spring.client-id=client  
	- spring.security.oauth2.client.registration.spring.client-secret=password  
	- spring.security.oauth2.client.registration.spring.authorization-grant-type=authorization_code  
	- spring.security.oauth2.client.registration.spring.client-authentication-method=client_secret_basic  
	- spring.security.oauth2.client.registration.spring.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}  
	- spring.security.oauth2.client.registration.spring.scope=user.read,openid
- client 和 resource的区别
	- 除了 jwt 校验，具有登陆跳转等功能
	- 可以接入多个服务跳转
- 如果是gateway项目，会增加TokenRelayGatewayFilterFactory，可以进行tokenRelay
- client一般就是配置在gateway上的