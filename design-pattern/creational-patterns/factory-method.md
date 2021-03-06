# 工厂方法模式

## 工厂方法（Factory Method）

> 由子类决定实例化哪个类。

## Intent

定义了一个创建对象的接口，但由子类决定要实例化哪个类。工厂方法把实例化操作推迟到子类。

## Class Diagram

在简单工厂中，创建对象的是另一个类，而在工厂方法中，是由子类来创建对象。

下图中，Factory 有一个 doSomething\(\) 方法，这个方法需要用到一个产品对象，这个产品对象由 factoryMethod\(\) 方法创建。该方法是抽象的，需要由子类去实现。​  
​

![](../../.gitbook/assets/image%20%285%29.png)

## Implementation

```text
 public abstract class Factory {
     abstract public Product factoryMethod();
     public void doSomething() {
         Product product = factoryMethod();
         // do something with the product
     }
 }
```

```text
 public class ConcreteFactory extends Factory {
     public Product factoryMethod() {
         return new ConcreteProduct();
     }
 }
```

```text
 public class ConcreteFactory1 extends Factory {
     public Product factoryMethod() {
         return new ConcreteProduct1();
     }
 }
```

```text
 public class ConcreteFactory2 extends Factory {
     public Product factoryMethod() {
         return new ConcreteProduct2();
     }
 }
```

```text
public class Client {
    public static void main(String[] args) {
        Factory factory = new ConcreteFactory();
        factory.doSomething();
    }
}
```

 

## JDK

* [java.util.Calendar](http://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html#getInstance--)
* [java.util.ResourceBundle](http://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-)
* [java.text.NumberFormat](http://docs.oracle.com/javase/8/docs/api/java/text/NumberFormat.html#getInstance--)
* [java.nio.charset.Charset](http://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html#forName-java.lang.String-)
* [java.net.URLStreamHandlerFactory](http://docs.oracle.com/javase/8/docs/api/java/net/URLStreamHandlerFactory.html#createURLStreamHandler-java.lang.String-)
* [java.util.EnumSet](https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html#of-E-)
* [javax.xml.bind.JAXBContext](https://docs.oracle.com/javase/8/docs/api/javax/xml/bind/JAXBContext.html#createMarshaller--)

