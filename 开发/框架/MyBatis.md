## MyBatis

SqlSessionFactory 实例   

SqlSessionFactoryBuilder 构建者   

使用xml文件构建SqlSessionFactory实例是非常简单的事

MyBatis最强大的特性之一就是它的 ==动态语句功能==


#### MyBatis简单使用

1. 创建Mybatis配置文件   
  Configure.xml
  ```
  <configuration>
          <typeAliases>
                  <typeAlias alias="" type=""/>
          </typeAliases>
          <environments default="">
                  <enviroment id="">
                          <transactionManager type="JDBC"/>
                          <dataSource type="POOLED">
                                  <!-- 数据库配置文件-->
                                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                                  <property name="url" value="jdbc:mysql://127.0.0.1:3306/xxxx"/>
                                  <property name="username" value="root"/>
                                  <property name="password" value="root"/>
                          </dataSource>
                  </enviroment>
          </enviroments>

          <mappers>
                  <mapper resource="pakagePath/映射文件.xml"/>
          <mappers>
  </configuration>
  ```

  2. 创建实体类和映射文件
  创建类对应的映射文件xxx.xml
  ```
  <mapper namespace="pakagePath.xxx.xxx.models.xxxMapper">
          <select id="getUserByID" parameterType="int" resultType="User">
                  select * from user where id=#{id}
          </select>
  </mapper>
  ```

  配置文件Configure.xml是mybatis用来建立sessionFactory    
  里面包含了数据库连接相关内容，还有java类所对应的别名。这个别名要和映射文件resultType对应。   
  Configure.xml 里面的mapper是包含要映射类的xml配置文件   
  在映射文件里定义各种SQL语句，以及这些语句的参数和要返回的类型等等。   

  3. 执行操作

  `Reader reader = Resources.getResourceAsReader("config/Configure.xml")`   
  `SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader)`   
  `SqlSession session = sqlSessionFactory.openSession);`   
  `session.selectOne("mapper namespace + select id", id)`     

  #### 总结

* **关于parameterType:** 就是用来在SQL映射文件中指定输入参数类型的，可以指定为基本数据类型(如int,float等)，包装数据类型(如String,Interger等)以及用户自己编写的JavaBean封装类   
* **关于resultType:** 在加载SQL配置，并绑定指定输入参数和运行SQL之后，会得到数据库返回的响应结果，此时使用resultType就是用来指定数据库返回的信息对应的Java的数据类型。  
* **关于"#{}":**

* **关于"${}":**
