# Java 安全管理器（Security Manager)

根据维基百科中对 [Java security](https://en.wikipedia.org/wiki/Java_security) 的介绍，可以把 Java 安全管理器可以当成是一个**沙箱**环境，可以运行一些不受信任的代码，防止这些代码访问平台的 API 或者是一些私有数据。

例如，可能会阻止不受信任的代码在本地文件系统上读/写入文件、使用当前用户的特权运行任意命令、访问通信网络、使用反射访问对象的内部私有状态或导致 JVM 退出。

也就是说**沙箱**负责保护一些系统资源，而且保护级别是不同的：

- 内部资源，如本地内存；
- 外部资源，如访问其文件系统或是在同一局域网的其他机器；
- 对于运行的组件（applet），可以访问其web服务器；
- 主机通过网络传输到磁盘的数据流。

一般来讲，沙箱的默认状态允许其中的程序访问CPU、内存等资源，以及其上装在的Web服务器。若沙箱完全开放，则其中程序的权限与主机相同。









> 可以简单的理解为，Java 安全管理器控制了代码权限。



## 启动安全管理器

Java 默认不会启动安全管理器，但是可以在 JVM 启动时候设置 `-Djava.security.manager` 选项来启动，也可以再使用 `-Djava.security.policy` 选项指定一个安全管理的配置文件。默认的安全管理器配置文件是 `$JAVA_HOME/jre/lib/security/java.policy`，即当未指定配置文件时，将会使用该配置。

在启用安全管理器的时候，配置遵循以下基本原则：

1. 没有配置的权限表示没有。
2. 只能配置有什么权限，不能配置禁止做什么。
3. 同一种权限可多次配置，取并集。
4. 统一资源的多种权限可用逗号分割。



## 基本使用

`java.lang.SecurityManager` 类包含了很多 `checkXXXX` 方法，如用于判断对文件访问权限的 `checkRead(String file)` 方法。

这些检查方法调用 `SecurityManager.checkPermission` 方法，后者根据安全策略文件判断调用应用是否有执行所请求的操作权限。如果没有，将引发 `SecurityException`。

调用 `checkXXXX` 方法通常如下所示：

```java
SecurityManager security = System.getSecurityManager();
if (security != null) {
    security.checkXXX(argument,  . . . );
}
```

因此，安全管理员有机会通过抛出异常来防止操作完成。 安全管理器例程只是简单地返回如果允许在运行，但抛出一个`SecurityException`如果操作是不允许的。 这个惯例的唯一例外是`checkTopLevelWindow` ，它返回一个`boolean`值。

当前的安全管理器由`setSecurityManager`方法在`System`类中`System` 。 当前的安全管理器是通过`getSecurityManager`方法获得的。

























































## 参考资料

[Java security](https://en.wikipedia.org/wiki/Java_security)

[java安全管理器SecurityManager入门](https://www.cnblogs.com/yiwangzhibujian/p/6207212.html)

[Java安全：SecurityManager与AccessController](https://juejin.cn/post/6844903657775824910)



https://www.itread01.com/content/1546970610.html



https://www.itread01.com/content/1546796712.html