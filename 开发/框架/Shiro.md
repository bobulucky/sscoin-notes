##Shiro基本概念

Shiro是Apache下的一个开源项目。
它是一个很易用与Java项目的安全框架，提供了认证、授权、加密、会话管理，
与spring Security 一样都是做一个权限的安全框架，但与Spirng Secutiry相比，在于Shiro使用了比较简单易懂易于使用的授权方式。
shiro属于轻量级框架，相对于security简单的多。

![](https://images2017.cnblogs.com/blog/1031841/201711/1031841-20171110162750169-589847029.png)   

**Authentication:** 身份认证/登录，验证用户是不是拥有相应的身份   
**Authorization:** 授权，即权限认证，验证某个已认证的用户是否拥有某个权限   
**Session Manager:** 会话管理，即用登录后就是一个会话，在没有退出之前，它的所有信息都在会话中   
**Cryptongraphy:** 加密，保护数据的安全性，如密码加密存储到数据库，而不是文明存储   
**Web Suport:** Web支持，可以非常容易的集成到Web环境
**Caching:** 缓存，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，这样可以提高效率
**Concurrency:** shiro支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去
**Testing:** 提供测试支持
**Run As:** 允许一个用户假装为另一个用户（如果他们允许）的身份进行访问
**Remember Me:**  记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了   

![](https://images2015.cnblogs.com/blog/871676/201607/871676-20160722213407794-1894786938.png)   

1. 使用用户的登录信息创建令牌  
    `UsernamePasswordToken token = new UsernamePasswordToken(username, password);`   
    token可以理解为用户令牌，登录的过程被抽象为Shiro验证令牌是否具有合法身份以及相关权限。   
2. 执行登录动作   
     ```
     SecurityUtils.setSecurityManager(securityManager); // 注入SecurityManager
     Subject subject = SecurityUtils.getSubject(); // 获取Subject单例对象
     subject.login(token); // 登陆
     ```   
     Shiro的核心部分是SecurityManager,它负责安全认证与授权。Shiro本身已经实现了所有的细节，用户可以完全把它当做一个黑盒来使用。SecurityUtils对象，本质上就是一个工厂类似Spring中的ApplicationContext。
3. 判断用户   
    Shiro本身无法知道所持有令牌的用户是否合法，因为除了项目的设计人员恐怕谁都无法得知。因此Realm是整个框架中为数不多的必须由设计者自行实现的模块，当然Shiro提供了多种实现的途径。   
4. AuthorizationInfo和AuthenticationInfo   
    AuthorizationInfo代表了用户的角色信息集合，AuthenticationInfo代表了角色的权限信息集合。

#### 实现realm
