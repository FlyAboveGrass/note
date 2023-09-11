
### java 的三大版本
1. javaSE。标准版，桌面程序，控制台开发
2. javaME。嵌入式开发
3. javaEE。E 企业级开发，服务器开发



### JDK、JRE、JVM
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

### 变量类型

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

### 修饰符

**访问控制修饰符**
- **default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。
- **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
- **public** : 对所有类可见。使用对象：类、接口、变量、方法
- **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。

### Number & Math 类

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


### Character 类
Character 类用于对单个字符进行操作。
-  `isLetter(char ch)`
- `isDigit(char ch)`
- `isWhitespace(char ch)`
- `isUpperCase(char ch)`
- `isLowerCase(char ch)`
- `toUpperCase(char ch)`
- `toLowerCase(char ch)`
- `toString(char ch)`

 ### ** String 类**

创建字符串的方式对比

> 注意： String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了。如果需要修改，则应该使用 StringBuffer 类。

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


### StringBuffer & StringBuilder

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




### Array 类
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















