*参考：*

> http://c.biancheng.net/view/939.html
>
> https://www.cnblogs.com/zsql/p/11115244.html

## 概念

在 Java 中，**一切皆对象**，对象就是面向对象程序设计的**核心**。对象与现实世界的实体是一一对应的，是对实体的描述和概括。对象有以下特点：

- 对象具有属性和行为
- 对象具有变化的状态
- 对象具有唯一性
- 对象都是某个类的实例
- 一切皆为对象，真实世界中的所有事物都可以视为对象



## 面向对象三大特性

### 封装性

封装是将代码及其处理的数据绑定在一起的一种编程机制，该机制保证了程序和数据都不受外部干扰且不被误用，**封装的目的在于保护信息**。例如在 Java 中的类就是基本封装单位，封装其内部复杂的机制。

- 优点：模块化，代码复用，信息隐藏
- 缺点：会影响运行效率



### 继承性

继承性是指子类拥有父类的**非`private`**属性和方法，这是类之间的一种关系。Java 只支持**单继承**。



### 多态性

多态性体现在父类中定义的属性和方法被子类继承后，可以具有**不同的**属性或表现方式。





## 类

类表示了一个客观世界某类群体的一些基本特征抽象，即**类是对象的抽象**。

### 普通类

```java
class Person {
    private String name;
}
```



### 抽象类

如果一个类中没有包含足够的信息来描述一个具体的对象，就可以定义为抽象类，而后通过继承它的类来完善描述。

抽象类无法实例化，而其他功能与普通类相同。抽象类相比于接口来说，好处在于子类可以**按需**实现/重写自己的方法。

```java
abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public void speak() {
        System.out.println(name + "说话：");
    }

    public void eat() {
        System.out.println(name + "吃饭");
    }
}

class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void speak() {
        super.speak();
        System.out.println("汪汪汪");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal dog = new Dog("狗子");
        dog.speak();
    }
}
```

运行结果：

```
狗子说话：
汪汪汪
```



### 内部类

*参考：*

> https://www.cnblogs.com/dolphin0520/p/3811445.html

定义在类（外部类）中或者一个方法中的类，就是内部类。

#### 成员内部类

定义在一个类中的类即成员内部类，成员内部类可以无条件访问外部类的所有成员属性和成员方法，例如：

```java
class OuterClass {
    static int status = 0;
    private String name;

    {
        name = "outer";
    }

    public void show() {
        System.out.println(status);
    }

    class InnerClass {
        String name = "inner";

        public void show() {
            OuterClass.this.show();
            System.out.println(OuterClass.this.name);
            System.out.println(name);
        }
    }
}

public class ClassTest {
    public static void main(String[] args) {
        new OuterClass().new InnerClass().show();
    }
}
```

运行结果：

```
0
outer
inner
```

---



**使用内部类可以实现多继承的效果**：

```java
class Phone {
    public String show() {
        return "手机";
    }
}

class Pad {
    public String show() {
        return "平板";
    }
}

class Machine {
    class phone extends Phone {
        @Override
        public String show() {
            return super.show();
        }
    }

    class pad extends Pad {
        @Override
        public String show() {
            return super.show();
        }
    }

    public void show() {
        System.out.println("我是" + new phone().show() + "，我也是" + new pad().show());
    }
}

public class Test {
    public static void main(String[] args) {
        Machine machine = new Machine();
        machine.show();
    }
}

```

运行结果：

```
我是手机，我也是平板
```



#### 局部内部类

局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内。

```java
class Outer {
    private int value = 10;

    public void method() {
        int value = 20;
        class Inner {
            public void show() {
                System.out.println(Outer.this.value);
                System.out.println(value);
            }
        }

        new Inner().show();
    }
}

public class Multiple {
    public static void main(String[] args) {
        new Outer().method();
    }
}
```

运行结果：

```
10
20
```



#### 匿名内部类

通过继承或者实现接口的方式创建的没有名字的内部类就是匿名内部类，例如：

```java
interface Inner {
    void show();
}

class Outer {
    private int age = 18;

    public void method() {
        new Inner() {
            @Override
            public void show() {
                System.out.println(age);
            }
        }.show();
    }
}

public class Test {
    public static void main(String[] args) {
        new Outer().method();
    }
}
```

运行结果：

```
18
```



#### 静态内部类

虽然`static`不能用来修饰类，但是内部类可以看作为外部类的一个成员，因此可以用`static`修饰，其只能访问外部类的静态成员，例如：

```java
class Outer {
    private static int value = 10;

    static class Inner {
        public static void show() {
            System.out.println(value);
        }
    }
}

public class Multiple {
    public static void main(String[] args) {
        Outer.Inner.show();
    }
}
```

运行结果：

```
10
```

