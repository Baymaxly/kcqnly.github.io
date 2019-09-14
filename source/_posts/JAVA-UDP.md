---
title: JAVA UDP编程 
thumbnail: /gallery/wuhan.jpg
date: 2019-08-24 19:24:01
tags: JAVA
categories:
- JAVA
---

## UDP:无连接无状态的通信协议
- 发送方发送数据，如果接收方刚好在原地，则可以接收数据。如果不在，那么信息将丢失
- 发送方无法得知是否发送成功
- 简单，节省，经济

<!--more-->
## 相关类
### DatagramSocket:通讯的数据管道
- send和receive方法
- 绑定一个IP和Port

### DatagramPacket
- 集装箱：封装数据
- 地址：目的地IP+Port

## 代码实现
### 一、实现单方面发送和接收
#### 发送端
```java
import java.net.*;
import java.util.Scanner;

public class UdpSend {
    public static void main(String[] args) throws Exception {
        DatagramSocket ds = new DatagramSocket();
        Scanner in = new Scanner(System.in);
        while(true) {
            String str = in.nextLine();
            DatagramPacket dp = new DatagramPacket(str.getBytes(), str.getBytes().length,
                    InetAddress.getByName("127.0.0.1"), 3000);
             /*如果用str.length(),会出现中文乱码的问题。
        将数据装到DatagramPacket时,英文字符是一个占一个字节,
        而一个中文字符是占据两个字节,所以发送数据的长度如果用sendStr.length()计算,
        英文字符不会出错,计算中文字符就会少计算了要发送的总字节数,
        造成UDP服务端接收中文乱码*/
            ds.send(dp);
            System.out.println("发送成功");
            if(str.equals("bye"))
                return ;
        }

    }
}

```




#### 接收端
```java
import java.net.*;

public class UdpRecv {
    public static void main(String[] args) throws Exception {
        DatagramSocket ds = new DatagramSocket(3000);
        byte[] buf = new byte[1024];
        DatagramPacket dp = new DatagramPacket(buf, 1024);
        String strRecv;
        do {
            System.out.println("UdpRecv: 我在等待信息");
            ds.receive(dp);
            System.out.println("UdpRecv: 我接收到信息");
            strRecv = new String(dp.getData(), 0, dp.getLength());
            System.out.println(strRecv);
        }while(!strRecv.equals("bye"));

        ds.close();
    }
}
```

### 二、实现双向传输

#### 发送端
```java
import java.net.*;

public class UdpSend
{
    public static void main(String [] args) throws Exception
    {
        DatagramSocket ds=new DatagramSocket();//建立通信管道
        Scanner in = new Scanner(System.in);
        String str=in.nextLine();
        DatagramPacket dp=new DatagramPacket(str.getBytes(),str.getBytes().length,
                InetAddress.getByName("127.0.0.1"),3000);  //127.0.0.1代表本机
        System.out.println("UdpSend: 我要发送信息");
        ds.send(dp);
        System.out.println("UdpSend: 我发送信息结束");

        Thread.sleep(1000);
        byte [] buf=new byte[1024];
        DatagramPacket dp2=new DatagramPacket(buf,1024);
        System.out.println("UdpSend: 我在等待信息");
        ds.receive(dp2);
        System.out.println("UdpSend: 我接收到信息");
        String str2=new String(dp2.getData(),0,dp2.getLength()) +
                " from " + dp2.getAddress().getHostAddress()+":"+dp2.getPort();
        System.out.println(str2);

        ds.close();
    }
}
```




#### 接收端
```java
import java.net.*;

public class UdpRecv
{
    public static void main(String[] args) throws Exception
    {
        DatagramSocket	ds=new DatagramSocket(3000);
        byte [] buf=new byte[1024];
        DatagramPacket dp=new DatagramPacket(buf,1024);
        Scanner in = new Scanner(System.in);

        System.out.println("UdpRecv: 我在等待信息");
        ds.receive(dp);
        System.out.println("UdpRecv: 我接收到信息");
        String strRecv=new String(dp.getData(),0,dp.getLength()) +
                " from " + dp.getAddress().getHostAddress()+":"+dp.getPort();
        System.out.println(strRecv);

        Thread.sleep(1000);
        System.out.println("UdpRecv: 我要发送信息");
        String str=in.nextLine();
        DatagramPacket dp2=new DatagramPacket(str.getBytes(),str.length(),
                InetAddress.getByName("127.0.0.1"),dp.getPort());
        ds.send(dp2);
        System.out.println("UdpRecv: 我发送信息结束");
        ds.close();
    }
}
```

## 感谢阅读