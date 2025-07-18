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
- `isLetter(char ch)`
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

## 日期

略

## 正则表达式

略

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

没有 find 和 findIndex 方法，额外的实现
```
import java.util.Arrays;
import java.util.Optional;

public class Main {
    public static void main(String[] args) {
        String[] array = {"apple", "banana", "cherry"};

        // 类似 JS 的 find：返回第一个匹配的元素
        Optional<String> found = Arrays.stream(array)
                .filter(s -> s.startsWith("b"))
                .findFirst();
        System.out.println(found.orElse("Not found")); // 输出 "banana"

        // 类似 JS 的 findIndex：返回第一个匹配的索引
        int index = IntStream.range(0, array.length)
                .filter(i -> array[i].startsWith("b"))
                .findFirst()
                .orElse(-1); // 未找到时返回 -1
        System.out.println(index); // 输出 1
    }
}
```

## 条件、循环、while 等写法

```
public class ControlStructuresDemo {
    public static void main(String[] args) {
        // ========== 条件语句 ==========
        // 1. 简单的 if 语句
        int age = 18;
        if (age >= 18) {
            System.out.println("你已经成年了");
        }

        // 2. if-else 语句
        int score = 85;
        if (score >= 60) {
            System.out.println("及格");
        } else {
            System.out.println("不及格");
        }

        // 3. 多条件 else-if
        int temperature = 25;
        if (temperature > 30) {
            System.out.println("天气炎热");
        } else if (temperature > 20) {
            System.out.println("天气温暖");
        } else if (temperature > 10) {
            System.out.println("天气凉爽");
        } else {
            System.out.println("天气寒冷");
        }

        // ========== switch case 语句 ==========
        // 1. 传统 switch 语句
        int dayOfWeek = 3;
        switch (dayOfWeek) {
            case 1:
                System.out.println("星期一");
                break;
            case 2:
                System.out.println("星期二");
                break;
            case 3:
                System.out.println("星期三");
                break;
            case 4:
                System.out.println("星期四");
                break;
            case 5:
                System.out.println("星期五");
                break;
            case 6:
                System.out.println("星期六");
                break;
            case 7:
                System.out.println("星期日");
                break;
            default:
                System.out.println("无效的星期");
        }

        // 2. Java 12+ 的增强 switch 表达式
        String season = "Spring";
        String description = switch (season) {
            case "Spring" -> "春暖花开";
            case "Summer" -> "夏日炎炎";
            case "Autumn" -> "秋高气爽";
            case "Winter" -> "寒冬腊月";
            default -> "未知季节";
        };
        System.out.println(description);

        // ========== 循环语句 ==========
        // 1. for 循环
        System.out.println("for 循环示例:");
        for (int i = 0; i < 5; i++) {
            System.out.println("i = " + i);
        }

        // 2. 增强 for 循环 (for-each)
        System.out.println("for-each 循环示例:");
        int[] numbers = {1, 2, 3, 4, 5};
        for (int num : numbers) {
            System.out.println("数字: " + num);
        }

        // 3. while 循环
        System.out.println("while 循环示例:");
        int count = 0;
        while (count < 3) {
            System.out.println("count = " + count);
            count++;
        }

        // 4. do-while 循环
        System.out.println("do-while 循环示例:");
        int x = 5;
        do {
            System.out.println("x = " + x);
            x--;
        } while (x > 0);

        // 5. 循环控制语句
        System.out.println("循环控制示例:");
        for (int i = 0; i < 10; i++) {
            if (i == 3) {
                continue; // 跳过本次循环
            }
            if (i == 7) {
                break; // 终止循环
            }
            System.out.println("控制循环: i = " + i);
        }

        // 6. 嵌套循环
        System.out.println("嵌套循环示例:");
        for (int i = 1; i <= 3; i++) {
            for (int j = 1; j <= 3; j++) {
                System.out.println("i = " + i + ", j = " + j);
            }
        }

        // 7. 标签循环 (很少使用)
        System.out.println("标签循环示例:");
        outerLoop:
        for (int i = 0; i < 5; i++) {
            innerLoop:
            for (int j = 0; j < 5; j++) {
                if (i == 2 && j == 2) {
                    break outerLoop; // 跳出外层循环
                }
                System.out.println("i = " + i + ", j = " + j);
            }
        }
    }
}
```

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

# 面向对象

## 继承

继承关键字：
- extends
- implements
- super 和 this
- final
	- 使用 final 关键字声明类，就是把类定义定义为最终类，不能被继承，或者用于修饰方法，该方法不能被子类重写

## 重写(Override)与重载(Overload)

### 重写(Override)

重写（Override）是指子类定义了一个与其父类中具有相同名称、参数列表和返回类型的方法，并且子类方法的实现覆盖了父类方法的实现。 **即外壳不变，核心重写！**

### 重载(Overload)

重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。

每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

## Java 多态

多态是同一个行为具有多个不同表现形式或形态的能力。

### 多态存在的三个必要条件

- 继承
- 重写
- 父类引用指向子类对象：Parent p = new Child();

### 多态的形式

#### 两种重要的多态

##### 1. 编译时多态（静态多态/方法重载）

```java
class Calculator {
    // 方法重载 - 同名方法，参数不同
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public String add(String a, String b) {
        return a + b;
    }
}

// 使用
Calculator calc = new Calculator();
System.out.println(calc.add(1, 2));       // 调用int版本
System.out.println(calc.add(1.5, 2.5));   // 调用double版本
System.out.println(calc.add("Hello", "World")); // 调用String版本
```

##### 2. 运行时多态（动态多态/方法重写）

```java
class Animal {
    public void makeSound() {
        System.out.println("动物发出声音");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("汪汪汪");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("喵喵喵");
    }
}

// 使用
Animal myAnimal = new Animal(); // Animal对象
Animal myDog = new Dog();       // Dog对象，向上转型
Animal myCat = new Cat();       // Cat对象，向上转型

myAnimal.makeSound(); // 输出"动物发出声音"
myDog.makeSound();    // 输出"汪汪汪" - 运行时确定
myCat.makeSound();    // 输出"喵喵喵" - 运行时确定
```

#### 多态的高级实现方式

##### 1. 接口多态

```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("绘制圆形");
    }
}

class Square implements Shape {
    @Override
    public void draw() {
        System.out.println("绘制方形");
    }
}

// 使用
Shape shape = new Circle(); // 接口引用指向实现类对象
shape.draw(); // 输出"绘制圆形"

shape = new Square();
shape.draw(); // 输出"绘制方形"
```

##### 2. 抽象类多态

```java
abstract class Vehicle {
    abstract void run();
}

class Car extends Vehicle {
    @Override
    void run() {
        System.out.println("汽车行驶");
    }
}

class Bike extends Vehicle {
    @Override
    void run() {
        System.out.println("自行车骑行");
    }
}

// 使用
Vehicle vehicle = new Car();
vehicle.run(); // 输出"汽车行驶"

vehicle = new Bike();
vehicle.run(); // 输出"自行车骑行"
```

##### 3. 多态参数（方法参数使用父类/接口类型）

```java
class Zoo {
    public void animalSound(Animal animal) {
        animal.makeSound(); // 根据实际传入对象调用相应方法
    }
}

// 使用
Zoo zoo = new Zoo();
zoo.animalSound(new Dog()); // 输出"汪汪汪"
zoo.animalSound(new Cat()); // 输出"喵喵喵"
```

## 抽象类

略

## 封装

略

## 数据结构

### 集合框架 (Collection Framework)

#### 1. List 链表 (有序、可重复)

##### ArrayList

```java
List<String> arrayList = new ArrayList<>();
arrayList.add("A");
arrayList.add("B");
arrayList.get(0); // 快速随机访问
```

- ​**特点**​：动态数组，非同步，查询快(O(1))，增删慢(O(n))
- ​**适用场景**​：频繁访问元素，元素数量变化不大

##### LinkedList

```java
List<String> linkedList = new LinkedList<>();
linkedList.add("A");
linkedList.addFirst("B"); // 添加到头部
linkedList.getLast(); // 获取最后一个元素
```

- ​**特点**​：双向链表，非同步，增删快(O(1))，查询慢(O(n))
- ​**适用场景**​：频繁在头尾增删元素

##### Vector

```java
List<String> vector = new Vector<>();
vector.add("A"); // 同步方法
```

- ​**特点**​：线程安全的动态数组，性能较差
- ​**替代方案**​：`Collections.synchronizedList(new ArrayList<>())`

##### Stack 栈 (继承自Vector)

```java
Stack<String> stack = new Stack<>();
stack.push("A");
stack.pop();
```

- ​**特点**​：后进先出(LIFO)
- ​**推荐替代**​：`Deque<Integer> stack = new ArrayDeque<>()`

#### 2. Set 哈希表 (无序、唯一)

##### HashSet

```java
Set<String> hashSet = new HashSet<>();
hashSet.add("A");
hashSet.contains("A"); // 快速查找
```

- ​**特点**​：基于HashMap实现，无序，允许null
- ​**底层**​：哈希表，查找O(1)

##### LinkedHashSet

```java
Set<String> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add("A");
linkedHashSet.add("B"); // 保持插入顺序
```

- ​**特点**​：维护插入顺序的HashSet
- ​**适用场景**​：需要保持插入顺序的唯一集合

##### TreeSet

```java
Set<String> treeSet = new TreeSet<>();
treeSet.add("B");
treeSet.add("A"); // 自动排序
```

- ​**特点**​：基于TreeMap实现，自然排序或Comparator
- ​**底层**​：红黑树，查找O(log n)

#### 3. Queue/Deque (队列)

##### PriorityQueue

```java
Queue<Integer> pq = new PriorityQueue<>();
pq.offer(3);
pq.poll(); // 取出最小元素
```

- ​**特点**​：优先级队列，无界，非同步
- ​**底层**​：最小堆实现

##### ArrayDeque

```java
Deque<String> deque = new ArrayDeque<>();
deque.offerFirst("A"); // 头部添加
deque.pollLast(); // 尾部移除
```

- ​**特点**​：双端队列，比LinkedList更高效
- ​**适用场景**​：栈和队列操作

### Map 框架 (键值对)

#### HashMap

```java
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("key", 1);
hashMap.get("key");
```

- ​**特点**​：基于哈希表，无序，允许null键/值
- ​**Java 8改进**​：链表转红黑树(当链表长度>8)

#### LinkedHashMap

```java
Map<String, Integer> linkedHashMap = new LinkedHashMap<>();
linkedHashMap.put("A", 1);
linkedHashMap.put("B", 2); // 保持插入顺序
```

- ​**特点**​：维护插入顺序或访问顺序
- ​**应用场景**​：LRU缓存实现

#### TreeMap

```java
Map<String, Integer> treeMap = new TreeMap<>();
treeMap.put("B", 2);
treeMap.put("A", 1); // 按键排序
```

- ​**特点**​：基于红黑树，按键排序
- ​**查找效率**​：O(log n)

#### Hashtable

```java
Map<String, Integer> hashtable = new Hashtable<>();
hashtable.put("A", 1); // 同步方法
```

- ​**特点**​：线程安全但性能差
- ​**替代方案**​：`ConcurrentHashMap`

#### ConcurrentHashMap

```java
Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("A", 1); // 分段锁实现
```

- ​**特点**​：高并发优化，线程安全
- ​**Java 8改进**​：改用CAS+synchronized

## Object

Java Object 类是所有类的父类，也就是说 Java 的所有类都继承了 Object，**子类可以使用 Object 的所有方法**。

- `toString()`
    返回对象的字符串表示形式，默认实现返回"类名@哈希码"的字符串

- `equals(Object obj)`
    比较两个对象是否"相等"，默认实现是比较对象引用

- `hashCode()`
    返回对象的哈希码值，用于哈希表数据结构(如HashMap、HashSet)

- `getClass()`
    返回对象的运行时类(Class对象)，包含该类的元数据信息

- `clone()`
    创建并返回当前对象的一个副本，需要实现Cloneable接口才能使用

- `finalize()`
    当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用(已弃用)

## Java 多线程编程

Java 多线程编程是指使用 Java 语言创建和管理多个线程，使程序能够同时执行多个任务的编程技术。线程是操作系统能够进行运算调度的最小单位，它被包含在进程之中，是进程中的实际运作单位。

### 多线程的基本概念

1. ​**线程(Thread)​**​：程序执行的最小单元，一个进程可以包含多个线程
2. ​**主线程**​：Java 程序启动时自动创建的线程
3. ​**并发(Concurrency)​**​：多个任务在单核CPU上交替执行
4. ​**并行(Parallelism)​**​：多个任务在多核CPU上同时执行

### Java 多线程的实现方式

#### 1. 继承 Thread 类

```java
class MyThread extends Thread {
    @Override
    public void run() {
        // 线程执行的代码
        System.out.println("线程运行中: " + Thread.currentThread().getName());
    }
}

// 使用
MyThread thread = new MyThread();
thread.start(); // 启动线程
```

#### 2. 实现 Runnable 接口

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("线程运行中: " + Thread.currentThread().getName());
    }
}

// 使用
Thread thread = new Thread(new MyRunnable());
thread.start();
```

#### 3. 实现 Callable 接口（可返回结果）

```java
class MyCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        return "线程执行结果: " + Thread.currentThread().getName();
    }
}

// 使用
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(new MyCallable());
System.out.println(future.get()); // 获取返回结果
executor.shutdown();
```

#### 4. 使用线程池（推荐方式）

```java
ExecutorService executor = Executors.newFixedThreadPool(5);
for (int i = 0; i < 10; i++) {
    executor.execute(() -> {
        System.out.println("线程执行: " + Thread.currentThread().getName());
    });
}
executor.shutdown();
```

### 线程的生命周期

1. ​**新建(NEW)​**​：线程对象被创建但未启动
2. ​**就绪(RUNNABLE)​**​：调用 start() 后，等待CPU调度
3. ​**运行(RUNNING)​**​：获得CPU时间片，执行 run() 方法
4. ​**阻塞(BLOCKED/WAITING/TIMED_WAITING)​**​：线程暂停执行
5. ​**终止(TERMINATED)​**​：线程执行完毕或异常终止

### 线程同步与通信

#### 1. synchronized 关键字

```java
// 同步方法
public synchronized void increment() {
    count++;
}

// 同步代码块
public void add(int value) {
    synchronized(this) {
        count += value;
    }
}
```

#### 2. Lock 接口

```java
Lock lock = new ReentrantLock();

public void update() {
    lock.lock();
    try {
        // 临界区代码
    } finally {
        lock.unlock();
    }
}
```

#### 3. 线程间通信

```java
// wait() 和 notify() 示例
class SharedResource {
    private boolean available = false;
    
    public synchronized void produce() {
        while(available) {
            wait(); // 等待消费者消费
        }
        available = true;
        notify(); // 通知消费者
    }
    
    public synchronized void consume() {
        while(!available) {
            wait(); // 等待生产者生产
        }
        available = false;
        notify(); // 通知生产者
    }
}
```

### 多线程的应用场景

1. ​**提高程序响应性**​：GUI应用中的耗时操作放在后台线程
2. ​**提高CPU利用率**​：多核CPU上的并行计算
3. ​**异步任务处理**​：服务器处理多个客户端请求
4. ​**定时任务执行**​：使用 ScheduledExecutorService
5. ​**批处理任务**​：大数据处理、文件批量操作

## 注解

注解（Annotation）是 Java 5 引入的一种元数据形式，它提供了一种向代码中添加结构化信息的方式。注解本身不会直接影响代码逻辑，但可以被编译器、开发工具或运行时环境读取和处理。

### 注解的基本语法

```java
// 定义注解
public @interface MyAnnotation {
    String value() default "default";
    int count() default 1;
}

// 使用注解
@MyAnnotation(value = "example", count = 5)
public class MyClass {
    // ...
}
```

### Java 内置的标准注解

#### 1. 编译时注解

- `@Override`：表示方法覆盖父类方法
- `@Deprecated`：表示元素已过时
- `@SuppressWarnings`：抑制编译器警告
- `@SafeVarargs`：表示方法对可变参数的使用是安全的
- `@FunctionalInterface`：表示接口是函数式接口

#### 2. 元注解（用于定义注解的注解）

- `@Retention`：指定注解的保留策略
    - `RetentionPolicy.SOURCE`：仅保留在源码中
    - `RetentionPolicy.CLASS`：保留在class文件中（默认）
    - `RetentionPolicy.RUNTIME`：运行时可通过反射获取
- `@Target`：指定注解可以应用的元素类型
    - `ElementType.TYPE`：类、接口、枚举
    - `ElementType.FIELD`：字段
    - `ElementType.METHOD`：方法
    - `ElementType.PARAMETER`：参数
    - `ElementType.CONSTRUCTOR`：构造器
    - `ElementType.LOCAL_VARIABLE`：局部变量
    - `ElementType.ANNOTATION_TYPE`：注解类型
    - `ElementType.PACKAGE`：包
    - `ElementType.TYPE_PARAMETER`：类型参数（Java 8）
    - `ElementType.TYPE_USE`：类型使用（Java 8）
- `@Documented`：表示注解应包含在Javadoc中
- `@Inherited`：表示注解可被子类继承
- `@Repeatable`：表示注解可重复使用（Java 8）

### 注解的处理方式

#### 1. 编译时处理（APT）

```java
// 定义处理器
@SupportedAnnotationTypes("com.example.MyAnnotation")
@SupportedSourceVersion(SourceVersion.RELEASE_8)
public class MyAnnotationProcessor extends AbstractProcessor {
    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        // 处理注解逻辑
        return true;
    }
}
```

#### 2. 运行时处理（反射）

```java
// 获取类上的注解
MyAnnotation annotation = MyClass.class.getAnnotation(MyAnnotation.class);
if (annotation != null) {
    System.out.println(annotation.value());
    System.out.println(annotation.count());
}

// 获取方法上的注解
Method method = MyClass.class.getMethod("someMethod");
MyAnnotation methodAnnotation = method.getAnnotation(MyAnnotation.class);
```

### 常见框架中的注解

#### Spring 框架

- `@Component`：标识为Spring组件
- `@Service`：标识为服务层组件
- `@Repository`：标识为数据访问组件
- `@Controller`/@`RestController`：标识为控制器
- `@Autowired`：自动注入依赖
- `@RequestMapping`：映射Web请求

### 自定义注解示例

```java
// 定义注解
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface LogExecutionTime {
    String description() default "";
}

// 使用注解
public class BusinessService {
    @LogExecutionTime(description = "耗时操作")
    public void performLongRunningTask() throws InterruptedException {
        Thread.sleep(1000);
    }
}

// 处理注解（使用AOP）
@Aspect
@Component
public class LogExecutionTimeAspect {
    @Around("@annotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        
        Object proceed = joinPoint.proceed();
        
        long executionTime = System.currentTimeMillis() - start;
        
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        return proceed;
    }
}
```

### 注解的优势

1. ​**简化配置**​：替代XML配置，使配置更直观
2. ​**减少样板代码**​：如Lombok注解自动生成代码
3. ​**提高可读性**​：直接在代码中表达意图
4. ​**编译时检查**​：可以在编译时发现问题
5. ​**运行时处理**​：框架可以利用注解实现各种功能

### 注意事项

1. 过度使用注解可能会使代码难以理解
2. 注解处理可能会增加编译时间
3. 运行时注解会影响性能（反射操作）
4. 某些注解是框架特定的，可能造成技术锁定

## Java 8 新特性

- **Lambda 表达式** − Lambda 允许把函数作为一个方法的参数（函数作为参数传递到方法中）。
- **方法引用** − 方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。
- **函数式接口**
	- 函数式接口(Functional Interface)就是一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。函数式接口可以被隐式转换为 lambda 表达式。
- **默认方法**
	- 简单说，默认方法就是接口可以有实现方法，而且不需要实现类去实现其方法。我们只需在方法名前面加个 default 关键字即可实现默认方法。
- **新工具** − 新的编译工具，如：Nashorn引擎 jjs、 类依赖分析器jdeps。
- **Stream API** −新添加的Stream API（java.util.stream） ，以一种声明的方式处理数据，把真正的函数式编程风格引入到Java中。
- **Date Time API** − 加强对日期与时间的处理。
- **Optional 类** − Optional 类已经成为 Java 8 类库的一部分，用来解决空指针异常。

### Lambda 表达式

Lambda 表达式本质上是一个**匿名函数**，它：

- 没有名称
- 不需要属于任何类
- 可以作为参数传递给方法或存储在变量中

语法的组成：
1. ​**参数列表**​：与方法的参数列表相同
2. ​**箭头符号**​：`->`，将参数与Lambda主体分开
3. ​**Lambda主体**​：可以是一个表达式或代码块

#### 常见形式示例

1. ​**无参数**​：

```java
() -> System.out.println("Hello Lambda")
```
2. ​**一个参数**​（可省略括号）：
```java
x -> x * x
```

3. ​**多个参数**​：

```java
(int x, int y) -> x + y
```

4. ​**带代码块**​：

```java
(String s) -> {
    System.out.println(s);
    return s.length();
}
```

#### 函数式接口

Lambda 表达式需要与**函数式接口**配合使用。函数式接口是指：

- 只有一个抽象方法的接口
- 可以用 `@FunctionalInterface` 注解标记

##### 常见函数式接口

| 接口              | 方法                  | 描述              |
| --------------- | ------------------- | --------------- |
| `Supplier<T>`   | `T get()`           | 无参数，返回一个结果      |
| `Consumer<T>`   | `void accept(T t)`  | 接受一个输入参数，无返回值   |
| `Function<T,R>` | `R apply(T t)`      | 接受一个输入参数，返回一个结果 |
| `Predicate<T>`  | `boolean test(T t)` | 接受一个输入参数，返回布尔值  |
| `Runnable`      | `void run()`        | 无参数，无返回值        |

#### 使用场景

##### 1. 替代匿名内部类

```java
// 旧方式
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("传统方式");
    }
}).start();

// Lambda方式
new Thread(() -> System.out.println("Lambda方式")).start();
```

##### 2. 集合遍历

```java
List<String> list = Arrays.asList("a", "b", "c");

// 传统方式
for (String s : list) {
    System.out.println(s);
}

// Lambda方式
list.forEach(s -> System.out.println(s));
```

##### 3. 集合操作（Stream API）

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 计算平方和
int sum = numbers.stream()
                .map(x -> x * x)
                .reduce(0, (a, b) -> a + b);
```

##### 4. 条件过滤

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// 过滤长度大于3的名字
List<String> filtered = names.stream()
                           .filter(name -> name.length() > 3)
                           .collect(Collectors.toList());
```

#### 方法引用（java 8 新特性）

Lambda表达式的一种简化写法，有四种形式：

1. ​**静态方法引用**​：`ClassName::staticMethod`
2. ​**实例方法引用**​：`instance::instanceMethod`
3. ​**特定类型的任意对象方法引用**​：`ClassName::instanceMethod`
4. ​**构造方法引用**​：`ClassName::new`

##### 示例

```java
// 等价于 s -> System.out.println(s)
names.forEach(System.out::println);

// 等价于 s -> s.length()
names.stream().map(String::length);

// 等价于 () -> new ArrayList<>()
Supplier<List<String>> supplier = ArrayList::new;
```

#### 变量捕获

Lambda表达式可以访问：

- 实例变量和静态变量
- 局部变量（必须是final或事实上final）

```java
int num = 10; // 事实上final
Runnable r = () -> System.out.println(num); // 合法

num = 20; // 如果取消注释，会导致编译错误
```

#### 优势

1. ​**代码简洁**​：减少样板代码
2. ​**函数式编程**​：支持将行为作为参数传递
3. ​**并行处理**​：更容易实现并行操作
4. ​**可读性强**​：表达意图更明确
