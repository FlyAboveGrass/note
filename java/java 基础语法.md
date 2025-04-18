# java 的三大版本

1. javaSE。标准版，桌面程序，控制台开发
2. javaME。嵌入式开发
3. javaEE。E 企业级开发，服务器开发

# JDK、JRE、JVM

- JDK。 java development kit
	- JRE。 java runtime environment
		- JVM。 java virtual machine
![[Pasted image 20230902235726.png]]

# 基础语法

**java 保留关键字**

- 访问控制
	- public
	- protected
	- private
- 类、方法、函数修饰符
	- abstract
	- class
	- extends
	- final 最终的，不可改变的
	- implements
	- interface
	- native
	- new
	- static
	- strictfp 严格浮点、精准浮点
	- synchronized 线程、同步
	- transient 短暂
	- volatile 易失
- 保留关键字
	- goto
	- const

## 变量类型

- 参数变量
	- 只在方法被调用的时候存在，只在函数内部可以使用
	- 基本类型是值传递的，对象类型是引用传递的
- 局部变量
	- 局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁。
	- 必须初始化
	- 只在函数内部可以访问
- 成员变量
	- 成员变量声明在一个类中，但在方法、构造方法和语句块之外。当一个对象被实例化之后，每个成员变量的值就跟着确定。
- 类变量（静态变量）
	- 无论创建多少个类实例，静态变量在内存中只有一份拷贝，被所有实例共享。
	- 静态变量在类加载时被创建，在整个程序运行期间都存在。
		- 因为这个特性，静态变量可以用来存储整个程序都需要使用的数据，如配置信息、全局变量等。

## 修饰符

**访问控制修饰符**
- **default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。
- **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
- **public** : 对所有类可见。使用对象：类、接口、变量、方法
- **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。

## Number & Math 类

- Number 类
	- Byte
	- Short
	- Integer
	- Long
	- Float
	- Double
	- BigDecimal
	- BigInteger
- Math 类
	- Math 类提供了许多方法，包括
		- abs()
		- ceil()
		- floor()
		- max()
		- min()
		- pow()
		- sqrt()
		- random()
		- sin()
		- cos()
		- tan()
		- asin()
		- acos()
		- atan()
		- log()
		- exp()
		- round()

## Character 类

Character 类用于对单个字符进行操作。
-  `isLetter(char ch)`
- `isDigit(char ch)`
- `isWhitespace(char ch)`
- `isUpperCase(char ch)`
- `isLowerCase(char ch)`
- `toUpperCase(char ch)`
- `toLowerCase(char ch)`
- `toString(char ch)`

## ** String 类**

创建字符串的方式对比

> 注意： String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了。如果需要修改，则应该使用 StringBuffer 类。

字面量定义的String，如果定义的String是第一次出现的，会被存放在字符串常量池，然后返回字符串常量池里的引用，否则则直接返回字符串常量池里的引用。而对象定义的String会被存放在堆里。

```
// String 创建的字符串存储在公共池
String str = "Runoob";


// new 创建的字符串对象在堆上
String str2=new String("Runoob");
```


String 类中常用的方法
- `length()`：返回字符串的长度
- `charAt(int index)`：返回指定索引处的字符
- `indexOf(char c)`：返回字符在字符串中第一次出现的索引
- `lastIndexOf`
- `substring(int beginIndex)`：返回从指定索引处开始到字符串末尾的子字符串
- `substring(int beginIndex, int endIndex)`：返回从指定索引处开始到指定索引之间的子字符串
- `toUpperCase()`：将字符串转换为大写字母
- `toLowerCase()`：将字符串转换为小写字母
- `trim()`：去除字符串前后的空格
- `equals(Object obj)`：比较字符串是否相等
- `compareTo(String anotherString)`：按字典顺序比较字符串
- `replace(char oldChar, char newChar)`：用新字符替换字符串中的旧字符
- `replaceAll(String regex, String replacement)`：使用新字符替换字符串中匹配正则表达式的所有子字符串
- `split(String regex)`：将字符串拆分为子字符串数组，使用正则表达式分隔
- `isEmpty`
- `startsWith`

## StringBuffer & StringBuilder

当对字符串进行修改的时候，需要使用 StringBuffer 和 StringBuilder 类。
和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。

> StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。

> 但由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。

常用的方法
- `append(str)`：将指定字符串添加到此字符序列的末尾
- `capacity()`：返回当前容量
- `charAt(index)`：返回指定索引处的字符
- `delete(start, end)`：删除此序列的子字符串中的字符
- `insert(offset, str)`：将字符串插入此字符序列中
- `length()`：返回长度（字符数）
- `replace(start, end, str)`：使用新字符串替换此序列的子字符串中的字符
- `reverse()`：将此字符序列用其反转形式取代
- `substring(start)`：返回一个新的字符串，该字符串是此字符串的子字符串
- `substring(start, end)`：返回一个新字符串，该字符串是此字符串的子字符串

## Array 类

Java 语言中提供的数组是用来存储固定大小的同类型元素。

数组的元素类型和数组的大小都是确定的，所以当处理数组元素时候，我们通常使用基本循环或者 For-Each 循环。

```
for (int i = 0; i < myList.length; i++) { 
	System.out.println(myList[i] + " "); 
}

for(type element: array)
{
    System.out.println(element);
}
```



常用方法：
- `clone()`：复制数组
- `length`：返回数组的长度
- `equals(Object[] obj)`：比较数组是否相等
- `fill(Object[] a, Object val)`：使用指定的值填充整个数组
- `sort(Object[] a)`：对数组进行排序
- `binarySearch(Object[] a, Object key)`：在数组中查找指定元素并返回其索引
- `toString()`：返回包含数组元素的字符串表示形式

## 方法

方法的一般定义方式为

```
修饰符 返回值类型 方法名(参数类型 参数名){
    ...
    方法体
    ...
    return 返回值;
}


// 例如
public static int max(int num1, int num2) {
	int result;

	if (num1 > num2)
		result = num1;
	else
		result = num2；
	return result;
}
```


**方法的重载**

### 方法参数的作用域

方法内定义的变量被称为局部变量。
局部变量的作用范围从声明开始，直到包含它的块结束。
局部变量必须声明才可以使用。

你可以在一个方法里，不同的非嵌套块中多次声明一个具有相同的名称局部变量，但你不能在嵌套块内两次声明局部变量。

### 构造方法

- 任何类都有构造方法，如果你没有显式的声明构造函数，那么 java 会自动给你提供一个
- 构造方法没有返回值
- 当一个对象被创建时候，构造方法用来初始化该对象。构造方法和它所在类的名字相同

### 可变参数

JDK 1.5 开始，Java 支持传递同类型的可变参数给一个方法。一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。任何普通的参数必须在它之前声明。

```
class VarargsDemo {
	public static void main() {
		printMax(1, 2, 3, 4,5)
		printMax(new double[] {1, 2, 3, 4,5})
	}
	public static double printMax（double ...numbers[]） {
		double res = numbers[0];
		for (num in numbers) {
			res = res > num ? res : num
		}
		return res;
	}
}

```

### finalize() 方法

Java 允许定义这样的方法，它在对象被垃圾收集器析构(回收)之前调用，这个方法叫做 finalize( )，它用来清除回收对象。

例如，你可以使用 finalize() 来确保一个对象打开的文件被关闭了。

在 finalize() 方法里，你必须指定在对象销毁时候要执行的操作。

```
class Cake extends Object {  
  private int id;  
  public Cake(int id) {  
    this.id = id;  
    System.out.println("Cake Object " + id + "is created");  
  }  
    
  protected void finalize() throws java.lang.Throwable {  
    super.finalize();  
    System.out.println("Cake Object " + id + "is disposed");  
  }  
}
```

### 流(Stream)、文件(File)和 IO

一个流可以理解为一个数据的序列。输入流表示从一个源读取数据，输出流表示向一个目标写数据。

### 读取控制台输入

```
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

// read() 方法从控制台不断读取字符直到用户输入 q。
public class BRRead {
    public static void main(String[] args) throws IOException {
        char c;
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("输入字符, 按下 'q' 键退出。");
        // 读取字符
        do {
            c = (char) br.read();
            System.out.println(c);
        } while (c != 'q');
    }
}

// 还可以使用  readLine 读取一整行字符
// str = br.readLine();
```

### 文件读写

![[Pasted image 20230907233328.png]]

文件读写的两个重要的流是 **FileInputStream** 和 **FileOutputStream**。

#### InputStream

```
File f = new File("C:/java/hello");
InputStream in = new FileInputStream(f);
```

创建了 InputStream 实例后就可以调用方法去对流进行操作了
- `available()` - 返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取的字节数。返回一个整数值。
- `close()` - 关闭输入流并释放与其关联的所有系统资源。
- `finalize` ： 清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出 IOException 异常。
- `read()` - 从输入流中读取下一个字节的数据。
- `read(byte[] b)` - 从输入流中读取一些字节并将它们存储到缓冲数组 `b` 中。
- `read(byte[] b, int off, int len)` - 从输入流中最多读取 `len` 个字节的数据，并将它们存储到一个字节数组中。
- `skip(long n)` - 跳过并丢弃输入流中的 `n` 个字节的数据。

#### FileOutputStream

```
File f = new File("C:/java/hello");
OutputStream fOut = new FileOutputStream(f);
```

- `close()`：关闭此输出流并释放与此流相关联的所有系统资源。
- `flush()`：刷新此输出流并强制写出所有缓冲的输出字节。
- `write(byte[] b)`：将指定的字节数组写入此输出流。
- `write(byte[] b, int off, int len)`：将指定字节数组中从偏移量 `off` 开始的 `len` 个字节写入此输出流。
- `write(int b)`：将指定的字节写入此输出流。


**示例代码**
```
//文件名 :fileStreamTest2.java
import java.io.*;
 
public class fileStreamTest2 {
    public static void main(String[] args) throws IOException {
 
        File f = new File("a.txt");
        FileOutputStream fop = new FileOutputStream(f);
        // 构建FileOutputStream对象,文件不存在会自动新建
 
        OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");
        // 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbk
 
        writer.append("中文输入");
        // 写入到缓冲区
 
        writer.append("\r\n");
        // 换行
 
        writer.append("English");
        // 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入
 
        writer.close();
        // 关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉
 
        fop.close();
        // 关闭输出流,释放系统资源
 
        FileInputStream fip = new FileInputStream(f);
        // 构建FileInputStream对象
 
        InputStreamReader reader = new InputStreamReader(fip, "UTF-8");
        // 构建InputStreamReader对象,编码与写入相同
 
        StringBuffer sb = new StringBuffer();
        while (reader.ready()) {
            sb.append((char) reader.read());
            // 转成char加到StringBuffer对象中
        }
        System.out.println(sb.toString());
        reader.close();
        // 关闭读取流
 
        fip.close();
        // 关闭输入流,释放系统资源
 
    }
}
```

#### 文件和 I/O

还有一些关于文件和I/O的类，我们也需要知道：

- [File Class(类)](https://www.runoob.com/java/java-file.html)
- [FileReader Class(类)](https://www.runoob.com/java/java-filereader.html)
- [FileWriter Class(类)](https://www.runoob.com/java/java-filewriter.html)

#### 目录

```
import java.io.File;
public class CreateDir {
	public static void main(String[] args) {
		File fileIns = new File('./xxx/xxx');
		fileIns.mkdirs();
	}
}
```

- `createNewFile()`：创建一个新文件。
- `delete()`：删除此抽象路径名表示的文件或目录。
- `exists()`：测试此抽象路径名表示的文件或目录是否存在。
- `getAbsolutePath()`：返回此抽象路径名的绝对路径名字符串。
- `getName()`：返回由此抽象路径名表示的文件或目录的名称。
- `getParent()`：返回此抽象路径名父目录的路径名字符串；如果此路径名没有指定父目录，则返回 null。
- `getPath()`：将此抽象路径名转换为路径名字符串。
- `isDirectory()`：测试此抽象路径名表示的文件是否是一个目录。
- `isFile()`：测试此抽象路径名表示的文件是否是一个标准文件。
- `list()`：返回一个字符串数组，命名由此抽象路径名表示的目录中的文件和目录。
- `mkdir()`：创建此抽象路径名指定的目录。
-  `mkdirs()`：创建此抽象路径名指定的目录及其父目录
- `renameTo(File dest)`：重命名此抽象路径名表示的文件。

### 异常处理

异常发生的原因有很多，通常包含以下几大类：
- 用户输入了非法数据。
- 要打开的文件不存在。
- 网络通信时连接中断，或者 JVM 内存溢出。

要理解 Java 异常处理是如何工作的，你需要掌握以下三种类型的异常：
- **检查性异常**：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
- **运行时异常：** 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
- **错误：** 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。




```
sudo mkdir -p /opt/settings/ && sudo touch /opt/settings/server.properties && sudo chmod 777 /opt/settings/server.properties && echo -e "env=DEV\napollo.meta=http://configserver.internal.dev.supermonkey.com.cn"\nidc=default > /opt/settings/server.properties
```
