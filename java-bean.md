# 一 、什么是java-bean



1. 所有属性为private

2. 提供默认构造方法
3. 提供getter和setter
4. 实现serializable接口 

实际上就是一种向后兼容的规范。

# 二、java注解

首先可以先将注解理解为一个简单的标签。

## 2.1 注解语法

```java
public @interface TestAnnotaion{

}
```

形式与接口很类似，不过在interface前面多加了一个@符号。

## 2.2 注解的应用

例如：

```java
@TestAnnotation
public class Test{

}
```

创建一个类Test， 然后在类定义的地方加上 @TestAnnotation 就可以用 TestAnnotation 注解这个类了。 

你可以简单理解为将 TestAnnotation 这张标签贴到 Test 这个类上面。

不过，要想注解能够正常工作，还需要介绍一下一个新的概念那就是元注解。

## 2.3 元注解

元注解是可以注解到注解上的注解，或者说元注解是一种基本注解，但是它能够应用到其它的注解上面。

如果难于理解的话，你可以这样理解。元注解也是一张标签，但是它是一张特殊的标签，它的作用和目的就是给其他普通的标签进行解释说明的。

 元标签有-**@Retention**、**@Documented**、**@Target**、@**Inherited**、**@Repeatable** 5 种。 



### 2.3.1 @Retention

 Retention意为保留期的意思。当 **@Retention** 应用到一个注解上的时候，它解释说明了这个注解的的存活时间。

 它的取值有如下几种：

- RententionPolicy.SOURCE 注解只在源码阶段保留，在编译器进行编译时它将会被忽视。

- RetentionPolicy.CLASS 注解只被保留到编译进行的时候，并不会被加载到JVM中。

-  RetentionPolicy.RUNTIME 注解可以保留到程序运行的时候，它会被加载进入到 JVM 中，所以在程序运行时可以获取到它们 。

  示例：

  ```java
  @Retention(RetentionPolicy.RUNTIME)
  public @interface TestAnnotation{
  
  }
  ```

此例中指定TestAnnotation可以程序运行周期被获取到，因此它的生命周期非常长。



### 2.3.2 @Documented

 顾名思义，这个元注解肯定是和文档有关。它的作用是能够将注解中的元素包含到 Javadoc 中去。 



### 2.3.3 @Target

Target是目标的意思，**@Target**指定了注解运用的地方。

 你可以这样理解，当一个注解被 **@Target **注解时，这个注解就被限定了运用的场景。 

类比到标签 ，原本标签是你想张贴到哪个地方就到哪个地方，但是因为 **@Target **的存在，它张贴的地方就非常具体了，比如只能张贴到方法上、类上、方法参数上等等。**@Target **有下面的取值 :

- ElementType.ANNOTATION_TYPE 可以给一个注解进行注解
- ElementType.CONSTRUCTOR 可以给构造方法进行注解
-  ElementType.FIELD 可以给属性进行注解 
-  ElementType.LOCAL_VARIABLE 可以给局部变量进行注解 
-  ElementType.METHOD 可以给方法进行注解 
-  ElementType.PACKAGE 可以给一个包进行注解 
-  ElementType.PARAMETER 可以给一个方法内的参数进行注解 
-  ElementType.TYPE 可以给一个类型进行注解，比如类、接口、枚举 



### 2.3.4 @Inherited

**Inherited** 是继承的意思，但是它并不是说注解本身可以继承，而是说如果一个超类（祖先类）被**@Inherited**注解过的注解进行注解的话，那么如果它的子类没有被任何注解应用的话，那么这个子类就继承了超类的注解。

例如：

```java
@Inherited
@Retention(RetetionPolicy.RUNTIME)
@interface Test{}

@Test
public class A{}

public class B extends A{}
```

注解Test被**@Inherited**修饰，之后类**A**被**Test**注解，类**B**继承**A**，类**B**也拥有**Test**这个注解。



可以这样理解：

老子非常有钱，所以人们给他贴了一张标签叫做富豪。

老子的儿子长大后，只要没有和老子断绝父子关系，虽然别人没有给他贴标签，但是他自然也是富豪。

老子的孙子长大了，自然也是富豪。

这就是人们口中戏称的富一代，富二代，富三代。虽然叫法不同，好像好多个标签，但其实事情的本质也就是他们有一张共同的标签，也就是老子身上的那张富豪的标签。



### 2.3.5 @Repeatable

 **Repeatable**是可重复的意思。 

 什么样的注解会多次应用呢？通常是注解的值可以同时取多个。 

 举个例子，一个人他既是程序员又是产品经理,同时他还是个画家。 

示例：

```java
@interface Persons{
	Person[] value();
}

@Repeatable(Persons.class)
@interface Person{
    String role default "";
}

@Person(role="artist")
@Person(role="coder")
@Person(role="PM")
public class SuperMan{
    
}
```

