java

---

从node.js 到Java nio 到netty 这些都是非阻塞IO的实现。
传统的Bio模式是阻塞的 代码如下

```java
while (! bCanExit ) {

try {

           // 该方法一直会阻塞，直到有新的连接过来

          Socket socket = server .accept();

         Connection connection = new Connection(socket);

         connection.setClientId(Util.random32UUID ());

         connectionManager .add(connection);

         if ( logger .isInfoEnabled()){

              logger .info( " 有一个新的连接 !" );

         }

} catch (IOException e) {}

}

try {

server .close();

} catch (IOException e) {

logger. error( "close serverSocket error:" , e);

}
```

accept方法会一直在那里死等，直到有连接过来。在连接过来之前只能干等着，而异步IO的思想就是让主线程不阻塞。
拿node.js来讲，node是只有单线程机制，所有的请求都由这个线程处理，但是对于IO操 作其实质是用户空间中的程序不用依赖内核空间中的I/O操作实际完成，即可进行后续任务。
