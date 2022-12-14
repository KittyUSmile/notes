

# Java8新特性



## Stream流

> 对数组或集合进行链式操作，使用场景很多。





### 基本数据类型优化

> Stream 流在操作方便的同时，其实并没有简化底层的操作，该怎样还是怎样，只是减少了我们的代码量。我们使用 Stream 流的 map 操作时可能会遇到一些性能问题，当我们的数据 map 的数据是基本类型的包装类型，如Integer 时，在实现方法中对其进行加减或其它操作，会先将其转为 int 类型，这是一个自动拆箱的过程，当返回时又是 Integer，就会进行自动装箱操作，当 Stream 流操作的数据量足够大时，可能就会消耗过量的时间，因此我们需要尝试去解决这一问题，解决频繁拆箱装箱。而 Stream 为我们提供了这样一些 map 操作，如 mapToInt、mapToFloat、mapToDouble、mapToLong，直接将参数变成基本数据类型，就解决了我们频繁拆箱装箱的问题了。

**使用示例**

```java
@Test
public void test01(){
    List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9);
    list.stream()
        .map(integer -> integer++)
        //优化为 mapToInt
        .mapToInt(i -> i++);
}
```



### 并行流

> 



## Optional

> Optional 的作用是优雅地避免空指针异常，Java8以前判断某个要调用对象之间的属性或方法，可能需要先对属性判空，不然可能触发空指针异常，这种操作一多，可能就会影响代码的阅读感，而Optional 就是解决这种问题而来的。

### 概述

> Optional 本质上只是通过一层封装把原对象包装成一个 Optional 对象，原对象作为了 Optional 的 value 属性，通过 Optional 对象来操作原对象，就能很好避免空指针。



### 创建对象

> Optional 创建对象的方式只能是 Optional.xx() ，因为 Optional 里的构造方法都是私有的，创建Optional 对象的方式是 Optional.of(T value) 、**Optional.ofNullable(T value)** 和 Optional.empty()。

#### Optional.of(T value)

> Optional.of(T value) 方法的特点是传入的 value 参数不能为空，如果为空，会抛出空指针异常。该方法返回一个 Optional\<T> 对象。

**模板：Optional\< 对象 > op = Optional.of ( 对象 );**

**使用示例**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setName("张三");
    
    Optional<Student> stuOptional = Optional.of(student);   // 正常运行
    Optional<Student> o2 = Optional.of(null);				// 抛出空指针异常
}
```



#### Optional.ofNullable(T value)

> Optional.ofNullable(T value) 方法的特点是允许传入的 value 为空，这是我们最常使用的创建 Optional 对象的方法，该方法返回一个 Optional\<T> 对象。

**模板：Optional\< 对象 > op = Optional.ofNullable ( 对象 );**

**使用示例**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setName("张三");
    
    Optional<Student> o1 = Optional.ofNullable(student);	// 正常运行
    Optional<Student> o2 = Optional.ofNullable(null);		// 正常运行
}
```



#### Optional.empty()

> 创建出一个 value 为空的 Optional 对象。该方法比较常用的场景是方法返回 Optional 对象的地方（将原本返回对象封装进 Optional 返回，这是一种编程习惯的改变），如果封装的对象为空，则调用该方法。（相对鸡肋，完全可以用 Optional.ofNullable() 替换）

**模板：Optional\< 对象 > op = Optional.empty();**

**使用示例**

```java
@Test
public void test01(){
    Optional<Student> o1 = Optional.empty();
}
```



### 方法

> Optional 对象提供的操作 value 的方法。

#### ifPresent(Consumer<? super T> action)

> 如果 value 不为空则执行参数提供的方法。Consumer 其实是一个函数式接口，代表要进行的操作，我们实现的是其中的 accept(T t) 方法，其没有返回值。ifPresent() 方法内部会先对 value 进行判空，再执行 accept() 方法。

**使用示例**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setName("张三");
    
    Optional.ofNullable(student).ifPresent(stu->{
        System.out.println(stu.getName());
    });
}
```



#### get()

> 获取 Optional 对象中的 value 属性。需要注意的是：如果 Optional 对象的 value 属性为空，get() 方法会抛出 NoSuchElementException 异常，所以 get() 方法要慎用。

**使用示例**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setName("张三");
    
    Optional<Student> o1 = Optional.ofNullable(student);
    o1.get();			// 正常运行
    
    Optional<Student> o2 = Optional.ofNullable(null);
    o2.get();			// 抛出 NoSuchElementException 异常
}
```



#### orElseGet(Supplier<? extends T> supplier)

> 获取 Optional 对象中的 value 属性，不过和 get() 方法有区别的地方是：调用 orElseGet() 方法当 value 为空时，会执行参数提供的方法，并返回方法执行完毕后的对象，需要注意的是，Supplier 也是一个函数式接口，我们实现的是其中的 get() 方法，该方法具有返回值。

**使用示例**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setName("张三");
    
    Optional<Student> o1 = Optional.ofNullable(student);
    o1.orElseGet(()->new Student());    // 获得张三student对象
    
    Optional<Student> o2 = Optional.ofNullable(null);
    o2.orElseGet(()->new Student());    // 获得新student对象
}
```



#### orElseThrow()

> 获取 Optional 对象中的 value 属性，但是当 value 属性为空时，会抛出 NoSuchElementException 异常，效果和 get() 方法一样。该方法还有一个重载方法，允许自定义抛出的异常类型。

**使用示例（orElseThrow() 方法需要 JDK版本 >= 10）**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setName("张三");

    Optional<Student> o1 = Optional.ofNullable(student);
    o1.orElseThrow();        // 正常运行

    Optional<Student> o2 = Optional.ofNullable(null);
    o2.orElseThrow();        // 抛出 NoSuchElementException 异常
}
```



#### filter(Predicate<? super T> predicate)

> 过滤 value ，不符合的话会在当前链操作中将其过滤（注意：是当前的链操作），其中 Predicate 是一个函数式接口，实现方法的返回值是 boolean 类型，返回为 false 时过滤掉 value。

**使用示例**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setGender(1);

    Optional<Student> optional = Optional.ofNullable(student);
    // 过滤掉value
    optional.filter(stu -> stu.getGender() == 0).ifPresent(stu -> System.out.println(stu));
    // 原filter操作不影响Optional对象
    optional.ifPresent(stu -> System.out.println(stu));     // 正常输出stu对象
}
```



#### isPresent()

> 判断 Optional 对象中的 value 是否存在（是否不为空）。

**使用示例**

```java
@Test
public void test01(){
    Student student = new Student();
    student.setGender(1);

    Optional<Student> o1 = Optional.ofNullable(student);
    System.out.println(o1.isPresent());   //true

    Optional<Student> o2 = Optional.ofNullable(null);
    System.out.println(o2.isPresent());   //false
}
```



#### map()

> 没用懂，感觉相当鸡肋。



## 函数式接口

> 函数式接口指的是只有一个抽象方法的接口，注意是一个抽象方法，通过继承的也算，默认方法不影响。函数式接口的主要作用是提供一批匿名内部类方法（当然可以用lambda表达式替换），它不关心方法的名字，只关心可以匿名提供什么类型的操作，有返回值的，无返回值的，返回值是对象的，返回值是 boolean 的，各成一类函数式接口，这些函数式接口的出现主要是为了给链式操作提供方便。



## 方法引用

> 方法引用也是为了简化代码，是一种语法糖，符号是 **::** ，它有一定的使用条件，即我们在使用 lambda 表达式时，如果方法体中只有一个方法的调用（包括构造方法），这时我们就可以使用方法引用进一步简化代码。

**基本格式：类名或对象名::方法名**



### 静态方法引用

> 简化单行引用类的静态方法的调用，使用静态方法引用也有前提：首先我们在使用 lambda 表达式时，方法体中只有一行代码，并且这行代码是调用了某个类的**静态方法**，并且我们把重写的抽象方法**所有的参数都按照顺序传入了这个静态方法**中，这时我们就可以改为使用静态方法引用的格式。

**格式：类名::方法名**

**使用示例**

```java
@Test
public void test01(){
    List<String> list = Arrays.asList("1", "2", "3", "4", "5", "6", "7", "8", "9", "10");
    list.stream()
        // 刚好s参数在实现方法的静态方法中恰好作为可以按顺序传入的参数
        .map(s -> String.valueOf(s))
        // 就可以将其改为静态引用
        .map(String::valueOf);
}
```



### 对象实例方法引用

> 简化单行对象实例方法的调用，使用对象实例方法引用有前提：首先我们在使用 lambda 表达式时，方法体中只有一行代码，并且这行代码是调用了某个对象的**实例方法**，并且我们把重写的抽象方法**所有的参数都按照顺序传入了这个实例方法**中，这时我们就可以改为使用对象实例方法引用的格式。

**格式：对象名::方法名**

**使用举例**

```java
@Test
public void test01(){
    List<String> list = Arrays.asList("1", "2", "3", "4", "5", "6", "7", "8", "9", "10");
    StringBuilder str = new StringBuilder();
    list.stream()
        // 刚好s参数在实现方法的实例方法中恰好作为可以按顺序传入的参数
        .map(s -> str.append(s))
        // 就可以将其改为对象引用
        .map(str::append);
}
```



### 引用类的实例方法

> 简化单行类实例方法的调用，使用类实例方法引用有前提：首先我们在使用 lambda 表达式时，方法体中只有一行代码，并且这行代码是调用了**第一个参数的成员方法**，并且我们把重写的抽象方法**所有的参数都按照剩余参数的顺序传入了这个实例方法**中，这时我们就可以改为使用类实例方法引用的格式。

**格式：类名::方法名**

**使用举例**

```java
public class Test01 {
    
    interface UseString{
        String use(String str, int start, int length);
    }
    
    public static String subAuthorName(UseString useString){
        return useString.use("hello", 1, 3);
    }

    @Test
    public void test01(){
        // 传统写法，第一个参数作为调用方法，后面的参数都传入第一个参数对象的某个方法中，可以改用引用
        subAuthorName((str, start, length)-> str.substring(start, length));
        subAuthorName(String::substring);
    }

}
```

[^注]: 这样的代码可读性较差，慎用



### 构造器引用

> 简化构造方法调用。使用构造器引用有前提：首先我们在使用 lambda 表达式时，方法体中只有一行代码，并且我们把重写的抽象方法**所有的参数都按顺序传入了这个构造方法**中，这时我们就可以改为使用构造器引用。

**格式：类名::new**

**使用举例**

```java
@Test
public void test01(){
    List<String> list = Arrays.asList("1", "2", "3", "4", "5", "6", "7", "8", "9", "10");
    list.stream()
        // 一行方法、参数按顺序全部传入方法，改用构造器引用
        .map(s -> new StringBuilder(s))
        .map(StringBuilder::new);
}
```

