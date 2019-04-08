---
title: java面试清单
date: 2019-02-26 03:22:23
tags: 
- 面试清单
- java

categories: 职业
---
# java基本数据类型取值范围

|                        | 默认值    | 存储需求（字节）  | 取值范围     |
| ---------------------- | --------- | ----------------- | ------------ |
| byte (字节型)          | 0         | 1                 | -2^7—2^7-1   |
| char (字符型)          | ‘ \u0000′ | 2                 | 0—2^16-1     |
| short                  | 0         | 2                 | -2^15—2^15-1 |
| int                    | 0         | 4                 | -2^31—2^31-1 |
| long                   | 0         | 8                 | -2^63—2^63-1 |
| <font color=red>float </font> | 0.0f      | <font color=red>4</font> | -2^31—2^31-1 |
| double                 | 0.0d      | 8                 | -2^63—2^63-1 |
| boolean                | false     | 1                 | true\false   |

# java异常
- 通常分为编译时异常和运行异常。
- <font color=red>编译时异常</font>需要我们手动的进行捕捉处理，也就是我们用<font color=red>try....catch</font>块进行捕捉处理。
- <font color=red>运行时异常</font>只有在编译器在编译运行时才会出现，这些<font color=red>不需要（捕获）</font>我们手动进行处理
- error是系统出错，catch是无法处理的
- RuntimeException不需要程序员进行捕获处理
- 只需要对exception的实例进行捕获即可
- 自定义异常，实现Throwable

```java
class WrongInputException extends Throwable {  // 自定义的类
    WrongInputException(String s) {
        super(s);
    }
}
```
<img src="http://www.yubajin.cn/img/20190407180944-java_exception.jpg" height="500px" width="400px">

# struts1和struts2的区别
- 从<font color=red>action类</font>上分析:
   - Struts1要求Action类继承一个抽象基类。Struts1的一个普遍问题是使用抽象类编程而不是接口。 
   - Struts 2 Action类可以实现一个<font color=red>Action接口</font>，也可实现其他接口，使可选和定制的服务成为可能。Struts2提供一个ActionSupport基类去实现常用的接口。Action接口不是必须的，任何有execute标识的POJO对象都可以用作Struts2的Action对象。
- 从Servlet 依赖分析: 
   - Struts1 Action 依赖于Servlet API ,因为当一个Action被调用时HttpServletRequest 和 HttpServletResponse 被传递给execute方法。 
   - Struts 2 Action<font color=red>不依赖于容器</font>，允许Action脱离容器单独被测试。如果需要，Struts2 Action仍然可以访问初始的request和response。但是，其他的元素减少或者消除了直接访问HttpServetRequest 和 HttpServletResponse的必要性。
- 从action线程模式分析: 
   - Struts1 Action是单例模式并且必须是线程安全的，因为仅有Action的一个实例来处理所有的请求。单例策略限制了Struts1 Action能作的事，并且要在开发时特别小心。Action资源必须是线程安全的或同步的。 
   - Struts2 Action对象<font color=red>为每一个请求产生一个实例</font>，因此没有线程安全问题。（实际上，servlet容器给每个请求产生许多可丢弃的对象，并且不会导致性能和垃圾回收问题）

# 编码字节流到UTF-8编码字节流的转换
1. String (byte[] bytes, String charsetName) 通过使用指定的charsetName解码/指定的byte数组，构造一个新的String
2. String.getBytes(Charset charsetName)
使用给定的 charsetName将此 String 编码到 byte 序列，并将结果存储到新的byte数组。

# java javac jdb
| javac.exe    | java document（文档） | 是编译.java文件    |
| ------------ | --------------------- | ------------------ |
| java.exe     |                       | java虚拟机         |
| javadoc.exe  | java document（文档） | 是生成Java说明文档 |
| jdb.exe      | java debug调试        | 是Java调试器       |
| javaprof.exe | java profile          | 是剖析工具         |

# 对象序列化
1. 对对象进行<font color=red>传输</font>的貌似只有ObjectOutputStream和ObjectInputStream这些以<font color=red>Object</font>开头的流对象。
2. 对象<font color=red>序列化</font>的所属类需要实现<font color=red>Serializable</font>接口
3. static代表类的状态，成员数据不能被串行化
4. <font color=red>transient</font>（短暂的；persisent永久的）代表对象的临时数据，成员数据不能被串行化。
5. <font color=red>volatile</font>（不稳定的）修饰符，针对<font color=red>多线程情况</font>下出现的。它只保证了共享变量的可见性。在线程 A 修改被 volatile 修饰的共享变量之后，线程 B 能够读取到正确的值。synchronized通常称为重量级锁，volatile更轻量级

# 抽象类与接口
**抽象类** 
特点:
1. 抽象类中的抽象方法，需要有<font color=red>子类实现</font>，如果子类不实现，则子类也需要定义为抽象的。
2. 抽象类中可以存在<font color=red>普通属性，方法</font>，静态属性和方法。 
3. 抽象类中可以存在抽象方法。 
4. 如果一个类中有一个抽象方法，那么当前类一定是抽象类；抽象类中不一定有抽象方法。 
5. 抽象类中可以<font color=red>构造方法</font>。

**接口** 
1. 在接口中只有方法的声明，<font color=red>没有方法体</font>。 
2. 在接口中只有<font color=red>常量</font>，因为定义的变量，在编译的时候都会默认加上 public static final 
3. 在接口中的方法，永远都被public来修饰。 
4. 接口中<font color=red>没有构造方法</font>，也不能实例化接口的对象。 
5. 接口可以实现<font color=red>多继承 </font>。
6. 接口的方法可以不光是抽象<font color=red>abstract</font>，也可以有<font color=red>default</font>、<font color=red>static</font>。
7. 接口中定义的方法都需要有实现类来实现，如果实现类不能实现接口中的所有方法则实现类定义为抽象类。

# 修饰符
**类修饰符：**
- <font color=red>public</font>（访问控制符），将一个类声明为公共类，他可以被任何对象访问，一个程序的主类必须是公共类。
- abstract，将一个类声明为抽象类，没有实现的方法，需要子类提供方法实现。
- <font color=red>final</font>，将一个类生命为最终（即非继承类），表示他不能被其他类继承。
- friendly，默认的修饰符，只有在相同包中的对象才能使用这样的类。对于内部类来说，可以有所有的修饰，因为内部类放在外部类中，与成员变量的地位一致，所以有四种可能。  
**成员变量修饰符：**
- <font color=red>public</font>（公共访问控制符），指定该变量为公共的，他可以被任何对象的方法访问。
- private（私有访问控制符）指定该变量只允许自己的类的方法访问，其他任何类（包括子类）中的方法均不能访问。
- protected（保护访问控制符）指定该变量可以别被自己的类和子类访问。在子类中可以覆盖此变量。
- friendly ，在同一个包中的类可以访问，其他包中的类不能访问。
- <font color=red>final</font>，最终修饰符，指定此变量的值不能变。
- static（静态修饰符）指定变量被所有对象共享，即所有实例都可以使用该变量。变量属于这个类。
- transient代表对象的临时数据，成员数据不能被串行化。
- volatile修饰符，针对多线程情况下出现的。它只保证了共享变量的可见性。在线程 A 修改被 volatile 修饰的共享变量之后，线程 B 能够读取到正确的值。synchronized通常称为重量级锁，volatile更轻量级
**方法修饰符**：
- <font color=red>public</font>（公共控制符）
- private（私有控制符）指定此方法只能有自己类等方法访问，其他的类不能访问（包括子类）
- protected（保护访问控制符）指定该方法可以被它的类和子类进行访问。
- <font color=red>final</font>，指定该方法不能被重载。
- static，指定不需要实例化就可以激活的一个方法。
- synchronize，同步修饰符，在多个线程中，该修饰符用于在运行前，对他所属的方法加锁，以防止其他线程的访问，运行结束后解锁。
- native，本地修饰符。指定此方法的方法体是用其他语言在程序外部编写的。

# 会话跟踪技术
1. URL重写：
   `http://w3cschool.cc/file.htm;sessionid=12345 `
   URL地址重写的原理是将该用户Session的id信息重写 到URL地址中，以便在服务器端进行识别不同的用户。URL重写能够在客户端停用cookies或者不支持cookies的时候仍然能够发挥作用。
2.  隐藏表单域：
   `<input type="hidden" name="sessionid" value="12345">`
   此表单元素并不在客户端显示，浏览时看不到，源代码中有不支持常规的超文本链接，其不会导致表单提交
3. Cookie:
   ![img](http://www.yubajin.cn/img/20190407180944-java_cookie.png ''cookie'') 
   - Cookie是由客户端请求，服务器给客户端发送的一小段文本信息，客户端接受响应后将数据保存在客户端浏览器中；客户端再次访问服务器端时，会携带cookie的数据，服务器就能够识别出客户端了
   - Cookie是可以被禁止的。
4. session：  
   - 保存在服务器端。将数据保存在服务端，服务器压力大，数据安全
   - Session是依赖Cookie的，如果Cookie被禁用，那么session也将失效。

# ThreadLocal
<img src="http://www.yubajin.cn/img/20190407180944-java_threadlocal.jpg" width="60%" heigth="55%" >
1. “ThreadLocal存放的值是线程封闭，线程间互斥的” 
2. “主要用于线程内共享一些数据，避免通过参数来传递”在Thread类中有一个<font color=red>Map</font>，用于存储每一个线程的变量的副本。

# wait和sleep方法
1. wait()方法属于Object类，sleep()属于<font color=red>Thread类</font>
2. 调用wait()方法的时候，线程会放弃对象锁;调用sleep()方法的过程中，线程<font color=red>不会释放对象锁</font>
3. wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用
4. sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常

# static
- 静态变量static在不同的实例中地址一样，储存在全局区
- 而且静态变量或方法是在类<font color=red>被创建的时候加载</font>，并且只会加载一次， static的地址是不会变的，而且是储存在全局区的，毕竟这样才可以共享嘛

# Java程序初始化
1. 初始化顺序
- 父类的静态成员初始化 > 父类的静态代码块 > 子类的静态成员初始化 > 子类的静态代码块 > 父类的代码块 > 父类的构造方法 > 子类的代码块 > 子类的构造方法
2.  静态初始化块
- 无法直接调用静态初始化块
- 在创建第一个实例前或引用任何静态成员之前，将自动调用静态初始化块来初始化
- 静态初始化块的标准写法，没有访问修饰符、参数：
  ```java
  static {
      // 初始化内容
  }
  ```
# HashMap
![img](http://www.yubajin.cn/img/20190407180944-java_hashmap.jpg ''hashmap'') 
1. HashMap是一个用于存储<font color=red>KeyValue键值对</font>的集合，每一个键值对也叫做Entry。这些个键值对分散存储在一个数组中，这个数组就是HashMap的主干
2.  HashMap的<font color=red>初始长度为16}</font>，并且每次自动扩展或手动初始化时，长度必须是2的幂目的：服务于从key映射到index的Hash算法，实现一个尽量均匀分布的Hash函数
3. 得到index主要取决于HashCode值最后几位
4. Hash Resize(扩展长度)：取决于Capacity（当前长度）和LoadFactor（负载因子）,HashMap.size >= Capacity * LoadFactor时，需要扩容
5. 扩容步骤：
- 新建一个Entry空数组，长度为原数组的2倍（<font color=red>Resize</font>）
- 遍历原Entry数组，将所有Entry重新Hash到新数组中(<font color=red>ReHash</font>)
- 注意在高并发时容易形成<font color=red>环形链</font>，进入死循环，故HashMap不是<font color=red>线程安全</font>
![img](http://pnflv3lnc.bkt.clouddn.com/java_hashmap2.png) 
6. 线程安全的集合：HashTable或者Collections.synchronizedMap
7. ConcurrentHashMap：为了兼顾线程安全和运行效率ConcurrentHashMap是一个<font color=red>二级哈希表</font>。在一个总的哈希表下面，有若干个子哈希表
<img src="http://pnflv3lnc.bkt.clouddn.com/java_hashmap3.png" width="60%" heigth="60%">

# HTTP状态码
1. 1xx（临时响应）
- 100:请求者应当继续提出请求
- 101：切换协议
2. 2xx（<font color=red>成功</font>)
- 200（成功）：服务器成功处理请求
- 201（已创建）：请求成功并且创建好了新资源
- 202（已接受）：服务器已接受请求，但尚未处理
- 203（非授权信息）：服务器处理了请求，但没有返回信息可能来自另一来源
- 204（无内容）：服务器处理了请求，但没有返回内容
- 205（重置内容）：服务器处理了请求，但没有返回内容
- 206（部分内容）：，服务器处理了部分GET请求
3. 3xx（<font color=red>重定向</font>）服务器对客户端说：<font color=red>走开</font>,表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。
- 300（多种选择）针对请求，服务器可执行多种操作。 服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。
- 301（永久移动）请求的网页已永久移动到新位置。 
- 302（临时移动）
- 303（查看其他位置）请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。
- 304（未修改）自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
- 305（使用代理）请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。
- 307（临时重定向）服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
4. 4xx（<font color=red>请求错误</font>）服务器对客户端说：<font color=red>你错了</font>,这些状态代码表示请求可能出错，妨碍了服务器的处理。
- 400（错误请求）服务器不理解请求的语法。
- 401（未授权）请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
- 403（禁止）服务器拒绝请求。
- 404（未找到）服务器找不到请求的网页。
- 405（方法禁用）禁用请求中指定的方法。
- 406（不接受）无法使用请求的内容特性响应请求的网页。
- 407（需要代理授权）此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。
- 408（请求超时）服务器等候请求时发生超时。
- 409（冲突）
- 410（已删除）
- 411（需要有效长度）
- 412（未满足前提条件
- 413（请求实体过大）
- 414（请求的 URI 过长）
- 415（不支持的媒体类型）
5. 5xx（服务器错误）服务器对客户端说：<font color=red>我错了</font>，这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。
- 500（服务器内部错误）服务器遇到错误，无法完成请求。
- 501（尚未实施）服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。
- 502（错误网关）服务器作为网关或代理，从上游服务器收到无效响应。\n503（服务不可用）服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。
- 504（网关超时）服务器作为网关或代理，但是没有及时从上游服务器收到请求。
- 505（HTTP 版本不受支持）服务器不支持请求中所用的 HTTP 协议版本。

# Java concurrent包
- Semaphore：一个计数信号量
- ReentrantLock：一个可重入的互斥锁定 Lock，功能类似synchronized，但要强大的多。
- Future：是与Runnable,Callable进行交互的<font color=red>接口</font>，比如一个线程执行结束后取返回的结果等等，还提供了cancel终止线程。
- CountDownLatch：一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。 

# Java线程池
## 线程
![img](http://www.yubajin.cn/img/20190407180944-java_xiancheng.jpg ''xiancheng'') 
## 多线程实现
### 通过继承Thread类 
1. 继承Thread类
2. 重写Thread的run()方法 
3. 新建一个进程类
4. 调用start方法
```java
class Thread1 extends Thread{      
    private String name; 
    private int num = 10;
    
    publicMyThread(String name){  
        this.name =name;  
    }  

    public void run(){  
        for(int i = 0; i < 500; i++){  
            if(this.num > 0){  
                System.out.println(this.name+"---->"+(this.num--));  
            }  
        }  
    }  
} 
public class ThreadDemo1 {  
    publicstaticvoidmain(String[] args) {  
        Thread1 mt1= new Thread1("xiaoming"); 
        Thread1 mt2= new Thread1("xiaohua"); 
        Thread1 mt3= new Thread1("xiaoli"); 

        mt1.start();  
        mt2.start();  
        mt3.start();  
    }  
}  
```
### 通过继承Thread类
1. 实现Runnable类
2. 重写Thread的run()方法 
3. 新建一个进程类
4. 调用start方法
```java
class Thread2 implements Runnable{  

      private int num =10;  
      public void run(){  
            for(int i = 0; i<500; i++){ 
                  if(this.num>0){
                             System.out.println(Thread.currentThread().getName() +(this.ticket));  
                  }  
            }  
      }  
} 

public class RunnableDemo2 {  
      public static void main(String[] args) {  
          
            // 设计三个线程 
            Thread2 mt = new Thread2();  
            Thread t1 = new Thread(mt, "xiaoming"); 
            Thread t2 = new Thread(mt, "xiaohua"); 
            Thread t3 = new Thread(mt, "xiaohong"); 
          
            t1.start();  
            t2.start();  
            t3.start();  
      }  
}  
```

# 数组复制方法
- for循环的话，很灵活，但是代码不够简洁
- System.arraycopy()源码。可以看到是<font color=red>native方法</font>：native关键字说明其修饰的方法是一个原生态方法，方法对应的实现不是在当前文件，而是在用其他语言（如C和C++）实现的文件中。
- copyOf不是System的方法，而是Arrays的方法，下面是源码，可以看到本质上是<font color=red>调用的arraycopy</font>方法。
-  Clone，返回的是<font color=red>Object【】</font>，需要强制转换。 一般用clone效率是最差的

# java.lang包中不能被继承的类
- public final class Double
- public final class Float
- public final class Integer
- public final class Long
- public final class Math 
- public final class Byte
- public final class Short 
- public final class String 
- public final class Void 
- public final class System 
- public final class StrictMath
- public final class StringBuffer
- public final class StringBuilder
- public final class Character
- public final class ProcessBuilder
- public final class RuntimePermission
- public final class StackTraceElement
- public static final class Character.UnicodeBlock
- public final class Class<T>
- public final class Compile

# HttpServlet容器响应Web客户请求流程如下：
1. Web客户向Servlet容器发出Http请求；
2. Servlet容器解析Web客户的Http请求；
3. Servlet容器创建一个<font color=red>HttpRequest</font>对象，在这个对象中封装Http请求信息；
4. Servlet容器创建一个<font color=red>HttpResponse</font>对象；
5. Servlet容器调用HttpServlet的<font color=red>service</font>方法，这个方法中会根据request的Method来判断具体是执行doGet还是doPost，把HttpRequest和HttpResponse对象作为service方法的参数传给HttpServlet对象；
6. HttpServlet调用HttpRequest的有关方法，获取HTTP请求信息；
7. HttpServlet调用HttpResponse的有关方法，生成响应数据；
8. Servlet容器把HttpServlet的响应结果传给Web客户。

# java并发
1. CopyOnWriteArrayList适合使用在<font color=red>读操作远远大于写操作</font>的场景里，比如缓存。<font color=red>不能用于实时读</font>的场景线程并发的读，则分几种情况： 
- 如果写操作未完成，那么直接读取原数组的数据； 
- 如果写操作完成，但是引用还未指向新数组，那么也是读取原数组数据； 
- 如果写操作完成，并且引用已经指向了新的数组，那么直接从新数组中读取数据。
2. ReadWriteLock 当写操作时，其他线程无法读取或写入数据，而当读操作时，其它线程无法写入数据，但却可以读取数据 。适用于<font color=red>读取远远大于写入</font>的操作。
3. ConcurrentHashMap是一个<font color=red>线程安全的Hash Table</font>，它的主要功能是提供了一组和HashTable功能相同但是线程安全的方法。ConcurrentHashMap可以做到读取数据不加锁，并且其内部的结构可以让其在进行写操作的时候能够将锁的粒度保持地尽量地小，不用对整个ConcurrentHashMap加锁。
4. volatile只能保证变量的安全，不能保证线程的安全

# java中的数据类型分类：
- 基本数据类型（或叫做<font color=red>原生类</font>、内置类型）8种：
  - 整数：byte，short，int，long（默认是int类型）
  - 浮点类型： float，double（默认是double类型）
  - 字符类型：char
  - 布尔类型：boolean
- <font color=red>引用数据</font>类型3种：<font color=red>数组</font>，类，接口

# Java中的关键字
1. 48个关键字：abstract、assert、boolean、break、byte、case、catch、char、class、continue、default、do、double、else、enum、extends、final、finally、float、for、if、implements、import、int、interface、instanceof、long、native、new、package、private、protected、public、return、short、static、strictfp、super、switch、synchronized、this、throw、throws、transient、try、void、volatile、while。
2. 二个保留字（现在没用以后可能用到作为关键字）：goto、const。
3. 个特殊直接量：<font color=red>true</font>、<font color=red>false</font>、<font color=red>null</font>。 （不是关键字）

# hashcode和equals的关系
1. equals()相等的两个对象他们的hashCode()肯定相等，也就是用equals()对比是绝对可靠的。
2. hashCode()相等的两个对象他们的equal()不一定相等，也就是hashCode()不是绝对可靠的。

# DBMS
原-事 一-完 隔-并 持-恢
原子性：（由DBMS的<font color=red>事务管理</font>子系统来实现）； 
一致性：（由DBMS的<font color=red>完整性</font>子系统执行测试任务）； 
隔离性: （由DBMS的<font color=red>并发控制</font>子系统实现）； 
持久性:（由DBMS的<font color=red>恢复管理</font>子系统实现的）

# final **修饰符**
1. public protect private 是<font color=red>访问控制符</font>
2. final 变量：
- final 变量能被显式地初始化并且只能初始化一次。可以在不是必须要在定义的同时完成初始化
- final <font color=red>对象的引用</font>不能改变，但是里面的值可以改变。
- final 修饰符通常和 static 修饰符一起使用来创建类常量。
3. final 方法
- 类中的 final 方法可以被子类继承，但是不能被子类修改。
4. final 类
- final 类不能被继承，没有类能够继承 final 类的任何特性。

# 继承Collection接口
​	![img](http://www.yubajin.cn/img/20190407180944-java_collection.jpg ''collection'')

# lambda表达式
```java
(parameters) ->{statements;} 
```
**可选类型声明：**不需要声明参数类型，编译器可以统一识别参数值。
**可选的参数圆括号：**一个参数无需定义圆括号，但多个参数需要定义圆括号。
**可选的大括号：**如果主体包含了一个语句，就不需要使用大括号。
**可选的返回关键字：**如果主体只有一个表达式返回值则编译器会自动返回值，<font color=red>大括号</font>需要指定明表达式返回了一个数值。

# JDBC statement
## Statement
- 普通的不带参的查询SQL
- 每次执行sql语句，数据库都要执行sql语句的编译 ，  
- 最好用于仅执行一次查询并返回结果的情形，<font color=red>效率高</font>于PreparedStatement

## PreparedStatement
- 是Statement的子接口, 是<font color=red>预编译</font>的,可以传入带<font color=red>占位符</font>的SQL语句
- 形式：String sql="insert into examstudent values(?,?,?,?,?,?,?)";
  - 可以防止SQL注入
  - 提高代码的可读性和可维护性;
  - 可变参数的SQL,编译一次,执行多次,效率高;

# 重载和重写
## 重写
- 两同：方法名、形参列表
- 两小：返回值类型比父类更小或相等、异常比父类方法更小或相等
- 一大：<font color=red>子类权限比父类大</font>或相等

## 重载
- 两同：同一个类，同一个方法名
- 三不同：参数列表不同。（类型，个数，顺序不同）