### \#Arrays.asList常见问题
1、使用Arrays.asList()方法把一个数组转换成list列表时，对得到的List列表进行add()和remove()操作，会抛出**java.lang.UnsupportedOperationException异常**  
Arrays.asList返回的List是一个不可变长度的列表，此列表不具备原List的很多特性。
2、 Arrays.asList接收空数组，list列表调用size的结果是1而非0
