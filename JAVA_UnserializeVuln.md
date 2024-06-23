# JAVA反序列化漏洞

## 前置知识**Java IO**

在学习Java反序列化之前我们先要了解一下Java的输入输出流：

java的IO流分为了**文件IO流（FileInput/OutputStream）和对象IO流（ObjectInput/OutputStream）**，从名字上就可以看出来一个是用来对文件进行输入和输出，一个是对对象进行输入和输出。

* 文件IO流：FileInputStream（将文件变为数据）FileOutputStream(filename)(将数据输出到filename文件)
* 对象同理

分析下面案例

```java
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("filename"));
oos.writeObject(obj); // ojb----->数据流------>文件
```

## 序列化与反序列化过程

一句话序列化的过程就是将对象**转变**为数据流（obj----->数据流）。同理反序列化就是数据流在**恢复**成对象的过程（数据流----->对象）;

```java
/** 要序列化和反序列化的类 **/
import java.io.Serializable;

public class Person implements Serializable {

    private String name;
    private int age;

    public Person(){

    }
    // 构造函数
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString(){
        return "Person{" +
                "name='" + name + '\\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
/** 序列化 **/
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutput;
import java.io.ObjectOutputStream;

public class SerializationTest {
    public static void serialize(Object obj) throws IOException{
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("ser.bin"));
        oos.writeObject(obj);
    }

    public static void main(String[] args) throws Exception{
        Person person = new Person("aa",22);
        System.out.println(person);
        serialize(person);
    }
}
```



```java
/** 反序列化 **/
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class UnserializeTest {
    public static Object unserialize(String Filename) throws IOException, ClassNotFoundException{
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(Filename));
        Object obj = ois.readObject();
        return obj;
    }

    public static void main(String[] args) throws Exception{
        Person person = (Person)unserialize("ser.bin");
        System.out.println(person);
    }
}
```



## Java反射

Java中如果我们有一个类，那么我们可以通过实例化(new)该类的对象并调用其中的方法，或者我们也可以直接调用该类中的静态方法。如果把通过new对象并且调用其中的方法的过程叫做“正射”，那么不使用new来创建对象并调用其中方法的过程就叫做“反射”。

### **反射常用到的方法**

在java的lang包中有一个静态Class类， 在java程序运行并编译加载一个类时，java.lang.Class就会实例化出一个对象，这个对象储存该类的所有信息 因此我们可以通过一些方法来获取到这个类的信息 先了解一些方法

- `Class.forName(classname)` 获取classname类中的所有属性包括类名 比如`Class clazz = Class.forName("java.lang.Runtime");`

  那么类clazz中就得到了java.lang.Runtime中的所有属性。

- `Class.newInstance()`实例化对象，并触发该类的构造方法 下面会详细解释。

- `Class.getMethod(method name,arg)` 获取一个对象中的public方法，由于java支持方法的重载，所以需要第二参数作为获取的方法的形参列表，这样就可以确定获取的是哪一个方法。

- `Method.invoke`() 执行方法，如果是一个普通方法，则invoke的第一个参数为该方法所在的对象，如果是静态方法则第一个参数是null或者该方法所在的类 第二个参数为要执行方法的参数。

`forName`并不是唯一获取一个类的方式，其他方式还有：

- obj.getClass() 如果上下文中存在某个类的实例obj，那我们可以直接通过obj.getClass来获取它的类
- Y1.class 如果已经加载了一个类Y1，只是想获取到它由java.lang.class所创造的对象，那么就直接使用这种方法获取即可，这种方法并不属于反射
- Class.Forname 如果知道某个类的名字，想获取到这个类，就可以使用forName来获取

## **关于forname**

默认情况下 `forName`的第一个参数是类名，第二个参数表示是否初始化，第三个参数就是ClassLoader。ClassLoader是一个“加载器”，告诉java虚拟机如何加载这个类，java默认的ClassLoader就是根据类名加载类，这个类名必须是完整路径，比如上面提到的java.lang.Runtime

第二个参数`initialize`用于forname时的初始化，一般我们会认为初始化就是加载类的构造函数，其实并不是，这里提到的初始化有以下过程：

```java
class TrainPrint{
    {
        System.out.printf("Empty block inittial &s\\n", this.getClass());
    }

    static{
        System.out.printf("Static initial %s\\n", TrainPrint.class);
    }

    public TrainPrint(){
        System.out.printf("Innitial %s\\n", this.getClass());
    }
}
```

