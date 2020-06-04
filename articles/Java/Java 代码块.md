> 参考：https://www.cnblogs.com/Qian123/p/5713440.html

## 执行顺序

**静态代码块** > <u>\[存在]</u>（**main 方法**）> <u>\[实例化对象]</u>（**构造代码块** > **构造函数** > <u>\[调用方法]</u>（**普通代码块**））

```java
public class CodeBlock {
    private final String str1 = "不可变成员属性";
    private static String str2 = "静态成员属性";
    private String str3 = "私有成员属性";
    String str4 = "普通成员属性";

    static {
        System.out.println("静态代码块：" + str2);
    }

    {
        System.out.println("构造代码块：" + str3);
    }

    public CodeBlock() {
        System.out.println("无参构造函数：" + str1);
    }

    public void fun1() {
        {
            System.out.println("普通代码块：" + str4);
        }
    }

    public static void fun2() {
        System.out.println("执行了静态方法");
    }

    public static void main(String[] args) {
        System.out.println("执行了 main 方法");
        System.out.println("----- 实例化对象 -----");
        new CodeBlock().fun1();
        System.out.println("----- 再次实例化对象 -----");
        new CodeBlock().fun1();
        System.out.println("----- 调用静态方法 ------");
        CodeBlock.fun2();
    }
}
```

运行结果：

```
静态代码块：静态成员属性
执行了 main 方法
----- 实例化对象 -----
构造代码块：私有成员属性
无参构造函数：不可变成员属性
普通代码块：普通成员属性
----- 再次实例化对象 -----
构造代码块：私有成员属性
无参构造函数：不可变成员属性
普通代码块：普通成员属性
----- 调用静态方法 ------
执行了静态方法
```



## 静态代码块

静态代码块**只能**定义在类中，使用关键字`static`和`{}`声明，格式如下：

```java
public class CodeBlock {
    static {
        System.out.println("静态代码块");
    }
}
```

### 执行时机

1. 类被**加载时**，且只会执行**一次**
2. 多个静态代码块按照**书写顺序**依次执行

### 特点

1. 只能访问**静态成员**
2. 可用在项目启动时**加载配置文件**



## 构造代码块

在类中使用`{}`声明构造代码块，格式如下：

```java
public class CodeBlock {
    {
        System.out.println("构造代码块");
    }
}
```

### 执行时机

1. 对象一建立就运行构造代码块了，而且**优先于**构造函数执行

### 特点

1. 可用于给**所有对象**进行**统一初始化**



## 构造函数

在类中使用`public ClassName() {}`声明一个构造方法，构造方法可以有多种类型（参数）

### 执行时机

1. 创建对象时就会调用**相应的**构造函数



## 普通代码块

在普通方法中使用`{}`定义一个普通代码块

### 执行时机

1. 方法**被调用**时执行

### 特点

1. 同一个方法中的普通代码块之间为**不同的作用域**



## 同步代码块

> 参考：https://www.cnblogs.com/fnlingnzb-learner/p/10335662.html