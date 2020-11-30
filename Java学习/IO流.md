# IO流



> - 视频参考<https://www.bilibili.com/video/BV1Wx411f7qN?p=170>
>- I/O，即输入Input输出Output，IO流指的是数据像瀑布流水一样进行传输
> - 功能：在本地磁盘和网络上操作数据

## IO流划分

> - 按照流向分
>   - 输入流：读数据
>   - 输出流：写数据
> - 按照操作分
>   - 字节流：以字节为单位操作数据010101...
>     - InputStream: 字节输入流的顶层抽象类
>       - FileInputStream      	   普通的字节输入流
>       - BufferedInputStream     高效的字节输入流（字节缓冲输入流）
>     - OutputStream: 字节输出流的顶层抽象类
>       - FileOutputStream           普通的字节输出流
>       - BufferedOutputStream   搞笑的字节输出流（字节缓冲输出流）
>   - 字符流：以字符为单位操作数据
>     - Reader：字符输入流的顶层抽象类
>       - FileReader         普通的字符输入流
>       - BufferedReader 高效的字符输入流（字符缓冲输入流）
>     - Writer：   字符输出流的顶层抽象类
>       - FileWriter			普通的字符输出流
>       - BufferedWriter。 搞笑的字符输出流（字符缓冲输出流）

---

## File类

> File对象代表磁盘某个文件或文件夹

- 构造方法
  - File(String pathname)
  - File(String parent,String child)
  - File(File parent,String child)
- 成员方法
  - createNewFIle();	创建文件
  - mkdir()和mkdirs();  创建目录
  - isDirecory();           判断是否为目录
  - isFIle();                   判断是否为文件
  - exists();                  判断File对象是否存在
  - getAbsolutePath(); 获取绝对路径
  - getPath();               获取相对路径
  - getName();             获取文件名
  - list();                       获取指定目录下所有文件夹名称的数组
  - listFiles();                获取指定目录下所有文件夹File对象数组
  - 其他：查看Java常用类

##  字符流读写文件

### 字符流读文件

> - **字符流只能用来拷贝纯文本文件** 
> - 创建字符流读文件对象
>   - Reader reader = new FileReader("")
> - 调用方法读取数据
>   - Int data = reader.read();
>   - 读取一个字符，返回该字符代表的整数，若到达流的末尾返回 -1
> - 异常
>   - IOException
> - 关闭资源
>   - Reader.close();

```java
public class IOTest {
    public static void main(String[] args) throws IOException {
        //读取字符流写法一
        //创建字符输入流对象
        String path =  new File("").getAbsolutePath();//获取目录前的绝对路径
        Reader reader = new FileReader(path+"/idea_test/lib/test.txt");
        //定义变量，用来接收读取到的字符
        int ch;
        /**
         * (ch = reader.read()) != -1
         * 1.执行了reader.read()，去文件读取一个字符
         * 2.执行ch = reader.read(),将读取到的字符赋值给变量
         * 3.(ch = reader.read())！= -1,用读取到的字符内容和-1做比较
         */
        while((ch = reader.read()) != -1){
            System.out.print((char) ch);//输出字符～ 不强转则输出字符int～
        }
        reader.close();

        //读取字符流写法二
        //创建字符流输入对象
        Reader reader2 = new FileReader(path+"/idea_test/lib/1.txt");
        //定义字符数组
        char[] chs = new char[3];
        //定义一个变量，记录读取到的有效字符数
        int len;
        while((len = reader2.read(chs))!= -1){
            //将读取到的内容，转换成字符串，然后打印
            /**
             * chs表示要操作的数组
             * 0表示起始索引
             * len表示要操作的字符的个数
             */
            String s = new String(chs,0,len);
            System.out.print(s);
        }
        reader2.close();
    }
}
```

### 字符流写文件

- 单个字符写入

> - 创建字符流写文件对象：
>   - Writer writer = new FileWriter("");
> - 调用方法写入数据
>   - int x = '中';
>   - writer.write(x);
>   - 写一个字符
> - 异常处理
>   - throws IOException
> - 关闭资源
>   - writer.close()

- 按字符数组写入

> - 创建字符流写文件对象：
>   - Writer writer = new FileWriter("");
> - 调用方法写入数据
>   - char[] chi = {'你','好','啊'}
>   - writer.write(chs);
>   - 写一个字符数组
> - 异常处理
>   - throws IOException
> - 关闭资源
>   - writer.close()

- 按字符串写入

> - 创建字符流写文件对象：
>   - Writer writer = new FileWriter("");
> - 调用方法写入数据
>   - writer.write("helloworld");
>   - 写一个字符串
> - 异常处理
>   - throws IOException
> - 关闭资源
>   - writer.close()

```java
public class FileWriteTest {
    public static void main(String[] args) throws IOException {
        /**
         * void write(int ch);
         * void write(char[] chs,int index,int len);
         * void write(String str);
         */
        //创建写入对象
        String path = new File("").getAbsolutePath();
        Writer writer = new FileWriter(path + "/idea_test/lib/test.txt");
        //写数据
        writer.write('好');
        writer.write("哈哈哈哈哈哈哈");
        writer.write(new char[]{'哦', '猴'},0,1);
        //释放资源
        writer.close();
    }
}
```

### 字符流拷贝文件 

- 按单个字符读写

  > - 创建字符流读写文件对象：
  >   - Reader reader = new FileReader("readme.txt");
  > - 创建字符流写文件对象：
  >   - Writer writer = new FileWriter("copy.txt");
  > - 调用方法读取数据：
  >   - Int data = reader,read();
  > - 调用方法写入数据：
  >   - writer.write(data);
  > - 异常处理
  >   - throws IOException
  > - 关闭资源
  >   - reader.close()
  >   - writer.close()

```java
public class CopyTest {
    //拷贝文件
    public static void main(String[] args) throws IOException {
        /**
         * IO流拷贝文件核型六步
         * 1.创建字符输入流对象，关联数据源文件
         * 2.创建字符输出流对象，关联目的地文件
         * 3.定义变量，记录读取到的内容
         * 4.循环读取，只要条件满足就一直读，并将读取到的内容赋值给变量
         * 5.将读取到的数据写入到 目的地文件夹
         * 6.释放资源
         */
        //1
        String path = new File("").getAbsolutePath();
        FileReader fr = new FileReader(path+"/idea_test/lib/1.txt");
        //2
        FileWriter fw = new FileWriter(path+"/idea_test/lib/3.txt");//细节：如果目的地文件不存在，程序会自动创建
        //3
        int len;
        //4
        while ((len = fr.read())!=-1){
            //5
            fw.write(len);
        }
        //6
        fr.close();
        fw.close();
    }
}
```

- 按字符数组读写

> - 创建字符流读文件对象：
>   - Reader reader = new FileReader("readme.txt");
> - 创建字符流写文件对象：
>   - Writer write  = new FileWriter("copy.txt");
> - 调用方法读取数据：
>   - char[] chs = new char[2048];
>   - int len = reader.read(chs);
> - 调用方法写入数据：
>   - writer.write(chs,0,len);
> - 异常处理
>   - Throws IOException
> - 释放资源
>   - reader.close()
>   - writer.close()

```java
public class CopyTest2 {
    public static void main(String[] args) throws IOException {
        /**
         * IO流拷贝文件核型六步
         * 1.创建字符输入流对象，关联数据源文件
         * 2.创建字符输出流对象，关联目的地文件
         * 3.定义变量，记录读取到的有效字符数
         * 4.循环读取，只要条件满足就一直读，并将读取到的有效字符书赋值给变量
         * 5.将读取到的数据写入到 目的地文件夹
         * 6.释放资源
         */
        //1
        String path = new File("").getAbsolutePath();
        FileReader fr = new FileReader(path+"/idea_test/lib/1.txt");
        //2
        FileWriter fw = new FileWriter(path+"/idea_test/lib/4.txt");//细节：如果目的地文件不存在，程序会自动创建
        //3
        char[] chs = new char[1024];
        int len;
        //4
        while ((len = fr.read(chs))!=-1){
            //5
            fw.write(chs,0,len);
        }
        //6
        fr.close();
        fw.close();
    }
}
```

### 字符缓冲流拷贝文件

> - 创建字符流读文件对象
>   - BufferedReader br = new BufferedReader(new FileReader("readme.txt"));
> - 创建字符流写文件对象
>   - BufferedWriter bw = new BufferedWriter(new FileWriter("copy.txt"));
> - 异常处理
>   - throws IOException
> - 使用while循环读写数据
>   - Int len;
>   - while((len = br. read())!=-1){
>   - ​       bw.write(len);
>   - }
> - 关闭资源
>   - br.close()
>   - bw.close()
> - 特点：自带有缓冲区，大小为16KB = 8192个字符

- 普通用法

```java
public class CopyTest {
    //拷贝文件
    public static void main(String[] args) throws IOException {

        //1
        String path = new File("").getAbsolutePath();
        BufferedReader br = new BufferedReader(new FileReader(path+"/idea_test/lib/1.txt"));
        //2
        BufferedWriter bw = new BufferedWriter(new FileWriter(path+"/idea_test/lib/3.txt"));
        //3
        int len;
        //4
        while((len = br.read()) != -1 ){
            //5
            bw.write(len);
        }
        //6
        br.close();
        bw.close();
    }
}
```

- 一次读写一行

```java
public class CopyTest {
    //拷贝文件
    public static void main(String[] args) throws IOException {
        //1
        String path = new File("").getAbsolutePath();
        BufferedReader br = new BufferedReader(new FileReader(path+"/idea_test/lib/1.txt"));
        //2
        BufferedWriter bw = new BufferedWriter(new FileWriter(path+"/idea_test/lib/5.txt"));
        //3
        String str;
        //4
        while((str = br.readLine()) != null ){
            //5
            bw.write(str);
            //换行
            /**
             * 由于不同系统的换行符不同，java支持跨平台！所以使用java自带的方法
             * windows  \r\n
             * mac  \r
             * unix \n
             */
            bw.newLine();
        }
        //6
        br.close();
        bw.close();
    }
}
```

## 字节流读写文件

### 字节流拷贝文件

- 按单个字节读写

> - 创建字节流读写文件对象
>
>   - InputStream is = new FileInputStream("xxx.png");
>
> - 创建字节流写文件对象
>
>   - OutputStram os = new FIleOutputStream("copy,png")
>
> - 异常处理
>
>   - throw IOException
>
> - 使用while循环读写数据
>
>   - int b
>     while((b = is.read()) != -1){
>
>     ​			os.write(b);
>     }
>
> - 关闭资源
>   - is.close()
>     os.close()

```java
public class StreamTest {
    public static void main(String[] args) throws IOException {
        String path = new File("").getAbsolutePath();
        FileInputStream fis = new FileInputStream(path+"/idea_test/lib/1.jpeg");
        FileOutputStream fos = new FileOutputStream(path+"/idea_test/lib/copy.jpeg");//不存在则创建
        int len;
        while ((len = fis.read())!= -1){
            fos.write(len);
        }
        fis.close();
        fos.close();
    }
}
```

- 按字节数组读写

> - 创建字节流读写文件对象
>   - InputStream is = new FileInputStream("xxx.png");
> - 创建字节流写文件对象
>   - OutputStram os = new FIleOutputStream("copy,png")
> - 异常处理
>   - throw IOException
> - 定义字节数组，设置每次读取的字节
>   - byte[] b = new byte[2048]
> - 循环读写数据
>   - Int len;
>   - while((len = is.read(b))!=-1){
>            os.write(b,0,len)
>     }
> - 关闭资源
>   - is.close()
>     os.close()

```java
public class StreamTest {
    public static void main(String[] args) throws IOException {
        String path = new File("").getAbsolutePath();
        FileInputStream fis = new FileInputStream(path+"/idea_test/lib/1.jpeg");
        FileOutputStream fos = new FileOutputStream(path+"/idea_test/lib/copy2.jpeg");//不存在则创建
        byte[] bys = new byte[1024];
        int len;
        while ((len = fis.read(bys))!= -1){
            fos.write(bys,0,len);
        }
        fis.close();
        fos.close();
    }
}
```

### 字节缓冲流拷贝文件

> - 创建字节缓冲流读文件对象
>
>   - BufferedInputStream bis = new BufferedInputStream(new FileInputStream("xxx.jpg"));
>
> - 创造字节缓冲流写文件对象
>
>   - BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copty.jpg"))
>
> - 异常处理
>
>   - throws IOException
>
> - 使用while循环读写数据
>
>   - int len;
>
>   - while((len = bis.read()) != -1){
>
>     ​      bos.write(len);
>     }
>
> - 关闭资源
>
>   - bis.close()
>     bos.close()

```java
public class StreamTest {
    public static void main(String[] args) throws IOException {
        String path = new File("").getAbsolutePath();
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(path+"/idea_test/lib/copy2.jpeg"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(path+"/idea_test/lib/copy3.jpeg"));
        int len;
        while ((len = bis.read())!=-1){
            bos.write(len);
        }
        bis.close();
        bos.close();
    }
}
```

- **总结：拷贝纯文本文件使用字符流，拷贝其他使用字节流～**

---

## 上传用户头像功能

> 步骤
>
> 1. 控制台录入用户头像路径
> 2. 解析路径字符串是否合法
> 3. 判断路径表示的File对象是否存在，是否为文件
> 4. 读取该文件并写入到指定目录
> 5. 提示头像上传成功或失败

```java
public class UpLoadFile {
    public static void main(String[] args) throws IOException {
        //定义一个方法获取用户头像路径
        //定义一个方法判断文件是否存在
        //如果存在，上传失败
        //如果不存在，就上传，上传成功
        File path = getPath();
        System.out.println(path);
        boolean flag = isExists(path.getName());
        if (flag){
            System.out.println("该文件已存在");
        }else{
            System.out.println("上传文件中");
            uploadFile(path);
            System.out.println("上传完成");
        }
    }
    /**
     * 用来获取要上传用户头像的路径
     * @return
     */
    public static File getPath(){
        while(true){
            //用户输入路径，并接收
            Scanner scanner = new Scanner(System.in);
            System.out.println("请输入您要录入的用户头像路径");
            String path = scanner.nextLine();
            //判读录入后缀名是否正确
            //如果不是，重写录入
            if (!path.endsWith(".jpg")&&!path.endsWith(".png")&&!path.endsWith(".bmp")&&!path.endsWith(".jpeg")){
                System.out.println("您录入的不是照片，请重新输入");
                continue;
            }
            //如果是判断路径文件是否存在，是否是文件
            File file = new File(path);
            if (file.exists() && file.isFile()){
                return file;
            }else{
                System.out.println("您录入的路径不合法，请重新录入");
            }
        }
    }

    /**
     * 判断文件是否存在
     * @param path
     * @return
     */
    public static boolean isExists(String path){
        String absolute_path = new File("").getAbsolutePath();
        //将lib文件夹封装成File对象
        File file = new File(absolute_path+"/idea_test/lib");
        //获取lib文件夹中所有的文件的名称数组
        String[] names = file.list();
        //遍历获取到的数组，用获取到的数据依次和path进行比较
        for (String name : names){
            if (name.equals(path)){
                //返回已存在
                return true;
            }
        }
        //不存在返回false
        return false;
    }

    /**
     * 用来上传用户头像
     * @param path 数据源文件路径
     */
    public static void uploadFile(File path) throws IOException {
        String absolute_path = new File("").getAbsolutePath();
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(path));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(absolute_path+"/idea_test/lib/"+path.getName()));
        int len;
        while ((len = bis.read())!=-1){
            bos.write(len);
        }
        bis.close();
        bos.close();
    }
}
```

