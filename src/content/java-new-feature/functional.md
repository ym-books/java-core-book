# Java函数式编程
Java 8 引入了函数式编程的概念，其中一个重要的概念就是函数式接口。函数式接口是一个只有一个抽象方法的接口，它可以被用作 lambda 表达式的类型，从而实现函数式编程的特性。

Java 自带了一些函数式接口，例如：

1. Runnable：没有入参，没有返回值的函数式接口，可以使用 lambda 表达式来替代匿名内部类，表示一段可执行的代码。
```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}

```
2. Supplier：没有入参，有一个返回值的函数式接口，可以使用 lambda 表达式来替代匿名内部类，表示一个能够供给值的对象。
```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}

```
3. Consumer：有一个入参，没有返回值的函数式接口，可以使用 lambda 表达式来替代匿名内部类，表示对一个对象进行操作。
```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}

```
4. Function：有一个入参，有一个返回值的函数式接口，可以使用 lambda 表达式来替代匿名内部类，表示将一个值转换为另一个值。
```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}

```
5. Predicate：有一个入参，返回一个 boolean 值的函数式接口，可以使用 lambda 表达式来替代匿名内部类，表示对一个对象进行判断。
```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}

```

这些函数式接口可以用于各种函数式编程场景，通过组合不同的函数式接口，可以实现更加复杂的函数式编程功能。需要注意的是，函数式接口只有一个抽象方法，但可以有多个默认方法或静态方法。函数式接口的定义需要使用 @FunctionalInterface 注解来标记，以确保只有一个抽象方法。

## 下面介绍具体使用
