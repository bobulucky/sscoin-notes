
###Java程序运行原理
在Java中引入了虚拟机（JVM, Java Virtual Machine）的概念，即在机器和编译程序之间加入了一层抽象的虚拟机器。虚拟机在任何平台上都提供给编译程序一个共同的接口。

编译程序只需要面向虚拟机，生成虚拟机能够理解的**字节码**（ByteCode）（class文件的内容），然后由**解释器**来将虚拟机代码转换为特定系统的机器码执行，每一种平台的解释器是不同的，但是实现的虚拟机是相同的。

Java源程序经过编译器编译成字节码，字节码由虚拟机解释执行，虚拟机将每一条要执行的字节码送给解释器，解释器将其翻译成为特定机器上的机器码，然后在特定的机器上运行。

> java编译器（编译） --> 虚拟机（解释执行） -->解释器（翻译） -->机器码

###Java程序运行过程
写好java代码，保存到硬盘，然后命令行执行
> javac YourClassName.java

此时，java代码就被编译成字节码（.class）
> java YourClassName

此时，JRE的类加载器从硬盘中读取class文件，载入到系统分配给JVM的内存区域--运行数据区（Runtime Data Areas）。然后执行引擎解释或者编译类文件，转换成特定CPU的机器码，CPU执行机器码，至此完成整个过程。

###Java环境变量
【Path】
在JDK的安装路径下，我们很容易发现bin文件下的javac、java命令——分别用于执行【编译】和【解释】的操作。配置path环境变量便可实现直接找到这两个命令并运行。

【ClassPath】
而classpath变量，通过后来的学习得知，在jdk1.5以上版本便可以不用配置。首先要知道java源程序通过编译生成的class文件，默认会存储到JRE文件下，也就是java运行环境路径下，在1.5之前的jdk还没智能到可以自动找到编译好的类，进行下一步解释操作，故需要手动配置classpath，指明class文件路径，在执行java命令形成可执行文件。

【JavaHome】
另一个javahome再单独运行java程序时是不需要进行配置的，因为编译、解释均已通过javac 和java命令完成。但在tomcat、jboss部署时需要配置该环境变量。

###JDK、JRE、JVM三者之间的关系
JDK（Java Development Kit）是针对Java开发员的产品，是真个java的核心，包括了java运行环境JRE、Java工具和Java基础类库。Java Runtime Environment (JRE) 是运行Java程序所必须的环境的集合，包含JVM标准实现及Java核心类库。JVM是Java Virtual Machine（虚拟机）的缩写，是整个Java实现跨平台最核心的部分，能够运行以Java语言写作的软件程序。

JDK是Java开发工具包。
JDK中包含JRE，在JDK的安装目录下有一个名为jre的目录，里面有两个文件夹bin和lib，在这里可以认为bin里的就是jvm，lib中则是jvm工作所需要的类库，而jvm和 lib和起来就称为jre。

* SE(J2SE)，standard edition，标准版，是我们通常用的一个版本，从JDK 5.0开始，改名为Java SE。
* EE(J2EE)，enterprise edition，企业版，使用这种JDK开发J2EE应用程序，从JDK 5.0开始，改名为Java EE。
* ME(J2ME)，micro edition,主要用于移动设备，嵌入式设备上的Java应用程序，从JDK 5.0开始，改名为Java ME。
