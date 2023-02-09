# 网络知识入门

Java 将和网络 IO 有关的接口放在 java.net 包下。

学习 Java 网络编程需要掌握 IO 知识。

进行网络编程前必须知道对方的 IP 地址，否则无法发送信息给对方。

网络编程同时还需要知道服务监听的端口，访问者将请求发送到这个端口，服务监听到后提供对应服务。

# InetAddress 类

> JavaSE 提供的用于获取 **IP** 或者**域名/主机名**的类，位于 java.net 包下（与网络相关的类或接口都在该包下），该类提供了数个常用的静态方法用于获取本机IP、目标域名IP等。

常用静态方法：

|       方法名        |   返回值    |                       作用                        |
| :-----------------: | :---------: | :-----------------------------------------------: |
|   getLocalHost()    | InetAddress |   获取本机的InetAddress对象，包含主机名和IP地址   |
| getByName("主机名") | InetAddress | 根据主机名获取InetAddress对象，包含主机名和IP地址 |
|  getByName("域名")  | InetAddress |   根据域名获取InetAddress对象，包含域名/IP地址    |

InetAddress 对象常用的方法：

|      方法名      | 返回值 |                作用                |
| :--------------: | :----: | :--------------------------------: |
| getHostAddress() | String |   获取InetAddress对象中的IP地址    |
|  getHostName()   | String | 获取InetAddress对象中的主机名/域名 |

# Socket

> Socket，套接字。通过 Socket 传输数据需要发送者和接收者，发送者把数据发送到接收者的某个端口上，接收者持续监听这个端口，从而获得发送者发送的数据。

## TCP

### 字节流编程

> 通过字节流从客户端向服务端发送数据。

#### 客户端

```java
@Test
public void SocketTCPClient1() throws IOException {
    //1、连接服务器（ip、端口）
    //  连接本机的9999端口，若连接成功，返回Socket对象
    Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
    //2、通过socket.getOutputStream()获取输出流
    OutputStream outputStream = socket.getOutputStream();
    //3、通过输出流将数据写入数据通道
    outputStream.write("hello how are you".getBytes());
    //设置写入结束标记[不加此代码不结束写入状态]
    socket.shutdownOutput();
    //4、关闭流对象和socket
    outputStream.close();
    socket.close();
}
```

#### 服务端

```java
@Test
public void SocketTCPServer1() throws IOException {
    //1、监听本机的9999端口，并等待连接
    //  细节: 该端口没有其它服务监听或使用(否则会报错)
    //  细节: ServerSocket可以通过 accept()返回不同的Socket[每次接收都会封装成一个Socket]
    ServerSocket serverSocket = new ServerSocket(9999);
    //2、当没有客户端连接该端口时，程序会阻塞，等待连接
    //  当有客户端连接时，则会返回Socket对象，程序继续执行
    Socket socket = serverSocket.accept();
    //3、通过socket.getInputStream() 读取客户端写入到数据通道的数据
    InputStream inputStream = socket.getInputStream();
    //4、通过IO读取
    byte[] buf = new byte[1024];
    int readLen = 0;
    while ((readLen = inputStream.read(buf)) != -1){
        System.out.println(new String(buf, 0, readLen));
    }
    //5、关闭连接
    inputStream.close();
    socket.close();
    serverSocket.close();
}
```



### 字符流编程

> 通过字符流从客户端向服务端发送数据。

#### 客户端

```java
@Test
public void SocketTCPClient1() throws IOException {
    //1、连接服务器（ip、端口）
    //  连接本机的9999端口，若连接成功，返回Socket对象
    Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
    //2、通过socket.getOutputStream()获取输出流
    OutputStream outputStream = socket.getOutputStream();
    //3、通过字节流获取字符流
    BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
    bufferedWriter.write("hello server 字符流");
    //插入一个换行符，表示写入的内容结束，注意，要求对方读入时使用readLine()进行读入
    bufferedWriter.newLine();
    //如果使用字符流，必须加上该行，手动将数据刷新到数据通道中
    bufferedWriter.flush();
    //4、关闭流对象和socket
    outputStream.close();
    socket.close();
}
```

#### 服务端

```java
@Test
public void SocketTCPServer1() throws IOException, InterruptedException {
    //1、监听本机的9999端口，并等待连接
    //  细节: 该端口没有其它服务监听或使用(否则会报错)
    //  细节: ServerSocket可以通过 accept()返回不同的Socket[每次接收都会封装成一个Socket]
    ServerSocket serverSocket = new ServerSocket(9999);
    //2、当没有客户端连接该端口时，程序会阻塞，等待连接
    //  当有客户端连接时，则会返回Socket对象，程序继续执行
    Socket socket = serverSocket.accept();
    //3、通过socket.getInputStream() 读取客户端写入到数据通道的数据
    InputStream inputStream = socket.getInputStream();
    //4、通过IO读取，将字节流转换为字符流
    BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
    String s = bufferedReader.readLine();
    System.out.println(s);

    //5、关闭连接[处理流直接关闭外层流即可]
    bufferedReader.close();
    socket.close();
    serverSocket.close();
}
```

### 网络文件上传

#### 客户端

```java

```

#### 服务端

```java

```



## UDP