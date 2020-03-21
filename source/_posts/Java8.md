---
title: Java8
date: 2020-03-18 09:19:08
tags:
---

# Java8

## FunctionalInterface函数式接口学习

除了default和静态方法，只有一个方法的函数就可以是函数式接口

1. 常用的接口分为四大类

```
#消费，接受一个对象，我来处理它，没有返回值
#理解：花钱
Consumer<T>  void accept(T t);

#生产，没有参数，就想单纯得到一个对象
#理解：没对象啊，给我生产一个，我需要用它
Supplier<T>  T get();

#断言 接受一个对象，我来处理它，判断他是否正确 
#理解：你要断言，就要给我对象和判断规则，我给你他对不对
Predicate<T> boolean test(T t);

#函数 接受一个对象，我来处理它，处理后返回一个东西给你
#理解：给我一个对象，他可以给你带来快乐
Function<T, R>  R apply(T t);
```



## Lambda表达式

基于函数式接口，简洁你的代码

使用语法可以总结为：（params1，params2）->{statement}

参数：这里如果是一个参数可以省略（）

返回值：如果一行代码直接返回，可以省略{}

原因：lambda会有一个类型判断，根据你的函数接口判断你的入参和返回值



lambda就是函数式接口实现创建一个真实的对象去调用你的方法体，{statement}是你要执行的方法体

1. 自我理解：看下面的例子

```
#没用lambda的时候
Consumer<Object> consumer = new Consumer<Object>() {
    @Override
    public void accept(Object o) {
        System.out.println(o);
    }
};
#使用lambda表达式怎么写
Consumer<Object> consumerLambda = (s-> System.out.println(s));
```

假设你现在有个方法

```
public static<T> void print(T t,Consumer<T> consumer){
    consumer.accept(t);
}
```

你就可以写成

```
print("ss",s-> System.out.println(s));
```

它会判断你的入参类型，T是String，s其实就是你在内部类中的Object参数

相当于这个接口new了一个 实现Consumer的真实对象，然后去调用实现的accept方法，执行的方法就是lambda的方法体System.out.println(s)

**最简单的解释consumer的accept方法就是你的lambda表达式中要执行的方法体**

有的人可能不太理解s-> System.out.println(s)的意思，就是T这个类型确认以后，你的参数也就确定了，类型也就确定了，实现accept方法的方法体就是你的lambda要做的事情，这里的方法体就是System.out.println(s)

有的人可能还会有一个疑惑，构造出这个函数式接口有什么意义，不就是为了简化代码，把你想对参数的处理，或者想得到一个对象简单的操作变得更简单，然后把接口当成别的方法的参数，你就可以用lambda，多方便呀！



再看一个最常用的例子吧

在集合Iterable<T>接口中有这么一个方法

```
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);
    }
}
```

里面后一个Consumer函数式接口，你new一个ArrayList的时候，调用forEach

this就是你的实现ArrayList集合，你的方法体呢，比如s-> System.out.println(s)

它就会打印集合所有元素

不知道这样说明不明白，这是我自己的理解

2. 结合上面的函数式接口说明

   ```
   Consumer<T> 接收一个参数T，方法体就是你要怎么处理它
   Supplier<T> 无参数，方法体是直接return一个你想要的T
   Function<T, R> 接收一个参数T，方法体就是你要怎么处理它，然后返回一个R
   Predicate<T> 接收一个参数T，返回boolean 实际就是Function<T, Boolean>
   ```

3. 方法引用

   说到lambda，不得不提这个功能，我觉得挺难理解的，大家肯定见过**Integer::new**这种写法，这个就是方法引用的作用，返回的就是一个函数式接口Function<Integer, Integer> function = Integer::new;具体的方法就是：

   ```
   public Integer(int value) {
       this.value = value;
   }
   ```

可以看出，这个和Function是一样的，传入一个参数，返回一个参数，java8就可以推导出可以作为一个函数式接口来用，方法引用的标准形式是：`类名::方法名`。

有以下四种形式的方法引用：

| 方式                             | 用法                                 |
| :------------------------------- | :----------------------------------- |
| 引用静态方法                     | ContainingClass::staticMethodName    |
| 引用某个对象的实例方法           | containingObject::instanceMethodName |
| 引用某个类型的任意对象的实例方法 | ContainingType::methodName           |
| 引用构造方法                     | ClassName::new                       |