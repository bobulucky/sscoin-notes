###流的概念和作用
> 流：代表任何能够输出数据的数据源对象或者是有能力接受数据的接收端对象 ---Thinking in Java

> 流的本质：数据传输，根据数据传输的特性将流抽象为各种类，方便更直观的进行数据操作。

>流的作用：为数据源和目的地建立一个传输通道


###IO模型
Java的IO模型设计使用了Decorator(装饰者)模式，按功能划分Stream


###流的分类
* 根据处理数据类型的不同分为： 字符流和字节流
* 根据数据流向的不同分为：输入流和输出流
* 根据数据源(去向)分类：
1、File(文件)：FileInputStream,FileOutputStream,FileReader,FileWriter
2、byte[]:ByteArrayInputStream,ByteArrayOutputStream
3、Char[]:CharArrayReader,CharArrayWriter
4、String[]:StringBufferInputStream,StringBufferOutputStream,StringReader,StringWriter
5、网络数据流：InputStream,OutStream,Reader,Writer


###字符流和字节流转换
转换流的特点：
1. 其实是字符流和字节流之间的桥梁
2. 可对读取到的字节数据经过指定编码转换成字符
3. 可对读取到的字符数据经过指定编码转换成字节

何时使用转换流：
1. 当字节和字符之间有转换动作时
2. 流操作的数据需要编码或解码时

转换流：在IO中还存在一类是转换流，将字节转换为字符流，同时可以将字符流转化为字节流。

1. InputStreamReader:字节到字符的桥梁
2. OutputStreamWriter:字符到字节的桥梁

###字节流和字符流的区别
字节流没有缓冲区，是直接输出的，而字符是输出到缓冲区的。因此在输出时，字节流不调用close()方法时，信息已经输出了，而字符流只有在调用close()方法关闭缓冲区时，信息才输出。要想字符流在未关闭时输出信息，则需要手动调用flush()方法。
* 读写单位不同：字节流以字节(8bit)为单位，字符流以字符为单位，根据码表映射字符，一次可能读多个字节。
* 处理对象不同：字节流能处理所有类型的数据(如图片、avi等)，而字符流只能处理字符类型的数据。  

结论：只要是处理存文本的数据，就优先考虑使用字符流。除此之外都使用字节流。
