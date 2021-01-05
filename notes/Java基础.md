# 一、基础类型

基础类型包括（类型/字节数）：

- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64
- boolean/~

基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的复制使用自动装箱与拆箱完成。

```java
Integer x =2;   //装箱
int y =x;       //拆箱
```

## 1.1 缓存池

new Integer(123) 与Integer.valueof(123)的区别在于:

new Integer(123)每次都会新建一个对象，而Integer.valueof(123)会优先使用缓存池中的对象。不存在再新建一个对象

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}  
```

编译器会在自动装箱过程调用valueOf()方法，因此多个值相同切值再缓存池范围内的Integer实例使用自动装箱来创建，那么就会引用相同的对象.

```java
Integer m =123;
Integer n = 123;
System.out.println(m ==n); //true
```

基本类型对应的缓冲池范围如下：

- int：-128~127
- short: -128~127
- byte: all
- boolean: true or false
- char: \u000 ~ \u007f

## 1.2 BigDecimal

由于浮点数存在净度损失，因此在浮点数之间的等值判断中，基本数据类型不能用==来比较，包装数据类型不能用 equals 来判断。

因此需要使用 BigDecimal 来定义浮点数的值，再进行浮点数的运算操作。

```java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("0.9");
BigDecimal x = a.subtract(b); //0.1
```

通过设置**setScale()** 设置保留几位小数以及保留规则。

```java
BigDecimal m = new BigDecimal("1.2345");
BigDecimal n = m.setScale(3,BigDecimal.ROUND_HALF_UP); //5进位
BigDecimal n = m.setScale(3,BigDecimal.ROUND_HALF_DOWN); //5舍弃
BigDecimal n = m.setScale(3,BigDecimal.ROUND_HALF_EVEN);//4舍弃 6进位，5看前一位，偶数舍弃，基数进位
```

注：实例化 BigDecimal 对象时，有限使用入参为 String 的构造方法或 BigDecimal.valueOf(),否则可能造成精度丢失（例如 Bigdecimal(double) 会导致精度丢失）。

```java
double d = 0.123;
BigDecimal bigDecimal = new BigDecimal(d);
System.out.println(bigDecimal); //0.1229999999999999982236431605997495353221893310546875
String s = "0.123";
BigDecimal bigDecimal1 = new BigDecimal(s);
System.out.println(bigDecimal1); //0.123
```

# 二、String

String被声明为 final，因此它不可以被继承。

在 Java 8 中，内部使用 char 数据存储数据，该数组被声明为 final，也就是说 String 不可变。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
```

在 Java 9 之后， String类的实现改用 byte 数组存储字符串，同时使用 coder 来标识使用了哪种编码， byte[] 和 coder 被声明为final， 也就是说 String 同样不可变。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```

注： byte 是字节数据类型 ，是有符号型的，占1 个字节；大小范围为-128—127 。char 是字符数据类型 ，是无符号型的，占2字节(Unicode码 ）；大小范围 是0—65535 ；char是一个16位二进制的 Unicode 字符，JAVA 用 char 来表示一个字符 。 使用byte数组可以减少一半的内存，byte 是一个字节来存储一个 char 字符， char 是使用2个字节来存储一个 char 字符。只有当一个char字符大小超过0xFF时，才会将byte数组变为原来的两倍，用两个字节存储一个char字符。

```java
String str = "Hello World";
byte[] bytes = str.getBytes();
for (int i = 0; i < bytes.length; i++) {
    byte aByte = bytes[i];
    System.out.println(aByte);//97 98 99
}
char[] ch = {'a','b','c'};
for (int i = 0; i < ch.length; i++) {
    char ch1 = ch[i];
    System.out.println(ch1); a b c
}
```

## 2.1 String 不可变的好处

**(1) 可以缓存hash值**

因为String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

**(2) String Pool 的需要**
如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。


**(3) 线程安全**
String 不可变天生具备线程安全，可以在多个线程中安全的使用。

## 2.2 String, StringBuffer and StringBuider

**(1) 可变性**

- String 不可变。
- StringBuffer 和 StringBuider 可变。本质上是一个没添加 final 修饰的 char[],扩容则是拷贝旧数组的字符到新的数组，因此在字符数可预计的情况下指定容量大小以优化性能。

**(2) 线程安全**

- String 不可变，因此是线程安全的。
- StringBuider 不是线程安全的。
- StringBuffer 是线程安全的，内部使用了 synchronized 来同步。


```java
    //StringBuffer 初始数组大小16
    public synchronized StringBuffer append(CharSequence s) {
        toStringCache = null;
        super.append(s);
        return this;
    }
    //StringBuilder 初始数组大小16
   public AbstractStringBuilder append(CharSequence s) {
        if (s == null)
            return appendNull();
        if (s instanceof String)
            return this.append((String)s);
        if (s instanceof AbstractStringBuilder)
            return this.append((AbstractStringBuilder)s);

        return this.append(s, 0, s.length());
    }
```

## 2.3 new String("abc")

使用这种方式一共会创建两个字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。

1. "abc" 属于字符串字面量， 因此在编译时期会在 String Pool 中创建一个字符串对象，指向这个"abc" 字符串字面量
2. 生成 String 对象，并且 String 对象中的数组指向 "abc" 中的数组。

## 2.4 String.intern()

使用 String.intern() 可以保证内容相同的字符串变量引用相同的内存对象。

下面示例中，s1 和 s2 采用 new String() 的方式新建了两个不同的对象，而 s3 是通过 s1.intern（）方法取得一个对象引用，这个方法首先把 s1 引用的对象放到 String Poll（字符串常量池）中,然后返回这个对象引用。 因此 s3 和 s1 引用的好同一个字符串常量池的对象。

```java
String s1 = new String("aaa");
String s2 = new String("aaa");
System.out.println(s1 == s2);           // false
String s3 = s1.intern();
System.out.println(s1.intern() == s3); // true
```

如果采用 "bbb" 这种使用双引号的形式创建字符串实例，会自动地将新建的对象放入 String Poll 中。

```java
String s4 = "bbb";
String s5 = "bbb";
System.out.println(s4 == s5);  // true
```

在 Java 7 之前，字符串常量池呗放在运行时常量池中,它属于永久代。而在 Java 7之后，字符串常量池被放在堆中。这是因为永久代空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。

# 三、参数传递

Java 的参数传递是以值传递的形式传入方法中，而不是传递引用。

Dog dog 的dog 是一个指针，存储的是对象的地址。 在将一个参数传入一个方法时，本质上是将对象的地址以值的方式传递到形参中。

```java
public class Dog {
    String name;

    Dog(String name) {
        this.name = name;
    }

    String getName() {
        return name;
    }

    String getObjectAddress() {
        return super.toString();
    }
}
```

```java
public class PassByValueExample {
    public static void main(String[] args) {
        Dog dog = new Dog("A");
        System.out.println(dog.getObjectAddress()); // Dog@4554617c
        func(dog);
        System.out.println(dog.getObjectAddress()); // Dog@4554617c
        System.out.println(dog.getName());          // A
    }

    private static void func(Dog dog) {
        System.out.println(dog.getObjectAddress()); // Dog@4554617c
        dog = new Dog("B");
        System.out.println(dog.getObjectAddress()); // Dog@74a14482
        System.out.println(dog.getName());          // B
    }
}
```

# 四、继承

## 4.1 访问权限

Java中有三个访问权限修饰符：private、protected和public，不加修饰符为包级可见。

可以对类或类中的成员（字段以及方法） 加上访问修饰符。

- 成员可见标识其他类可以用这个类的实例访问到该成员。
- 类可见标红其他类可以用这个类创建对象。

protected 用于修饰成员，表示在继承体系中成员对于子类可见，但是这个访问修饰符对于类没有意义。

如果子类的方法覆盖了父类的方法，那么子类中该方法的访问级别不允许低于父类的访问级别，这是为了确保可以使用父类实例的地方都可以使用子类实例，也就是确保满足里式替换原则。

字段绝不能是公有的，因为这么做的话就失去了对这个字段修改行为的控制，客户端可以对其随意修改。可以使用公有的getter 和 setter 方法来替换公有字段的使用。


```java
public class AccessExample {
    public int x;
}
```

```java
public class AccessExample {
    private int x;

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }
}
```

但是也有例外，如果是包级私有的类或者私有的嵌套类，那么直接暴露成员不会有特别大的影响。

```java
public class AccessWithInnerClassExample {
    private class InnerClass {
        int x;
    }

    private InnerClass innerClass;

    public AccessWithInnerClassExample() {
        innerClass = new InnerClass();
    }

    public int getValue() {
        return innerClass.x; // 直接访问
    }
}
```

## 4.2 抽象类与接口

### 4.2.1 抽象类

抽象类和抽象方法都使用 abstract 进行声明。抽象类一般会包含抽象方法，抽象方法一定位于抽象类中。
抽象类和普通类最大的区别是，抽象类不能被实例化，需要继承抽象类才能实例化其子类。

### 4.2.2 接口

接口是抽象类的延伸，在 Java 8 之前，它可以看成是一个完全抽象的类，也就是说它不能有任何的方法实现。

从 Java 8 开始，接口也可以拥有默认的方法实现，这样可以大大的减轻维护接口的成本，在 Java 8
之前如果接口中增加一个方法，那么所有实现该接口的类都要进行修改。

接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。
接口的默认字段都是 static 和 final 的。

### 4.2.3 比较

- 从设计层面上看，抽象类提供了一种 IS-A (父子继承) 关系，那就是必须满足里式替换原则，即子类对象必须能够替换掉所有父类对象。而接口更像是一种 LIKE-A (组合) 关系，它只是提供一种方法实现契约，并不要求接口和实现接口的类 具有 IS-A 关系。
- 从使用上看，一个类可以实现多个接口，但是不能继承多个抽象类。
- 接口的字段只能是 static 和 final 类型的, 而抽象类的字段没有这种限制。
- 接口的方法只能是 public 的, 而抽象类的方法可以由多种访问权限。

## 4.3 super

- 访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而完成一些初始化的工作。
- 访问父类的成员： 如果子类覆盖了父类中某个方法的实现，可以使用super 关键字来引用父类方法的实现。

```java
public class SuperExample {
    protected int x;
    protected int y;

    public SuperExample(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void func() {
        System.out.println("SuperExample.func()");
    }
}
```

```java
public class SuperExtendExample extends SuperExample {
    private int z;

    public SuperExtendExample(int x, int y, int z) {
        super(x, y);
        this.z = z;
    }

    @Override
    public void func() {
        super.func();
        System.out.println("SuperExtendExample.func()");
    }
}
```

```java
SuperExample e = new SuperExtendExample(1, 2, 3);
e.func();
```

```html
SuperExample.func()
SuperExtendExample.func()
```

## 4.4 重写与重载

- 重写(Override)存在于继承体系中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为 private 则子类就不能重写该方法
  
- 重载(Overload)存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。应该注意的是，返回值不同，其他都相同不算是重载。

# 五、 Object 通用方法

```java
public final native Class<?> getClass()

public native int hashCode()

public boolean equals(Object obj)

protected native Object clone() throws CloneNotSupportedException

public String toString()

public final native void notify()

public final native void notifyAll()

public final native void wait(long timeout) throws InterruptedException

public final void wait(long timeout, int nanos) throws InterruptedException

public final void wait() throws InterruptedException

protected void finalize() throws Throwable {}
```

## 5.1 equals

### 5.1.1 equals() 与 == 的区别

- 对于基本类型， == 判断两个值是否相等，基本类型没有 equals() 方法。
- 对于引用类型， == 判断两个实例是否引用同一个对象，而 equals() 判断引用的对象是否等价。

```java
Integer x = new Integer(1);
Integer y = new Integer(1);
System.out.println(x.equals(y)); // true
System.out.println(x == y);      // false
```

### 5.1.2 重写equals()

重写equals()的必要条件：

（一）自反性

```java
x.equals(x) //true
```
(二) 对称性
```java
x.equals(y) == y.equals(x); // true
```

(三) 传递性

```java
if (x.equals(y) && y.equals(z))
    x.equals(z); // true;
```

(四) 一致性

多次调用 equals() 方法结果不变。

```java
x.equals(y) == x.equals(y); // true
```

(五) 与 null 的比较

对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false


```java
x.euqals(null); // false;
```

重写 equals() 的步骤：

- 检查是否为同一个对象的引用，如果是直接返回true；
- 检查是否是同一个类型，如果不是，直接返回false;
- 将 Object 实例进行转型;
- 判断每一个关键域是否相等。

```java
public class EqualExample {
    private int x;
    private int y;
    private int z;

    public EqualExample(int x, int y, int z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true; //判断是否是同一个对象的引用
        if (o == null || getClass() != o.getClass()) return false; //判断是否是同一类型

        EqualExample that = (EqualExample) o; //将object 类型进行转型

        if (x != that.x) return false;//
        if (y != that.y) return false;//
        return z == that.z;// 判断每一个关键域是否相等
    }
}
```

## 5.2 hashCode()

hashCode() 的作用是获取哈希码，也称为散列码，作用是确定该对象在哈希表中的索引位置，也因此该方法在散列表中才有用，在其他情况下没用。

### 5.2.1 hashCode 与 equals 的关系

- 等价的两个实例散列值一定要相同，但是散列值相同的两个实例不一定等价
- 在重写 equals() 时应当也要重写 hashCode(), 去保证等价的两个实例散列值也相等。

下面举例为什么需要同时重写这 2 个方法。

 新建了两个等价的实例，并将它们添加到 HashSet 中。我们希望将这两个实例是一样的，只在集合中添加一个实例，但是因为 EqualExample 没有重写 hashCode()，因此这两个实例的散列值是不同的，最终导致集合添加了两个等价的实例。

```java
EqualExample e1 = new EqualExample(1, 1, 1);
EqualExample e2 = new EqualExample(1, 1, 1);
System.out.println(e1.equals(e2)); // true
HashSet<EqualExample> set = new HashSet<>();
set.add(e1);
set.add(e2);
System.out.println(set.size());   // 2
```

### 5.2.2 如何重写 hashCode

理想的散列函数应当具有均匀性，即不相等的实例应当均匀分布到所有可能的散列值上。这就要求散列函数要把所有域的值都考虑进来，可以将每一个域都当成R进制的某一位，然后组成一个R进制的整数。R一般取31，因为它是一个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与2相乘相当于向左移一位。

分别解析自定义类中与 equals 方法相关的域（变量），并以以下方式进行计算散列值。

单个域 a[hashCode] 的取值。

- boolean：true ？1 : 0
- byte/short/int/char: (int)a
- long: (int) (a^ a>>>32)
- float：Float.hashCode(a)
- double：Double.hashCode(a)
- 引用类型：若为 null 则为 0，否则递归调用该引用类型的 hashCode()。
- 容器类型：若为 null 则为 0，否则对每个变量进行 result =  31 * result + [hashCode] 计算。

```java
@Override
public int hashCode() {
    int result = 17;

    // 下面一行代码代表一个域
    result = 31 * result + [hashCode];
    result = 31 * result + [hashCode];
    result = 31 * result + [hashCode];

    return result;
}
```





