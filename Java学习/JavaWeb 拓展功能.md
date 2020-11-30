# JavaWeb 拓展功能

## 文件上传

```java
package Servlet;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.sql.SQLException;
import java.util.List;
import java.util.UUID;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import domain.Product;
import factory.ServiceFactory;
import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileUploadException;
import org.apache.commons.fileupload.ProgressListener;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

/**
 * Servlet implementation class FileServlet
 */

public class FileServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse
     *      response)
     */
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // TODO Auto-generated method stub
        // response.getWriter().append("Served at: ").append(request.getContextPath());

        // 判断上传的文件普通表单还是带文件的表单
        if (!ServletFileUpload.isMultipartContent(request)) {
            return;//终止方法运行,说明这是一个普通的表单,直接返回
        }
        //创建上传文件的保存路径,建议在WEB-INF路径下,安全,用户无法直接访间上传的文件;
//        String uploadPath =  request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + request.getContextPath()+"/admin/images/books";
        String uploadPath =this.getServletContext().getRealPath("/admin/images/books");
        File uploadFile = new File(uploadPath);
        System.out.println("path:"+uploadPath);
        if (!uploadFile.exists()){
            uploadFile.mkdir(); //创建这个目录
        }

        // 创建上传文件的保存路径，建议在WEB-INF路径下，安全，用户无法直接访问上传的文件
//        String tmpPath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + request.getContextPath()+"/admin/images/temp";
        String tmpPath = this.getServletContext().getRealPath("/admin/images/temp");
        File file = new File(tmpPath);
        if (!file.exists()) {
            file.mkdir();//创建临时目录
        }

        // 处理上传的文件,一般都需要通过流来获取,我们可以使用 request, getInputstream(),原生态的文件上传流获取,十分麻烦
        // 但是我们都建议使用 Apache的文件上传组件来实现, common-fileupload,它需要旅 commons-io组件;
        try {
            // 创建DiskFileItemFactory对象，处理文件路径或者大小限制
            DiskFileItemFactory factory = getDiskFileItemFactory(file);
            /*
             * //通过这个工厂设置一个缓冲区,当上传的文件大于这个缓冲区的时候,将他放到临时文件 factory.setSizeThreshold(1024 *
             * 1024); //缓存区大小为1M factory.setRepository (file);//临时目录的保存目录,需要一个File
             */

            // 2、获取ServletFileUpload
            ServletFileUpload upload = getServletFileUpload(factory);

            // 3、处理上传文件
            // 把前端请求解析，封装成FileItem对象，需要从ServletFileUpload对象中获取
            String msg = uploadParseRequest(upload, request, uploadPath);

            // Servlet请求转发消息
            if(msg.equals("200")) {
                // Servlet请求转发消息
                response.sendRedirect("/admin/login/home.jsp?item=product_list");
                return;
            }else {
                msg ="请上传文件";
            }
            request.setAttribute("msg",msg);
            request.getRequestDispatcher(request.getContextPath()+"/admin/home.jsp?message="+msg).forward(request, response);

        } catch (FileUploadException e) {
            // TODO 自动生成的 catch 块
            e.printStackTrace();
        }
    }

    public static DiskFileItemFactory getDiskFileItemFactory(File file) {
        DiskFileItemFactory factory = new DiskFileItemFactory();
        // 通过这个工厂设置一个缓冲区,当上传的文件大于这个缓冲区的时候,将他放到临时文件中;
        factory.setSizeThreshold(1024 * 1024);// 缓冲区大小为1M
        factory.setRepository(file);// 临时目录的保存目录,需要一个file
        return factory;
    }

    public static ServletFileUpload getServletFileUpload(DiskFileItemFactory factory) {
        ServletFileUpload upload = new ServletFileUpload(factory);
        // 监听长传进度
        upload.setProgressListener(new ProgressListener() {

            // pBytesRead:已读取到的文件大小
            // pContextLength:文件大小
            public void update(long pBytesRead, long pContentLength, int pItems) {
                System.out.println("总大小：" + pContentLength + "已上传：" + pBytesRead);
            }
        });

        // 处理乱码问题
        upload.setHeaderEncoding("UTF-8");
        // 设置单个文件的最大值
        upload.setFileSizeMax(1024 * 1024 * 10);
        // 设置总共能够上传文件的大小
        // 1024 = 1kb * 1024 = 1M * 10 = 10м

        return upload;
    }

    public static String uploadParseRequest(ServletFileUpload upload, HttpServletRequest request, String uploadPath)
            throws FileUploadException, IOException {

        String msg = "";
        Product product = new Product();

        // 把前端请求解析，封装成FileItem对象
        List<FileItem> fileItems = upload.parseRequest(request);
        for (FileItem fileItem : fileItems) {
            if (fileItem.isFormField()) {// 判断上传的文件是普通的表单还是带文件的表单
                // getFieldName指的是前端表单控件的name;
                String name = fileItem.getFieldName();
                String value = fileItem.getString("UTF-8"); // 处理乱码

                switch (name){
                    case "name":
                        product.setName(value);
                        break;
                    case "price":
                        product.setPrice(Double.parseDouble(value));
                        break;
                    case "pnum":
                        product.setPnum(Integer.parseInt(value));
                        break;
                    case "category":
                        product.setCategory(value);
                        break;
                    case "description":
                        product.setDescription(value);
                }

            } else {// 判断它是上传的文件

                // ============处理文件==============

                // 拿到文件名
                String uploadFileName = fileItem.getName();
                System.out.println("上传的文件名: " + uploadFileName);
                if (uploadFileName.trim().equals("") || uploadFileName == null) {
                    continue;
                }

                // 获得上传的文件名
                String fileName = uploadFileName.substring(uploadFileName.lastIndexOf("/") + 1);
                // 获得文件的后缀名
                String fileExtName = uploadFileName.substring(uploadFileName.lastIndexOf(".") + 1);

                /*
                 * 如果文件后缀名fileExtName不是我们所需要的 就直按return.不处理,告诉用户文件类型不对。
                 */

                System.out.println("文件信息[件名: " + fileName + " ---文件类型" + fileExtName + "]");
                // 可以使用UID（唯一识别的通用码),保证文件名唯
                // 0UID. randomUUID(),随机生一个唯一识别的通用码;
                String uuidPath = UUID.randomUUID().toString();

                // ================处理文件完毕==============

                // 存到哪? uploadPath
                // 文件真实存在的路径realPath
                String realPath = uploadPath + "/" + uuidPath;
                // 给每个文件创建一个对应的文件夹
                File realPathFile = new File(realPath);
                if (!realPathFile.exists()) {
                    realPathFile.mkdir();
                }
                // ==============存放地址完毕==============


                // 获得文件上传的流
                InputStream inputStream = fileItem.getInputStream();
                // 创建一个文件输出流
                // realPath =真实的文件夹;
                // 差了一个文件;加上翰出文件的名产"/"+uuidFileName
                FileOutputStream fos = new FileOutputStream(realPath + "/" + fileName);

                // 创建一个缓冲区
                byte[] buffer = new byte[1024 * 1024];
                // 判断是否读取完毕
                int len = 0;
                // 如果大于0说明还存在数据;
                while ((len = inputStream.read(buffer)) > 0) {
                    fos.write(buffer, 0, len);
                }
                // 关闭流
                fos.close();
                inputStream.close();
                String path =  request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + request.getContextPath()+"/admin/images/books";
                String url = path + "/" + uuidPath + "/" + fileName;
                product.setImgurl(url);
                try {
                    ServiceFactory.getProductInstance().addProduct(product);
                    msg="200";
                } catch (SQLException e) {
                    e.printStackTrace();
                }finally {
                    fileItem.delete(); // 上传成功,清除临时文件
                }
                //=============文件传输完成=============
            }
        }
        return msg;

    }
}
```



## 邮件发送

![thBIdH.png](https://s1.ax1x.com/2020/06/08/thBIdH.png)

### 传输协议

- SMTP协议
  - 发送邮件
  - 我们通常把用户smtp请求的服务器称之为SMTP服务器（邮件发送服务器）

- POP3协议
  - 接收邮件
  - 我们通常把处理用户pop3请求的服务器称之为POP3服务器（邮件接收服务器）

### 流程

1. A通过smtp协议连接到smtp服务器，然后发送一封邮件给**网易**的邮件服务器
2. **网易**邮件服务器分析发现需要去B的邮件服务器，通过smtp协议将邮件转投给**QQ**的smtp服务器
3. **QQ**将接收到的邮件存储在B用户这个邮件账号的文件系统中
4. B通过POP3协议连接到POP3服务器收取邮件
5. 从B这个邮箱账号的文件系统中取出邮件
6. **QQ**的POP3服务器将取出来的邮件发送到B手中



### Java发送邮件

> 使用java发送E-mail十分简单，但是首先得准备JavaMail API 和 Java Activation Framework

- 得到两个包
  - mail.jar
  - activation.jar



- 发送简单邮件，测试
  1. 创建包含邮件服务器的网络连接信息的Session对象
  2. 创建代表邮箱内容的Message对象
  3. 创建Transport对象，连接服务器，发送Message，关闭连接

主要有四个核心类，我们在编写程序时，记住这四个核心类，就很容易编写出java邮件处理程序

![thbNM8.png](https://s1.ax1x.com/2020/06/09/thbNM8.png)

```java
//普通邮件
package util;


import com.sun.mail.util.MailSSLSocketFactory;
import domain.User;

import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Properties;

//网站3秒原则：用户体验
//多线程实现用户体验，异步处理
public class SendMail extends Thread {

    private User user;

    public SendMail(User user){
        this.user = user;
    }

    @Override
    public void run() {
        try{
            //设置发送邮件的一些属性
            Properties prop = new Properties();
            prop.setProperty("mail.transport.protocol", "smtp");//邮件发送协议
            prop.setProperty("mail.smtp.host", "smtp.qq.com");//设置qq邮件服务器
            prop.setProperty("mail.smtp.auth", "true");//需要设置用户名密码

            //QQ邮件，还要设置SSL加密，加上以下代码即可
            MailSSLSocketFactory sf = new MailSSLSocketFactory();
            sf.setTrustAllHosts(true);
            prop.put("mail.smtp.ssl.enable", "true"); //qq邮箱必须设置这一项，ssl加密选项
            prop.put("mail.smtp.ssl.socketFactory",sf);


            //使用javaMail发送邮件的五个步骤

            //1.创建定义整个应用程序所需要的环境信息Session对象
            Session session = Session.getDefaultInstance(prop, new Authenticator() {
                public PasswordAuthentication getPasswordAuthentication() {
                    //这里需要验证邮箱的授权码，QQ邮箱需要授权码
                    return new PasswordAuthentication("1725236585@qq.com", "mehrksdiagwtbgce");
                }
            });

            //开启debug 输出控制带信息
//            session.setDebug(true);

            //2.通过session得到transport对象
            Transport transport = session.getTransport();

            //3.使用邮箱的用户名和授权码连上邮件服务器
            transport.connect("smtp.qq.com","1725236585@qq.com", "********");

            //4.创建邮箱
            //注意需要传递Session
            MimeMessage message = new MimeMessage(session);

            //发件人
            message.setFrom(new InternetAddress("1725236585@qq.com"));

            //收件人
            message.setRecipient(Message.RecipientType.TO,new InternetAddress(user.getEmail()));

            //邮件标题
            message.setSubject("用户注册邮件");

            //邮件内容
            message.setContent("恭喜您注册成功，您的用户名："+user.getUsername()+"，您的密码："+user.getPassword()+",请妥善保管，如有问题请联系网站客服！","text/html;charset=UTF-8");
            message.saveChanges();//保存修改
            //5.发送邮件
            transport.sendMessage(message,message.getAllRecipients());

            //6.关闭连接
            transport.close();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```



### 带图片和附件的邮件

MIME（多用途互联网邮件扩展类型）

- MimeBodyPart类
  - javax.mail.internet.MimeBodyPart类 表示的是一个MIME消息，它和MimeMessage类一样都是从Part接口继承过来。

- MimeMultipart类
  - javax.mail.internet.MimeMultipart是抽象类 Multipart的实现子类,它用来组合多个MIME消息。一个MimeMultipart对象可以包含多个代表MIME消息的MimeBodyPart对象。

![toav2q.png](https://s1.ax1x.com/2020/06/10/toav2q.png)

```java
//带附件和图片
import com.sun.mail.util.MailSSLSocketFactory;

import javax.activation.DataHandler;
import javax.activation.FileDataSource;
import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;
import java.security.GeneralSecurityException;
import java.util.Properties;

public class SendFileMail {
    public static void main(String[] args) throws MessagingException, GeneralSecurityException {

        //创建一个配置文件保存并读取信息
        Properties properties = new Properties();

        //设置qq邮件服务器
        properties.setProperty("mail.host","smtp.qq.com");
        //设置发送的协议
        properties.setProperty("mail.transport.protocol","smtp");
        //设置用户是否需要验证
        properties.setProperty("mail.smtp.auth", "true");

        //=================================只有QQ存在的一个特性，需要建立一个安全的链接
        // 关于QQ邮箱，还要设置SSL加密，加上以下代码即可
        MailSSLSocketFactory sf = new MailSSLSocketFactory();
        sf.setTrustAllHosts(true);
        properties.put("mail.smtp.ssl.enable", "true");
        properties.put("mail.smtp.ssl.socketFactory", sf);
        //=================================准备工作完毕

        //1.创建一个session会话对象；
        Session session = Session.getDefaultInstance(properties, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication("24736743@qq.com", "授权码");
            }
        });

        //可以通过session开启Dubug模式，查看所有的过程
        session.setDebug(true);


        //2.获取连接对象，通过session对象获得Transport，需要捕获或者抛出异常；
        Transport tp = session.getTransport();

        //3.连接服务器,需要抛出异常；
        tp.connect("smtp.qq.com","24736743@qq.com","授权码");

        //4.连接上之后我们需要发送邮件；
        MimeMessage mimeMessage = imageMail(session);

        //5.发送邮件
        tp.sendMessage(mimeMessage,mimeMessage.getAllRecipients());

        //6.关闭连接
        tp.close();
    }

    public static MimeMessage imageMail(Session session) throws MessagingException {

        //消息的固定信息
        MimeMessage mimeMessage = new MimeMessage(session);

        //邮件发送人
        mimeMessage.setFrom(new InternetAddress("24736743@qq.com"));
        //邮件接收人，可以同时发送给很多人，我们这里只发给自己；
        mimeMessage.setRecipient(Message.RecipientType.TO, new InternetAddress("24736743@qq.com"));
        mimeMessage.setSubject("我也不知道是个什么东西就发给你了"); //邮件主题

        /*
        编写邮件内容
        1.图片
        2.附件
        3.文本
         */

        //图片
        MimeBodyPart body1 = new MimeBodyPart();
        body1.setDataHandler(new DataHandler(new FileDataSource("src/resources/yhbxb.png")));
        body1.setContentID("yhbxb.png"); //图片设置ID

        //文本
        MimeBodyPart body2 = new MimeBodyPart();
        body2.setContent("请注意，我不是广告<img src='cid:yhbxb.png'>","text/html;charset=utf-8");

        //附件
        MimeBodyPart body3 = new MimeBodyPart();
        body3.setDataHandler(new DataHandler(new FileDataSource("src/resources/log4j.properties")));
        body3.setFileName("log4j.properties"); //附件设置名字

        MimeBodyPart body4 = new MimeBodyPart();
        body4.setDataHandler(new DataHandler(new FileDataSource("src/resources/1.txt")));
        body4.setFileName(""); //附件设置名字

        //拼装邮件正文内容
        MimeMultipart multipart1 = new MimeMultipart();
        multipart1.addBodyPart(body1);
        multipart1.addBodyPart(body2);
        multipart1.setSubType("related"); //1.文本和图片内嵌成功！

        //new MimeBodyPart().setContent(multipart1); //将拼装好的正文内容设置为主体
        MimeBodyPart contentText =  new MimeBodyPart();
        contentText.setContent(multipart1);

        //拼接附件
        MimeMultipart allFile =new MimeMultipart();
        allFile.addBodyPart(body3); //附件
        allFile.addBodyPart(body4); //附件
        allFile.addBodyPart(contentText);//正文
        allFile.setSubType("mixed"); //正文和附件都存在邮件中，所有类型设置为mixed；

        //放到Message消息中
        mimeMessage.setContent(allFile);
        mimeMessage.saveChanges();//保存修改

        return mimeMessage;
    }
}
```