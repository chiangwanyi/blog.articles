## 1. 基本数据类型

基本类型，或者叫做内置类型，共有 4 类 8 种：

| 类型         | 关键字  | 所占字节 | 取值范围                             | 默认值   |
| ------------ | ------- | -------- | ------------------------------------ | -------- |
| 布尔型       | boolean | 1        | true, false                          | false    |
| 字符型       | char    | 2        |                                      | '\u0000' |
| 字节型       | byte    | 1        | -128 ~ 127                           | 0        |
| 短整型       | short   | 2        | -2<sup>15</sup> ~ 2<sup>15</sup>-1   | 0        |
| 整型         | int     | 4        | -2<sup>31</sup> ~ 2<sup>31</sup> -1  | 0        |
| 长整型       | long    | 8        | -2<sup>63</sup> ~ 2<sup>63</sup> - 1 | 0        |
| 单精度浮点型 | float   | 4        |                                      | 0.0F     |
| 双精度浮点型 | double  | 8        |                                      | 0.0D     |

## 2. String，StringBuffer与StringBuilder的区别

### 2.1 String

String 是不可变的对象, 因此在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象。

### 2.2 StringBuffer

> aklsdhfkasdf
>
> asdfasdfasdf
>
> asdfasdfasdfasdf你是地方

`StringBuffer`是**线程安全**的，常用于字符串拼接，某些特别情况下， String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffer 对象的拼接，所以这些时候 String 对象的速度并不会比 StringBuffer 对象慢，而特别是以下的字符串对象生成中， String 效率是远要比 StringBuffer 快的：

```java
String str1 = "A" + "B" + "C";
StringBuffer str2 = new StringBuffer("A").append("B").append("C");
```

`str1`在被JVM解析时，会直接解析为：

```java
String str1 = "ABC";
```



### 2.3 StringBuilder

StringBuilder 是**线程不安全**的，类似于 StringBuffer 的简化替换（API 与 StringBuffer 相同），比 StringBuffer 快。



## 3. 文件读写

### 3.1 字节流

字节流处理单元为 1 个字节，操作字节和字节数组

- 读写**文本文件**——按固定字节数组

```java
FileInputStream in = new FileInputStream("src\\in.txt");
FileOutputStream out = new FileOutputStream("src\\out.txt");

byte[] bytes = new byte[1024];
int ch = -1;

while ((ch = in.read(bytes, 0, bytes.length)) != -1) {
    System.out.print(new String(bytes, 0, ch, StandardCharsets.UTF_8));
    out.write(bytes, 0, ch);
}

in.close();
out.close();
```

- 读写**二进制文件**-按固定字节数组

```java
FileInputStream in = new FileInputStream("src\\in.png");
FileOutputStream out = new FileOutputStream("src\\out.png");

byte[] bytes = new byte[1024];
int ch = -1;

while ((ch = in.read(bytes, 0, bytes.length)) != -1) {
    out.write(bytes, 0, ch);
}

in.close();
out.close();
```



### 3.2 缓存字节流

类似于**字节流**，效率更高。

```java
BufferedInputStream in = new BufferedInputStream(new FileInputStream("src\\in.txt"));
BufferedOutputStream out = new BufferedOutputStream(new FileOutputStream("src\\out.txt"));

byte[] bytes = new byte[1024];
int ch = -1;

while ((ch = in.read(bytes, 0, bytes.length)) != -1) {
    out.write(bytes, 0, ch);
}

out.flush();
in.close();
out.close();
```



### 3.3 字符流

字符流处理的单元为 2 个字节的 Unicode 字符，分别操作字符、字符数组或字符串，不能够用来读写**二进制文件**

```java
InputStreamReader in = new InputStreamReader(new FileInputStream("src\\in.txt"), StandardCharsets.UTF_8);
OutputStreamWriter out = new OutputStreamWriter(new FileOutputStream("src\\out.txt"), StandardCharsets.UTF_8);

char[] chars = new char[1024];

while ((in.read(chars, 0, chars.length)) != -1){
    out.write(chars);
}

out.close();
in.close();
```



### 3.4 缓存字符流

可以使用`readLine`读取一行。

```java
BufferedReader in = new BufferedReader(new InputStreamReader(new FileInputStream("src\\in.txt"), StandardCharsets.UTF_8));
BufferedWriter out = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("src\\out.txt"), StandardCharsets.UTF_8));

String str = null;

while ((str = in.readLine()) != null){
    out.write(str);
    out.newLine();
}

out.flush();
out.close();
in.close();
```

## 4. 反射

Java 的反射（reflection）机制是指在程序的运行状态中，可以构造任意一个类的对象，可以了解任意一个对象所属的类，可以了解任意一个类的成员变量和方法，可以调用任意一个对象的属性和方法。

反射机制运用在很多方面：

- JDBC 中动态加载了数据库驱动程序
- Spring 中的依赖注入
- 读取配置文件



定义一个`Person`类：

```java
package Reflection;

public class Person {
    private String name;
    private int age = 0;
    public String role = "admin";

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public void hi() {
        System.out.println("Hello");
    }

    public int test(int arg) {
        return arg * arg;
    }
}
```



### 4.1 反射创建对象

```java
Class<?> c = Class.forName("Reflection.Person");
Person person = (Person) c.newInstance();
System.out.println(person);
```

输出结果：

```
Person{name='null', age=0}
```



### 4.2 反射构造方法创建对象

```java
Class<?> c = Class.forName("Reflection.Person");
Constructor<?> constructor = c.getDeclaredConstructor(String.class, int.class);
Person person = (Person) constructor.newInstance("admin", 1);
System.out.println(person);
```

输出结果：

```
Person{name='admin', age=1}
```



### 4.3 反射获取类属性

```java
Class<?> c = Class.forName("Reflection.Person");
Field role = c.getField("role");
Field age = c.getDeclaredField("age");
Person person = (Person) c.newInstance();

System.out.println(role.get(person));

// age 是私有属性，需要设置权限为 true
age.setAccessible(true);
System.out.println(age.get(person));
```

输出结果：

```
admin
0
```



### 4.4 反射调用方法

```java
Class<?> c = Class.forName("Reflection.Person");
Object instance = c.newInstance();
Method hi = c.getMethod("hi");
Method test = c.getMethod("test", int.class);

System.out.println(hi);
System.out.println(test);
hi.invoke(instance);
System.out.println(test.invoke(instance, 4));
```

输出结果：

```
public void Reflection.Person.hi()
public int Reflection.Person.test(int)
Hello
16
```
