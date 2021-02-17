---
title: "Lambda表达式在观察者模式中的应用"
date: 2021-02-17T21:34:05+08:00
draft: false
categories: ["Programming"]
tags: ["Java"]
---

# 观察者模式

```java
// 观察者接口
package com.chapter.eight.observer;

public interface LandingObserver {
  void observerLanding(String name);
}

// 观察者实现类1
package com.chapter.eight.observer;

public class Aliens implements LandingObserver {

  @Override
  public void observerLanding(String name) {
    if (name.contains("Apollo")) {
      System.out.println("They'redistracted,lets invade earth!");
    }
  }
}

// 观察者实现类2
package com.chapter.eight.observer;

public class Nasa implements LandingObserver {

  @Override
  public void observerLanding(String name) {
    if (name.contains("Apollo")) {
      System.out.println("We made it!");
    }
  }
}


// 被观察者

package com.chapter.eight.observer;

import java.util.ArrayList;
import java.util.List;

public class Moon {

  private final List<LandingObserver> observers = new ArrayList<>();

  public void startSpying(LandingObserver observer) {
    observers.add(observer);
  }

  public void land(String name) {
    for (LandingObserver observer : observers) {
      observer.observerLanding(name);
    }
  }

}

// 入口
package com.chapter.eight.observer;

public class ObserverDemo {

  public static void main(String[] args) {
    Moon moon = new Moon();
    moon.startSpying(new Aliens());
    moon.startSpying(new Nasa());
    moon.land("Apollo 11");
  }
}
```

在观察者模式中，观察者接口是标准的函数接口，其实现类实际上封装了实现该接口的行为。在上例中，该函数接口的类型为`<String> -> <void>`，因此我们可以将观察者实现类中的相应方法改写为lambda表达式。不过，与命令者式不同，如果观察者实现类有较高的可能性被复用，则应当独立为类，不应勉强地改写为lambda表达式。

```java
package com.chapter.eight.observer;

public class ObserverDemo {

  public static void main(String[] args) {
    Moon moon = new Moon();
    LandingObserver alien = (name) -> {
      if (name.contains("Apollo")) {
        System.out.println("They're distracted,lets invade earth!");
      }
    };
    moon.startSpying(alien);

    LandingObserver nasa = (name) -> {
      if (name.contains("Apollo")) {
        System.out.println("We made it!");
      }
    };
    moon.startSpying(nasa);
    moon.land("Apollo 11");
  }
}
```