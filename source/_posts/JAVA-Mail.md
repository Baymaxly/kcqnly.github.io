---
title: Java-Mail
date: 2019-11-22 14:02:57
thumbnail: /gallery/java.jpg
tags: Java
categories: Java
---

之前做过的好几个项目都用到了JAVA发送邮件的功能，这里放出我用过的两种方法，其实现在只用第二种方法，hhh。
<!--more-->
## 方法一 使用javax.mail包
有一说一，这个真的捞，我之前用这个问题是真的多，windows上可以发，linux不能发......我是被这个折磨了好几晚

使用前先添加Maven依赖

```xml
<!-- https://mvnrepository.com/artifact/javax.mail/mail -->
<dependency>
    <groupId>javax.mail</groupId>
    <artifactId>mail</artifactId>
    <version>1.4</version>
</dependency>

```
上代码


```java

import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Date;
import java.util.Properties;
public class MailSend {
    //发件人地址
    private static String senderAddress = "xxx@163.com";
    //收件人地址
    private static String recipientAddress ;
    //发件人账户名（网易邮箱用户名）
    private static String senderAccount = "xxx";
    //发件人账户密码(网易邮箱的授权码)
    private static String senderPassword = "xxx";

    private Properties props;
    private Session session;
    public MailSend(String receive)
    {
        this.recipientAddress=receive;
        //1、连接邮件服务器的参数配置
        props = System.getProperties();
        //设置用户的认证方式
        props.setProperty("mail.smtp.auth", "true");
        //设置传输协议
        props.setProperty("mail.transport.protocol", "smtp");
        //设置发件人的SMTP服务器地址
        props.setProperty("mail.smtp.host", "smtp.163.com");

        props.put("mail.smtp.ssl.enable", true);
        //2、创建定义整个应用程序所需的环境信息的 Session 对象
        session = Session.getInstance(props);
    }

    /**
     * 获得创建一封邮件的实例对象
     * @param session
     * @return
     * @throws MessagingException
     * @throws AddressException
     */
    public static MimeMessage getMimeMessage(Session session,String data) throws Exception{
        //创建一封邮件的实例对象
        MimeMessage msg = new MimeMessage(session);
        //设置发件人地址
        msg.setFrom(new InternetAddress(senderAddress));

        msg.setRecipient(MimeMessage.RecipientType.TO,new InternetAddress(recipientAddress));
        //设置邮件主题
        msg.setSubject("Hello","UTF-8");
    
        msg.setText(data);
        //设置邮件的发送时间,默认立即发送
        msg.setSentDate(new Date());

        return msg;
    }

    public void sendMessage(String s) throws Exception {
        //3、创建邮件的实例对象
        Message msg = getMimeMessage(session,s);
        //4、根据session对象获取邮件传输对象Transport
        Transport transport = session.getTransport();
        //设置发件人的账户名和密码
        transport.connect(senderAccount, senderPassword);
        //发送邮件，并发送到所有收件人地址，message.getAllRecipients() 获取到的是在创建邮件对象时添加的所有收件人, 抄送人, 密送人
        transport.sendMessage(msg,msg.getAllRecipients());

        //如果只想发送给指定的人，可以如下写法
        //transport.sendMessage(msg, new Address[]{new InternetAddress("xxx@qq.com")});

        //5、关闭邮件连接
        transport.close();

    }


}
```

这个捞的很，发送HTML语句会出问题，只能发纯文本，这根本不能满足个性化需求啊，难搞

![](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/%E7%86%8A%E7%8C%AB%E5%A4%B4.jpg)


# 还是看方法二吧
## 使用apache.commons.mail包

添加Maven依赖
```xml
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-email -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-email</artifactId>
    <version>1.4</version>
</dependency>

```

代码代码，来看代码
```java
import org.apache.commons.mail.EmailException;
import org.apache.commons.mail.HtmlEmail;

public class MailSend {
    public static void main(String[] args) {
        HtmlEmail email = new HtmlEmail();
        try {
            // 这里是SMTP发送服务器的名字：163的如下："smtp.163.com"
            email.setHostName("smtp.163.com");
            // 字符编码集的设置
            email.setCharset("utf-8");
            // 收件人的邮箱
            email.addTo("xxx@qq.com");
            // 发送人的邮箱2
            email.setFrom("xxx@163.com");
            // 如果需要认证信息的话，设置认证：用户名-密码     ***是你开启POP3服务时的授权码，不是登录密码
            email.setAuthentication("xxx", "xxx");
            // 要发送的邮件主题
            email.setSubject("Test");
            // 要发送的信息，由于使用了HtmlEmail，可以在邮件内容中使用HTML标签
            email.setMsg("<img src='https://xxx.jpg'/>");
            // 发送
            email.send();
            System.out.println("发送成功");
        } catch (EmailException e) {
            e.printStackTrace();
            System.out.println("发送失败");
        }
    }

}

```

### 有一说一，爽


![](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/java/mail.jpg)
# 感谢阅读