# lambda表达式

[TOC]

JAVA对lambda表达式的需求源于实践。因此通过概念认识lambda实属缘木求鱼。过早接触概lambda念于认清lambda毫无帮助。

## 基本语法

```java
(parameters) -> expression
(parameters) -> { expression;}
```

## 函数式接口

1. 只包含一个抽象方法的接口，可以称之为函数式接口
2. 可以通过Lambda表达式创建接口对象
3. 可以使用@FunctionalInterface 注解，来检查自己定义的函数式接口是否正确。同时@FunctionalInterface 注解还会帮助添加一条javadoc函数式接口的说明。

### 通过接口传递代码

接口以及面向接口的编程，针对接口而非具体类型进行编程，可以降低程序的耦合性，提高灵活性，提高复用性。

### 举例

我们知道File有如下方法：

```java
// 该方法会遍历目录下的所有文件，将文件目录和文件名交于过滤器检测，检测成功文件会被存放File[]返回。
public File[] listFiles(FilenameFilter filter)
```

listFiles需要的其实不是FilenameFilter对象，而是它包含的如下方法：

```java
boolean accept(File dir, String name);
```

或者说，listFiles希望接受一段方法代码作为参数，但没有办法直接传递这个方法代码本身，只能传递一个接口。

```java
public static void Test01() {
        File f = new File(".");
        // 通过接口传递行为代码，就要传递一个实现了该接口的实例对象，在Java8以前最简洁的方式是使用匿名内部类
        // 列出当前目录下的所有扩展名为．txt的文件
        File[] files = f.listFiles(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                if (name.endsWith(".xml")) {
                    return true;
                }
                return false;
            }
        });

        for (File file : files) {
            System.out.println(file.getAbsolutePath());
        }
    }
```

通过Lambda进行改进

```java
//Java 8提供了一种新的紧凑的传递代码的语法：Lambda表达式。对于前面列出文件的例子，代码可以改为：
{
    File[] files = f.listFiles((File dir, String name) -> {
        if (name.endsWith(".txt")) {
            return true;
        }
        return false;
    });
}
// 可以看出，相比匿名内部类，传递代码变得更为直观，不再有实现接口的模板代码，不再声明方法，也没有名字，而是直接给出了方法的实现代码。
// Lambda表达式由->分隔为两部分，前面是方法的参数，后面{}内是方法的代码。上面的代码可以简化为：
{
    File[] files = f.listFiles((File dir, String name) -> {
        return name.endsWith(".txt");
    });
}
//当主体代码只有一条语句的时候，括号和return语句也可以省略，上面的代码可以变为：
{
    File[] files = f.listFiles((File dir, String name) -> name.endsWith(".txt"));
}
// 注意：没有括号的时候，主体代码是一个表达式，这个表达式的值就是函数的返回值，结尾不能加分号，也不能加return语句。
{
    File[] files = f.listFiles((dir, name) -> name.endsWith(".txt"));
}
//之所以可以省略方法的参数类型，是因为Java可以自动推断出来，它知道listFiles接受的参数类型是FilenameFilter，这个接口只有一个方法accept，这个方法的两个参数类型分别是File和String。
```



## 实例



```

```