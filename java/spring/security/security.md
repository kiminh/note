# Spring security

## Base

### User & Role

### presistence

## theory

### Filters

- 核心过滤器调用顺序

过滤器 | 作用
:-:|:-:
WebAsyncManagerIntegrationFilter | 根据请求封装获取 `WebAsyncManager`, 从 `WebAsyncManager` 中获取/注册 `SecurityContextCallableProcessingInceptor`
SecurityContextPresistenceFilter | 请求来临时, 创建 `SecurityContext`, 请求结束时m 清空 `SecurityContextHeader`
JeaderWroterFilter | 用来给 HTTP 响应添加 Header, 如 `X-Frame-Options`, `X-XSS-Protection*`, `X-Content-Type-Options`
LogoutFilter | 退出过滤器, 简单操作就是删除 Session, 根据 SpringSecurity 初始化配置的退出地址来匹配请求
**UsernamePasswordAuthenticationFilter** | 用户名和密码校验 Filter, 只对登录请求进行处理, 不处理其他请, 其逻辑在其父类 AbstractAuthenticationProcessingFilter 中
DefaultLoginPageGeneratingFilter | 登录失败请求或者登出请求跳转到登录页面
**BasicAuthenticationFilter** | 处理 Basic authentication 认证方式, 简单理解和 `UsernamePasswordAuthenticationFilter` 类似, 不过 `UsernamePasswordAuthenticationFilter` 通过表单提交, `BasicAuthenticationFilter` 认证方式只是将用户信息添加到 Request Hander 中
RequestCacheAwareFilters | 内部维护了一个 RequestCache, 用于缓存 Request 请求
SecurityContextHolderAwareRequestFilter | 对 ServletRequest 进行封装, 使得 Request 具有更加丰富的 API
AnonymousAuthenticationFIlter | 匿名身份过滤器, Spring 为兼容为登录的访问, 也哦足了一套认证流程, 使用了匿名身份
SessionManagerFilter | Sensing 相关的 Filter, 内部维护了一个 `SessionAuthenticationStrategy`, 两者联合使用, 防止 session-fixaction pritection attack, 以及限制同一用户开启多个会话数量
ExceptionTranslationFilter | 异常转换过滤器, 位于整个 `FilterChain` 后方, 用来转换整个脸露中的异常, 一般只处理 `AccessDeniedException` 和 `AuthenticationException`
FilterSecurityIntercepter | 进行角色相关的处理, 是登录是否成功的最后一个判断的拦截器, 针对不需要认证和需要认证的请求会进行不同的验证处理

## Dynamic role

spring security默认是在代码里约定好权限, 真实的业务场景通常需要可以支持动态配置角色访问权限, 即在运行时去配置 URL 对应的访问角色。
