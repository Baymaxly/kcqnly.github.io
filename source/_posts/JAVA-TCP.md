---
title: JAVA TCP编程 
thumbnail: /gallery/flyingfish.jpg
date: 2019-08-24 19:25:01
tags: Java
categories: 
- Java
---


TCP：Transmission Control Protocol
- 传输控制协议，面向连接的协议
- 两台机器之间可靠的，无差错的数据传输
- 双向字节流传递


1. 服务器（软件服务器：能实现一定功能，在公开地址上对外服务）
创建一个ServerSocket，等待连接
2. 客户机：创建一个Socket，连接到服务器
3. 服务器:ServerSocket接收到连接，创建一个Socket和客户的Socket创建专线连接，后续服务器和客户机的对话（这一对Socket）会在一个单独的线程（服务器端）上运行
4. 服务器继续等待连接，返回步骤1
<!--more-->

## 相关类
- ServerSocket:服务器码头
-需要绑定port
-如果有多块网卡，需要绑定IP地址
- Socket：运输通道
-客户端绑定服务器的地址和端口
-客户端往Socket输入流写入数据，送到服务端
-客户端从Socket输出流取服务器端过来的地址
-服务端则反之


## 注意事项
- 服务端等待响应时，处于阻塞状态
- 服务端可以同时响应多个客户端
- 服务端每接受一个客户端，就启动一个独立的线程与之对应
- 客户端与服务端都可以选择关闭这对Socket通道

## 代码演示
- 发送时间信息
- 简单回复（复读机）
- 解决中文乱码问题 使用PrintStream类

![客户端](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/java/%E5%AE%A2%E6%88%B7%E7%AB%AF.png)
![服务端](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/java/%E6%9C%8D%E5%8A%A1%E7%AB%AF.png)



客户端
```java

public class TcpClient {
    public static void main(String[] args) {
        try {
            Socket s = new Socket(InetAddress.getByName("127.0.0.1"), 8001);
            InputStream ips = s.getInputStream();    //开启通道的输入流
            BufferedReader brNet = new BufferedReader(new InputStreamReader(ips,StandardCharsets.UTF_8));
            OutputStream ops = s.getOutputStream();  //开启通道的输出流
            PrintStream ps = new PrintStream(ops, true, StandardCharsets.UTF_8);
            BufferedReader brKey = new BufferedReader(new InputStreamReader(System.in,StandardCharsets.UTF_8));
            
            while (true) {
                String strword = brKey.readLine();
                if (strword.equalsIgnoreCase("quit"))
                {
                    ps.println(strword);
                    break;
                }
                else {
                    ps.println(strword);

                    System.out.println("服务端: " + brNet.readLine());
                }
            }
            brNet.close();
            ps.close();
            brKey.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


服务端
```java
public class TcpServer {
    public static void main(String[] args) {
        try {
            ServerSocket ss = new ServerSocket(8001);
            while (true) {
                Socket s = ss.accept();
                System.out.println("连接成功");
                new Thread(new SendTime(s)).start();
            }
            //ss.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


SendTime.java
```java

    public void run() {
        System.out.println("服务开始");
        try {
            InputStream ips = s.getInputStream();
            OutputStream ops = s.getOutputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(ips, StandardCharsets.UTF_8));
            PrintStream ps = new PrintStream(ops, true, StandardCharsets.UTF_8);

            while (true) {
                String str = br.readLine();
                System.out.println("客户端:" + str);
                if (str.equalsIgnoreCase("quit"))
                    break;
                else {
                    if (str.equalsIgnoreCase("time")) {
                        Date time = new Date();
                        SimpleDateFormat ft = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
                        ps.println(ft.format(time));

                    } else {
                        String strEcho = str + " 666";
                        ps.println(strEcho);
                    }
                }
            }
            br.close();
            ps.close();
            s.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
## 感谢阅读

