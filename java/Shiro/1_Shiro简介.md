# Shiro 简介

- Apache Shiro 是Java的一个安全（权限）框架
- Shiro可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE环境，也可以用在JavaEE环境。
- Shiro 可以完成：认证、授权、加密、会话管理、与Web集成、缓存等。

基本功能点如下图所示

![shiro1.png](0_images/shiro1.png)

1. **Authentication**:身份认证/登录，验证用户是不是拥有相应的身份；
2. **AuthOrization**:授权，即权限认证，验证某个已认证的用户是否拥有某个权限，即判断用户是否能进行什么操作，如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用户对某个资源是否具有某个权限；
3. **SessionManager**:会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中。会话可以是普通的JavaSE环境，也可以是Web环境。
4. **Cryptography**:加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；
5. **Web Support**:Web支持，可以非常容易的集成到web环境。
6. **Caching**:缓存，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，能把权限自动传播过去。
7. **Testing**:提供测试支持
8. **Run As**:允许一个用户假装为另一个用户（如果他们允许）的身份进行访问；
9. **Remember Me**:记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了。

## Shiro 架构

1. **从外部来看Shiro**
    即从应用程序角度的来观察如何使用Shiro完成工作：
    ![shiro2.png](0_images/shiro2.png)
    - **Subject**:应用代码直接交互的对象是Subject，也就是说Shiro的对外API核心就是Subject。Subject代表了当前的“用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject，如网络爬虫，机器人等；与Subject的所有交互都会委托给SecurityManager；Subject其实是一个门面，SecurityManager才是实际执行者。
    - **SecurityManager**：安全管理器，即所有与安全有关的操作都会与SecurityManager交互，且其管理着所有的Subject。可以看出它是核心，它负责与Shiro的其他组件进行交互，它相当于SpringMVC中的DispatchServlet的角色。
    - **Realm**:Shiro从Realm获取安全数据（如用户、角色、权限），就是说SecurityManager要验证用户身份，那么它需要从Realm获取相应的用户进行比较以确定用户身份是否合法，也需要从Realm得到用户相应的角色/权限进行验证用户是否能进行操作，可以把Realm看成DataSource。

2. 从内部来看Shiro
    ![shiro3.png)](0_images/shiro3.png)
    - **Subject**:任何可以和应用交互的"用户";
    - **SecurityManager**：相当于SpringMVC中的DispatchServlet，是Shiro的心脏，所有具体的交互都通过SecurityManager进行控制，它管理着所有Subject，且负责进行认证、授权、会话及缓存的管理
    - **Authentication**:负责Subject认证，是一个扩展点，可以自动以实现，可以使用认证策略(Authentication Strategy)，即什么情况下用户算认证通过。
    - **AuthOrizer**:授权器，即访问控制器，用来决定主体是否有权限进行相应的操作。即控制着用户能访问应用中的哪些功能；
    - **Realm**:可以有1个或者多个Realm，可以认为是安全实体数据源，即用于获取安全实体的。可以是JDBC实现，也可是内存实现等等，由用户提供，所以一般在应用中都需要实现自己的Realm；
    - **SessionManager**：管理Session生命周期的组件。而Shiro并不仅仅可以用在Web环境，也可以用在如普通的Java环境
    - **CacheManager**:缓存控制器，来管理如用户、角色、权限等的缓存的。因为这些数据基本上很少改变，放到缓存中可以提高访问的性能
    - **Cryptography**:密码模块，Shiro提高了一些常见的加密组件用于如密码加密/解密。
