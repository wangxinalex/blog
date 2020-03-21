---
title: "Lambda表达式在命令者模式中的应用"
date: 2020-03-20T19:08:43+08:00
draft: false
categories: ["Programming"]
tags: ["Java"]
---


## @FunctionalInterface的作用

**函数接口**: 函数接口是*只有一个抽象方法*的接口，用作Lambda表达式的类型。如下例：

```java
public interface ActionListener extends EventListener {
    public void actionPerformed(ActionEvent event);
}
```

其中`actionPerformed`抽象方法必须用来表示*行为*

* 函数接口中抽象方法的命名不重要，只要方法签名和lambda表达式的*类型*匹配即可。
* 类型包括：接收参数类型，返回结果类型

`@FunctionalInterface`会检查某接口是否符合函数接口的定义。

## 命令者模式

命令者模式是最适合使用lambda表达式替换的设计模式。命令者模式中的Command对象本身就是一种封装了行为接口，完全符合函数接口的定义。

因此invoker对象可以直接接受lambda表达式并直接调用。

参见以下例子（详见https://www.runoob.com/design-pattern/command-pattern.html）

![image](https://upload-images.jianshu.io/upload_images/5024743-95e19b15d19aac27.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```java
// 命令对象接口
public interface Order {
   void execute();
}

// 命令接受者
public class Stock {
   
   private String name = "ABC";
   private int quantity = 10;
 
   public void buy(){
      System.out.println("Stock [ Name: "+name+", 
         Quantity: " + quantity +" ] bought");
   }
   public void sell(){
      System.out.println("Stock [ Name: "+name+", 
         Quantity: " + quantity +" ] sold");
   }
}

// 命令实体类
public class BuyStock implements Order {
   private Stock abcStock;
 
   public BuyStock(Stock abcStock){
      this.abcStock = abcStock;
   }
 
   public void execute() {
      abcStock.buy();
   }
}

public class SellStock implements Order {
   private Stock abcStock;
 
   public SellStock(Stock abcStock){
      this.abcStock = abcStock;
   }
 
   public void execute() {
      abcStock.sell();
   }
}

// 命令调用者类

import java.util.ArrayList;
import java.util.List;
 
public class Broker {
   private List<Order> orderList = new ArrayList<Order>(); 
 
   public void takeOrder(Order order){
      orderList.add(order);      
   }
 
   public void placeOrders(){
      for (Order order : orderList) {
         order.execute();
      }
      orderList.clear();
   }
}

// 入口

public class CommandPatternDemo {
   public static void main(String[] args) {
      Stock abcStock = new Stock();
 
      BuyStock buyStockOrder = new BuyStock(abcStock);
      SellStock sellStockOrder = new SellStock(abcStock);
 
      Broker broker = new Broker();
      broker.takeOrder(buyStockOrder);
      broker.takeOrder(sellStockOrder);
 
      broker.placeOrders();
   }
}
```

其中`Order`明显符合函数接口的定义，因此一定可以用lambda表达式或方法引用替换。替换它的lambda表达式的类型一定与`Order`接口中的抽象方法签名相同，在本例中，命令接受者的两个方法`buy`和`sell`符合此抽象方法的类型<void>-><void>，因此可以直接将命令接受者的两个方法的方法引用传入命令调用者，省去了构造命令实体类的工作。

```java
// 命令对象接口(声明为@FunctionalInterface)
@FunctionalInterface
public interface Order {
   void execute();
}

// 命令接受者
public class Stock {
   
   private String name = "ABC";
   private int quantity = 10;
 
   public void buy(){
      System.out.println("Stock [ Name: "+name+", 
         Quantity: " + quantity +" ] bought");
   }
   public void sell(){
      System.out.println("Stock [ Name: "+name+", 
         Quantity: " + quantity +" ] sold");
   }
}

// 命令调用者类

import java.util.ArrayList;
import java.util.List;
 
public class Broker {
   private List<Order> orderList = new ArrayList<Order>(); 
 
   public void takeOrder(Order order){
      orderList.add(order);      
   }
 
   public void placeOrders(){
      for (Order order : orderList) {
         order.execute();
      }
      orderList.clear();
   }
}

// 入口

public class CommandPatternDemo {
   public static void main(String[] args) {
      Stock abcStock = new Stock();
      
      Broker broker = new Broker();
      broker.takeOrder(abcStock::buy);
      broker.takeOrder(abcStock::sell); //传入lambda表达式
 
      broker.placeOrders();
   }
}
```

在原有的命令者模式中，之所以需要构造命令实体类（`BuyStock`和`SellStock`, 均实现了`Order`接口），是需要命令实体类作为一种组合成员，传入命令接口`Order`的不同实现，从而生成不同的行为。而lambda表达式可以直接传入不同的行为，因此在命令实体类不十分复杂，仅仅为命令接受者的不同方法的简单包装时，可以直接利用lambda表达式传入命令接受者的不同行为，无需再构造瘦包装的命令类。




