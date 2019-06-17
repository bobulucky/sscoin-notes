
###部署外部tomcat打成war包pom.xml调整
打包方式修改
```<packaging>jar</packaging>```
>变更为

```<packaging>war</packaging>```

禁用内置tomcat
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <!-- provided 表明该包只在编译和测试的时候使用，去除默认的tomcat -->
    <scope>provided</scope>
</dependency>
```
也可排除嵌入tomcat
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
修改启动类
>继承SpringBootServletInitializer覆盖configure方法

```
@SpringBootApplication
public class SpringbootTomcatApplication  extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootTomcatApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(SpringbootTomcatApplication.class);
    }
}
```
使用maven打包在target文件夹下生产war包，将war包copy到tomcat webapp文件夹下，进入到bin目录下startup.bat启动tomcat
这时如果tomcat低于8会有两个jar包冲突
log4j包冲突
>解决方法

```
<properties>
    <log4j.version>2.7</log4j.version>
</properties>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>${log4j.version}</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-jcl</artifactId>
    <version>${log4j.version}</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>${log4j.version}</version>
</dependency>
```

el-api包的冲突
```Caused by: java.lang.NoClassDefFoundError: javax/el/ELManager```
>解决方法

方法一：使用tomcat8.0以上
方法二：将el-api.jar移到tomcat7的lib下进行替换
