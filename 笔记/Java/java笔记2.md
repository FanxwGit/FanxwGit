

# 异常

## 异常分类

1. 运行时的异常RuntimeException
可以不处理，交给虚拟机处理。

2. 编译的异常Exception
必须要处理。

## 异常产生的过程

1. JVM会根据产生的原因创建一个异常对象，异常对象包括了（**内容，原因，位置** 

2. 但是在方法之中没有try-catch，JVM会把对象给调用此方法的main去处理

3. main方收到对象，但是main方法没有异常的处理逻辑，所以会继续把对象抛出给main方法的调用者JVM来处理。

## throws关键字

### 作用:

1. 当方法内部出现异常的时候，用throws丢出给方法的调用着处理（自己不处理），最终是交给JVM处理——中断处理 

### 注意:

1. throws必须写在方法的声明处
2. 抛出的异常是Exception或者是其子类
3. 如果丢出了，我们必须处理，用try处理，如果无就给JVM中断处理。
4. 如果抛出了编译异常，必须处理这个异常，也可以在方法体后面再次声明，再次抛出，最后抛到主函数（主函数也能继续抛出）。

## try-catch  

1. catch(异常类)里面接收的参数，子类一定要父类的最后，最后一个用Exception异常来捕捉其他异常。

## 3个处理异常的方法

1. String  getMessage() 返回此throwable的简单描述

2. String toString() 返回详细的字符串

3. void printStackTrace () //堆栈跟踪，JVM打印异常对象，最为全面

## finally代码块

1. 无论是否异常都会执行

2. 因为性质1，finally一般用于资源的释放和回收。

##继承时父子抛出异常注意
父类抛出了多个异常，子类重写父类方法时候，抛出的异常必须小于等于父类（不能比父类大，可以不抛出）

### 情况：

1. 子类重写父类的方法时候，抛出的异常相同。

2. 子类重写父类的方法时候，抛出父类的异常的子类。

3. 父类不抛出异常，子类也不能抛出异常

## 自定义异常

注意：
1. 一般以xxException结尾，说明是一个异常类
2. 必须继承**Exception**或者 **RuntimeException**
继承**Exception** 那么就是编译的异常，必须处理，要么throws，要么try...catch
继承**RuntimeException**，是运行期异常，交给JVM中断处理。

# 多线程
## 并发与并行：

* 并发：交替执行，一个人吃两个馒头
* 并行：同时发生，两个人吃两个馒头
##进程与线程
* 进程：硬盘进入到内存的程序（内存：临时存储RAM）
* 线程：属于进程，是进程之中的执行单元，一个进程之中至少有一个线程。
![](https://img2020.cnblogs.com/blog/2212500/202012/2212500-20201230231810519-178876058.png)

## 线程调度

* 分时调度
所有线程轮流使用，时间平均

* 抢占式调度
优先级高，cup用的时间多。多线程不能提高程序的运行速度，但是可以提高程序的运行效率，让cpu使用的效率更高。

## 主线程

> JVM会执行main方法，main方法会进入栈内存，JAVM会找操作系统开辟一条main方法通向cpu的执行路径，cpu可以通过这个路径执行main方法，这个路径有个名字就叫main（主线程）线程

总结：就是栈内存和cpu之间的路线

## Thread

1. 调用Thread的run方法，并且此线程不能重复执行。



## 多线程的原理 

方法：在栈里执行
new对象：在堆里

多线程：每次新线程的增加就会开辟一个新的栈空间，执行run方法，对于cpu可以随便执行任意一个方法。

内存原理：
![](https://img2020.cnblogs.com/blog/2212500/202012/2212500-20201231001746412-814394460.png)

## Thread常用的方法

1. 设置线程名称：setName 或者 父类构造
2. 获取线程名称getName
3. 返回一个线程的 静态方法currentThread
4. start
5. sleep（ms）暂停线程，其本身就有InterruptedException，用try或者 throw处理

## Runnable接口实现方法

1. 实现Runnable 接口 重写Run方法
2. 在主线程创建Runnable的实现类
3. 将Runnable作为参数传到Thread类里面
4. 用Thread的类的run方法调用Runnable 

## Runnable创建多线程接口的好处（尽量使用Runnable）

1. 避免单继承的局限性
2. 增加了程序的**拓展性**，降低了程序的**耦合性**，它把设置线程任务和开启新的线程进行了分离，实现类之中，我们重写了run方法设置线程任务，创建Thread对象，调用start的方法调用。**传递不同的实现类，实现不同的需求**

# 线程安全

## 概念
&nbsp;&nbsp;&nbsp;&nbsp;多线程访问了**共享的数据**，会产生线程的安全问题。比如三个人在一起卖票，就会出现负数票的情况

### 同步代码块

* **同步代码块**：`synchronized`关键字可以用于方法中的某个区块之中，表示只对这个区块的资源实行互斥访问，当有线程已经在执行这块代码了，其他线程会进行等待。
```
synchronized(同步锁){
可能出现线程安全问题的代码
}
```
* **原理：**多个线程一起抢cpu的执行权，谁抢到了就会执行run方法，当遇到synchronized代码块这时会检查synchronized代码块是否有锁对象，有的话，就获取锁对象进入代码块。当其他线程运行时，发现没有锁对象，因此会进入阻塞状态，直到其他线程归还锁对象。

* **总结**：
1.同步中**没有执行完毕不会释放**锁对象，**没有锁对象进不来**。
2.此外，锁对象一定要唯一，但是程序频繁判断锁，程序效率会有所降低。

###同步方法
* **同步方法**：将同步关键字加在函数声明上，直接调用即可。
* **原理**：实际上也是同步代码快，代码块的锁对象就是this（对象）本身。

### 静态同步方法
&nbsp;&nbsp;&nbsp;&nbsp;唯一值得一提的是，锁对象不是this，因为this是创建对象的时候生成的，而静态方法优先于对象，**锁对象是**本类的class属性——class文件对象（反射）,实际上就是。

```
synchronized(xxx类.class)
{

}
```
&nbsp;&nbsp;&nbsp;&nbsp;**使用方法**：和同步方法一样

## **Lock锁**

* Lock锁：Lock实现提供比使用synchronized方法和语句可以获得的更广泛的锁定操作

* 使用方法
1. 利用多态写法，创建Lock的实现类ReentrantLock。
2. 在可能出现安全问题的代码之前加lock();
3. 代码结束之后用unlock();
* 注意
1. 在try语句的里，将unlock放在finally里,这样无论程序是否都出现异常都可以把锁释放。

## 线程状态的概述

| 线程状态      | 导致状态发生的条件                                           |
| ------------- | ------------------------------------------------------------ |
| NEW           | 刚被创建等待start                                            |
| Runnable      | 可以在jvm运行的状态，不一定正在运行，取决操作系统处理器      |
| Waiting       | 无线等待，需要手动启动                                       |
| Timed Waiting | 计时醒来                                                     |
| Teminated     | run方法正常退出而死亡，或者是没有捕获异常JVM终止了run方法而死亡 |





## 线程池

1. 利用Executors静态方法获取ExecutorService实现类创建线程个数
2. 利用submit添加线程
3. shutDown 结束线程

* 注意 ：
    没有shutDown**销毁线程**，程序就不会自己结束，他会一直等待新的线程添加。
    
* 线程池使用方法如下：
  
    ```java
    ExecutorService es= Executors.newFixedThreadPool(5);//创建线程个数
    es.submit(new RunnableImpl());//添加线程进去 然后直接执行
    es.submit(new RunnableImpl());
    //如果没有销毁 程序不会结束
    es.shutdown();
    ```



# Lambda表达式

## 介绍

* Lambda表达式：可以用来代替匿名内部类，在参数里完成接口的实现。
* 使用条件：接口的需要实现的函数**有且仅有**一个。
* 语法规则：(参数列表)->{方法体}

## 不同情况示例

* 函数有参

```java
        arr.sort((Person o1,Person o2)->{
                    return o1.age - o2.age;
                });
```

* 函数无参

```
        new Thread( ()->{
            System.out.println("无参Lambda");
        }).start();
```

## Lambda的省略形式

* 参数类型可省

```
       Method(1,2,(a,b)->{
            System.out.println(a+b);// 1 2 是 a 和 b ， 1 和2  默认是传进去就是int
        });
```

* 省略括号(void方法体只有一行)

```
        Method(1,2,(a,b)->System.out.println(a+b));
```

* 省略括号(非void方法体只有一行，缩写必须是返回值)

```java
        arr.sort((o1,o2)->o1.age-o2.age);//简化版Lambda表达式
```



## 总结

1. Lambda格式固定 三段式 ()->{}
2. 省略括号只能写**一行**，并且如果是有**返回值函数**会将其的值作为返回值。



# File类

## File类的静态方法

1. pathSeparator 系统默认路径分隔符,windows 封号，linux 冒号。

2. separator系统默认文名称分隔符

值得一提的是，在不同的系统下，文件名称分隔符是不同的，比如`C:\Windows\123` 需要改写成`"C\:windows"+File.separator+"123"` ，这样就在不同的系统下正常运作了。



## 绝对路径与相对路径

* 绝对路径：以盘符开始的路径
* 相对路径：默认是当前项目的根目录



## File类的构造方法

`File(String pathname)`

参数注意：

* pathname 字符串的路径名称
* 路径可以是文件/文件夹结尾
* 路径可以是不存在的路径
* 

`File(String parent ,String child )`父路径和子路径

* 父路径和子路经可以单独书写，使用起来更加灵活。

`File("C:\\","apple.tXt")= "C:\\apple.txt"`



`File(File parent ,String child )`

* 路径更加灵活
* 父路径是File路径，可以使用File类的方法对路径进行操作，再用路径创新对象，直接放入就是合并。



## 常用方法

**获得：**

* `public String getAbsolutePath() `：获取绝对路径，
* `public String getPath()`：获取绝对/相对路径，**实际上重写toString就是调用了这个方法**
* `public String getName()`：获取末尾部分文件/文件夹的名称
* `public long length()`此File表示的文件长度，获取的是构造方法指定文件的大小，以字节为长度，
* 注意：
  1. 如果此文件不存在，那么length返回0。
  2. 文件夹是没有大小的概念的，不能获取文件夹的大小。
* `public Files[] listFiles()`
  * `public Files[] listFiles(FileFliter)`获取此目录内的所有文件，如果此File是文件，会抛出空指针异常。



**判断：**

* `pbulic boolean exists()`表示文件是否真实存在
* `public boolean isDirectory()`表示是否为目录
* `public boolean isFile()`表示是否为目录，与上面互斥，并且两个方法如果路径不存在，那么会返回false 



**操作：**

* `public boolean createNewFile()`：会先去找文件，如果文件不存在就会创建文件，如果路径不正确就会抛出异常，路径不正确和文件已经存在就会返回false，反之返回true。
* `public boolean makdir()`：创建文件夹
* `public boolean makdirs()`：创建多级文件夹，路径可以不存在，不存在就会自动创建多个文件夹。
* `public boolean delete()`：删除文件或者目录。如果删除的是目录，**则目录为空才能删除。**

```

public class FileCreateDelete {
    public static void main(String[] args) throws IOException {
        File f = new File("aaa.txt");
        System.out.println("是否存在:"+f.exists()); // false
        System.out.println("是否创建:"+f.createNewFile()); // true
        System.out.println("是否存在:"+f.exists()); // true
        
        // 目录的创建
        File f2= new File("newDir");
        System.out.println("是否存在:"+f2.exists());// false
        System.out.println("是否创建:"+f2.mkdir()); // true
        System.out.println("是否存在:"+f2.exists());// true// 创建多级目录
        File f3= new File("newDira\\newDirb");
        System.out.println(f3.mkdir());// false
        
        File f4= new File("newDira\\newDirb");
        System.out.println(f4.mkdirs());// true// 文件的删除
        System.out.println(f.delete());// true// 目录的删除
        System.out.println(f2.delete());// true
        System.out.println(f4.delete());// false
    }
}

```

## 综合案例

### **文件搜索：**

* 递归实现

  `public File `

```java
    //如果是文件夹 就输出里面的所有文件 如果里面的文件是目录就继续递归
    public  static void show01(File f)  {
        if(f.isFile()){
            if(f.getName().endsWith(".java"))
                System.out.println(f.getName());
        }
        else{
            File[] r = f.listFiles();
            for(var i:r){
                show01(i);
            }
        }
    }
```



* 文件过滤器`FileFilter`

`java.io.FileFilter`： 是一个接口，是File的过滤器。 该接口的对象可以传递给File类的 listFiles(FileFilter) 作为参数， 接口中只有一个方法。



`boolean accept(File pathname) `：测试pathname是否应该包含在当前File目录中，符合则返回true。



* 文件过滤器接口的使用

```
 File[] r = f.listFiles(new FileFilter() {
                @Override
                public boolean accept(File pathname) {
                    //如果是文件夹或者是我们想要的文件 那么就放进去
                    return pathname.isDirectory() || pathname.getName().endsWith(".java");
                }
            });
```



* 文件过滤器Lambda表达式实现

```
 File[] r = f.listFiles(file->file.isDirectory()|| file.getName().endsWith(".java"));
```

简化规则：

* **只有一个方法**需要重写
* **只有一行return**，return可以不写，但同时要把括号去掉
* 参数类型可以不写
* **参数只有一个的时候**，参数的括号也可以不写

(标准形式):  (参数)->{}  =>  (简化形式) 参数->返回值



# IO流



## 字节流

Java中I/O操作主要是指使用`java.io` 包下的内容，进行输入、输出操作。输入也叫做读取数据，输出也叫做作写 出数据。



### OutputStream

* OutputStream：此抽象类是所有输出字节流的超类。

* 常用方法：

  ​	``



### FileOutputStream

FileOutputStream：文件字节输出流

**作用：**内存数据写入到硬盘的文件中

**构造方法：**

* `FileOutputStream(String name)`

* `FileOutputStream(File name)`

  参数：写入数据的目的地

  ​	File：目的地是一个文件

  ​	String：目的地是文件路径

字节输出流使用的步骤：

1. 创建FileOutputStream对象，构造方法中传递写入数据的目的地
2. 调用write(byte[])把数据写入到文件中
3. 释放资源



**追加写数据：**

* `FileOutputStream(String name,boolean append)`
* `FileOutputStream(File name，boolean append)`

append为true就是代表不新建文件，即可以追加写数据。

**换行：**

* windows ：\r\n

* linux：/n

* amac：/r

  `write("\r\n".getbytes())`



## 字节输入流

### InputStream字节输入流 

* InputStream，此抽象类为所有输入流的超类

* **方法：**

  

### FileInputStream文件字节输入流

#### FileInputStream

* **作用：**

  硬盘中的文件读取到内存中



* **构造方法：**

  `FileInputStream(String name)`

  `FileInputStream(File file)`



#### 字节输入流的原理（硬盘->内存)

java程序->JVM->OS->OS调用读取数据的方法->读取文件到内存



#### 方法

* `int read()`

  读取文件中的一个字节并返回，读取到文件末尾返回-1

  **循环读取：**

  ```java
  while((le=file.read())!=-1)
  ```

* `int read(bytes[])`

  ```
  while((le=file.read(bytes))!=-1){
  	String s=new String(bytes,0,le)//这样可以避免放入过多的空格
  }
  ```

#### 

### 字节流练习——文件复制

```
        FileInputStream file=new FileInputStream("img\\ji.jpg");
        FileOutputStream file2=new FileOutputStream("img\\jiCopy.jpg",true);
        byte []bytes=new byte[1024];
        int le=0;
        while((le=file.read(bytes))!=-1){
            file2.write(bytes,0,le);
        }
        System.out.println("复制成功");
        file.close();
        file2.close();
```

思路：复制A文件到B，边读取边写即可。**注意关闭流，释放资源。**



## 字符流

### Reader

* 字符输入流，是字符输入流最顶层的超类，是一个抽象类。
* 常用方法
  * `read(char[])`
  * `read()`//读取一个字符并返

### FileReader字符输入流

* 作用：读取文件的字符流 

* 构造方法：
  * `FileReader(String fileName)`
  * `FileReader(File file)`

* 优点：相比于字节流，能够更好的**读取中文**，byte存储的范围是0~127 而字符char能够存储2个字节，能够存储0~65535，比如一个中文的字符编码为263，那么在byte显示下是负数，因此单个字节无法读取中文。

### Writer字符输出流

* Witer是最顶层的超类，是抽象，

**方法：**

* `write(String s)`
* `write(char[] buf)`
* `write(int i)`写入单个字符



### FileWriter文件字符输出流

* 使用方法：

  构造，用write写入缓冲区，最后用flash/close，数据从内存中写到硬盘之中。如果char数组大于8186，那么就会自动写入硬盘之中，所以不用flash，最后记得close即可。

* **利用try catch处理字符输出流的异常**

  在日常开发之中我们需要声明字符输出流，和close，但是我们不能总是抛出异常让jvm处理异常。下面分别是

  1. 普通处理
  2. JDK7特性
  3. JDK9特性处理方式

  ```
  FileWriter f = null;//首先给它附上初始值
  try{
      f=new FileWriter("123.txt");
      f.write("123");
  } catch (IOException e ){
      System.out.println(e);
  }finally{//在finally用来资源释放
      if(null!=f) {//解决f可能是空指针的情况
          try {
              f.close();//close还有可能有异常，需要再次处理
          } catch (Exception e) {
              System.out.println(e);
          }
      }
  }
  
  //JDK7 优化 在try()里面声明流对象，出了try的作用域会自动释放掉
  try(FileWriter file=new FileWriter("123.txt")){
  }catch (IOException e ){
      System.out.println(e);
  }
  
  
  //JDK9优化 可以把外面申明的流放进
  
  FileWriter file1=new FileWriter("123.txt");
  FileWriter file2=new FileWriter("123.txt");
  try(file1;file2){
      file1.write("12");
      file2.write("12");
  }catch (IOException e ){
      System.out.println(e);
  }
  ```

## Properties属性集

* Properties：是唯一一个和IO流相结合的集合。可以读取文件到缓冲区生成集合set，也可以将属性集合里面的东西读取到文件里，在文件里#表示注释，不会被读取，key值，和value值之间可以用任意符号进行分割，下面是常用方法。

* 方法:

  `public void load(Reader)`:载入文件

  `public void wtore(Wirter)`存储文件

  `public Set<String> stringPropertyNam()`将属性集里面的key集合存储到`Set<String>`里面





##  	转换流

### 字符编码和字符集

* 字符编码：自然语言和二进制的转换规则
* 字符集：有很多处理自然语言规则的集合







## 缓冲流

缓冲流，也叫高校流，给流增加一个数组，增加基本流的效率。

* 字节缓冲流：`BufferedInput/OutStream `
* 字符缓冲流：`BufferedWriter/Reader`

使用方法基本一样

### 利用缓冲流对文本序号进行排序

需求：现有乱序文本，每行前面有文本的编号，现需排序。

思路：ReadLine()读取文本，利用分割符对.进行分割，将序号存储到hashmap里面，会自动排好序。

**代码：**

```
    public static void main(String[] args) throws IOException{
        BufferedReader reader=new BufferedReader(new FileReader("123.txt"));
        BufferedWriter writer=new BufferedWriter(new FileWriter("123Copy.txt"));
        String s;
        HashMap<String,String> map =new HashMap<>();
        while((s=reader.readLine())!=null){//不断读取一行
            String str[]=s.split("\\.");//分隔符\\进行转义
            map.put(str[0],str[1]);
        }
        for(String i:map.keySet()){//获取哈希map的key的集合
           writer.write(i+"."+map.get(i)+"\r\n");
        }
        writer.close();
        reader.close();
    }
```

上有几个重要的点

1. `reader.readLine()!=null `不断获取每一行代码
2. split对一些字符的分割，需要加两个反斜杠
3. 用增强for循环遍历map，需要用`map.KetSet()`获取键值的集合，再通过键值去依次访问里面的值。

## 字符编码





#  网络编程

## 基础入门

### 软件结构

* C/S结构  Client/Server
* B/S结构  Browser/Server

### 网络通信协议

* **网络通信协议：**计算机在网络通信之中需要遵守的一定的规则
* **TCP/IP协议：**传输控制协议/因特网互联协议，结构为4层的分层模型。
  1. 应用层：应用程序的协议
  2. 传输层：网络通信
  3. 网络层：TCP/IP核心层，用来分组和发送。
  4. 数据结构层+物理层：底层网络

## 网络通信协议的分类

* **UDP：**用户数据汇报协议，UDP是无连接通信协议，就是不用确定连接。特点是**耗资小，通信效率高**。但是**偶尔会丢一两个数据包**。
* **TCP：**传输控制协议，需要先建立**连接**。特点是，**可靠无差错，每次链接都需要进行三次握手，效率没UDP搞**
  * 三次握手
    * 客户端喊服务器
    * 服务器回应客户端
    * 客户端丢绳子，确认连接。

## 网络编程三要素

### 协议：

* 协议：规则

### IP地址

* 互联网协议地址，可以理解成电话号码。

#### IP地址的分类

* IPv4:32位的二进制，表示为a.b.c.d形式。
* IPv6:128位地址长度

### 端口号

* 软件自带的一个数字，A计算机上的软件QQ，带着ip+端口号(3500) `192.0.0.1:3500`通过网络到另外一个计算机，另外一个接受了以后，就能用根据端口号从而用QQ来接受这个信息。



## TCP通信程序



### Clinet与Server通信

* `Socket/SocketServer`

步骤如下：

* **服务端：**

  1. 服务器端口创建SocketServer，并指定接收的客户端的端口号。

  2. 用accept进行接收，**等待**客户端传信号。

  3. 接收成功以后，获取套接字，通过套接字获取输入流，输出其内容即可。

* **客户端:** 

  	1. 创建套接字，发送信号，如果没有服务器接收，会抛出**Connection refused** 的异常。
  	2. 服务器接收信号，经过三次握手建立连接，获取套接字的输出流，write信息，然后开始等待。
  	3. 获取服务器发送回来的套接字的输入流，获取信息。




### TCP文件上传案例

* **原理：**:客户端读取本地的文件，文件上传到服务器，服务器把上传的文件保存到服务器的硬盘上面。







# 反射

![image-20210127224534557](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210127224534557.png)

* 反射：将各个组成部分封装成其他对象，





### 获取到class对象的方式：

1. `Class.forName("全类名")`
2. `类名.class()` 通过类名获取
3. `对象.getClass()`通过对象来获取

