---
title: JAVA IO总结
tags: IO
categories: JAVA
---

#JAVA IO总结

![IO分类图](https://www.2cto.com/uploadfile/Collfiles/20160801/20160801093838973.png)

## InputStream和OutputStream 
* InputStream是所有字节输入流的父类
- int read():读取一个字节以int形式返回,该int值的低8位有效，返回-1则表示EOF
- int read(byte[] b): 读取b.length字节到数组中，返回值为实际读取到的字节量

* OutputStream是所有字节输出流的父类
- void write(int d) 写出一个字节，写的是int的低8位
- void write(byte[] d)将给定的字节数组中的所有字节全部写出

## 文件字节流
* FileInputStream 是文件的字节输入流
- int read()
- int read(byte[] b)

* FileOutputStream 是文件的字节输出流，FOS有两种模式一种是重写模式，一种是追加模式
- void write(int d)
- void write(byte[] d)
- void write(byte[] d,int offset,int len)将字节数组d中 从偏移量offset开始的 len个字节写入文件输出流中

```
/**
 * 文件流
 * FileOutPutStream是字节输出流
 */
public class FosTest {

    File f = new File("fileOut.txt");

    //测试FOS 追加模式
    @Test
    @Ignore
    public void testFosAppendMode(){
        try{
            FileOutputStream fos = new FileOutputStream(f,true);
            fos.write(",Hello every one I am David".getBytes());
            fos.flush();
            fos.close();

        }catch (Exception e){
            e.printStackTrace();
        }
    }


    //测试FOS 重写模式
    @Test
    @Ignore
    public void testFosOverrideMode(){
        try{
            FileOutputStream fos = new FileOutputStream(f);
            fos.write("You are good Boy".getBytes());
            fos.flush();
            fos.close();
        }catch (Exception e){
            e.printStackTrace();
        }

    }
}
```

## 缓冲流
> 在读取数据时 若以字节为单位读取数据，会因为读取次数频繁而大大的降低读取效率
缓冲流就是通过提高每次读写的字节数量,减少读写次数 从而提高读写效率

* BufferedInputStream
* BufferedOutputStream

```
/**
 * BufferedOutputStream
 * 内部维护着一个缓冲区,当我们向该流写数据时，都会先将数据写入缓冲区, 当缓冲区已满时,缓冲流会一次性将数据写出
 * 用途：在向硬件设备写出数据时, 使用缓冲流可以减小写出次数，从而提高了写出效率
 * 注意: 有时我们在写出数据后希望 数据立即被写出，而不是存储在缓冲区中 直到缓冲区满后才真正写出，这时可以调用flush()强制写出
 *
 * BufferedInputPutStream 处理流
 * 内部也是维护一个缓冲区，当读取字节数据时会尽可能多的读取 然后存入缓冲区，然后逐一的返回缓存的字节数据，直到读取完毕
 */
public class BosBisTest {
    @Test
    public void testBos(){
        try{
            byte[] d = new byte[100];

            FileOutputStream fos = new FileOutputStream("fileOut.txt",true);
            BufferedOutputStream bos = new BufferedOutputStream(fos);
            bos.write(",I am from China".getBytes());
            bos.close();

            FileInputStream fio = new FileInputStream("fileOut.txt");
            BufferedInputStream bio = new BufferedInputStream(fio);

            int dd = -1;
            while((dd = bio.read())!=-1){
                //一个个字节的读取输出了，
                System.out.println(dd+",");
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }

}

```


## Reader和Writer
* Reader是字符输入流的父类，当要读文本文件时最好使用Reader类，它为读取文本而设计
* Writer是字符输出流的父类，当要写文本文件时最好使用Writer类，而不是直接写OutputStream；因为Writer类就是为写文本而设计的。

* 字符流是以char(字符)为单位读写数据的,一次处理一个Unicode
* 字符流的底层仍然是基本的字节流.

### Reader常用方法
- int read(): 读取一个字符,返回int值 "低16位"有效
- int read(char[] chs): 读取一个字符数组的length个字符并存入该数组,返回值为实际读取到的字符量

### Writer常用方法
- void write(int c): 写出一个字符,写出给定int值 “低16位”表示的字符。
- void write(char[] chs):将给定字符数组中所有字符写出
- void write(char[] chs,int offset,int len):将给定的字符数组中从offset处开始连续的len个字符写出


### 转换流
###InputStreamReader: 字符输入流
 使用该流可以设置字符集,并按照指定的字符集从流中按照该编码将字节数据转换为字符并读取 
 
  * InputStreamReader(InputStream in, String charsetName) 根据指定字符集创建ISR
  * InputStreamReader(InputStream in) 根据系统默认字符集创建ISR

### OutputStreamWriter:字符输出流
使用该流可以设置字符集,并按照指定字符集 将字符转换为对应字节后通过流写出 

* OutputStreamWriter(OutputStream out,String charsetName) 基于指定的字节输出流和字符集创建OSW
* OutputStreamWriter(OutputStream out) 基于指定的字节输出流和系统默认字符集创建OSW


```
/**
 * 测试InputStreamReader 和 OutputStreamWriter
 */
public class OswTest {

    @Test
    @Ignore
    public void testOsw() throws java.io.IOException{
        FileOutputStream fos = new FileOutputStream("demo.txt");
        OutputStreamWriter osw = new OutputStreamWriter(fos,"UTF-8");
        osw.write("中国人");
        osw.close();
    }

    @Test
    public void testIsr() throws java.io.IOException{
        FileInputStream fis = new FileInputStream("demo.txt");
        InputStreamReader isr = new InputStreamReader(fis,"utf-8");
        int data = -1;
        while ((data=isr.read())!=-1){
            System.out.println((char)data);
        }

        isr.close();
    }

}

```



### 文件字符流
* FileReader
* FileWriter

```
public class FileReaderWriterTest {
    @Test
    public void test(){
        try{
            File f = new File("demo.txt");
            FileWriter fw = new FileWriter(f);
            fw.write("Hello\ndavid");
            fw.close();

            FileReader fr = new FileReader(f);
            int res = 0;
            while ( (res=fr.read())!=-1){
                System.out.println((char)res);
            }

            fr.close();
        }catch (Exception e){
            e.printStackTrace();
        }

    }
}
```




### 字符缓冲流
* BufferedReader 它是缓存字符输入流,内部提供了缓冲区 可以提交读取效率

```
BufferedReader(Reader reader) 构造函数
String readLine() 该方法连续读取一行字符串
```

* BufferedWriter
 
 
```
 BufferedWriter(Writer writer)构造函数
 void newLine()  //写入一个行分隔符。 
 void write(char ch)
 void write(char[] cbuf)
 void write(char[] cbuf,int offset,int len)
 ```
 
 Demo代码
 
 ```
 public class BufferedReaderWriter {
 	
    @Test
    @Ignore
    public void testReader(){
        try {
            BufferedReader br = new BufferedReader(new FileReader("demo.txt"));
            String str= "";
            while((str=br.readLine())!=null){
                System.out.println("---"+str);
            }
            br.close();

        }catch (Exception e){
            e.printStackTrace();
        }
    }

    @Test
    public void testWriter(){
        try{
            BufferedWriter bw = new BufferedWriter(new FileWriter("demo.txt"));
            bw.write("Hello");

            bw.newLine();
            bw.write("I am david");
            bw.newLine();
            bw.close();

        }catch (Exception e){
            e.printStackTrace();
        }
    }

}
```
 

