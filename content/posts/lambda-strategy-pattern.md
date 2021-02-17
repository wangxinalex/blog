---
title: "Lambda表达式在策略模式中的应用"
date: 2021-02-17T21:37:49+08:00
draft: false
categories: ["Programming"]
tags: ["Java"]
---

# 策略模式

策略模式是一种常见的在运行时改变代码行为的设计模式。

参见以下例子：

```java
// 策略类接口
package com.chapter.eight.strategy;

import java.io.IOException;
import java.io.OutputStream;

public interface CompressionStrategy {
  OutputStream compress(OutputStream data) throws IOException;
}

// 实体策略类1
package com.chapter.eight.strategy;

import java.io.IOException;
import java.io.OutputStream;
import java.util.zip.GZIPOutputStream;

public class GZipCompressionStrategy implements CompressionStrategy {

  @Override
  public OutputStream compress(OutputStream data) throws IOException {
    return new GZIPOutputStream(data);
  }

}

// 实体策略类2
package com.chapter.eight.strategy;

import java.io.OutputStream;
import java.util.zip.ZipOutputStream;

public class ZipCompressionStrategy implements CompressionStrategy {

  @Override
  public OutputStream compress(OutputStream data) {
    return new ZipOutputStream(data);
  }
}

// 策略使用者
package com.chapter.eight.strategy;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;

public class Compressor {

  private final CompressionStrategy strategy;

  public Compressor(CompressionStrategy strategy) {
    this.strategy = strategy;
  }

  public void compress(Path inFile, File outFile) throws IOException {
    try (OutputStream outStream = new FileOutputStream(outFile)) {
      Files.copy(inFile, strategy.compress(outStream));
    }
  }
}

// 入口
package com.chapter.eight.strategy;

public class CompressDemo {

  public static void main(String[] args) {

    Compressor gzipCompressor = new Compressor(new GzipCompressionStrategy());

    Compressor zipCompressor = new Compressor(new ZipCompressionStrategy());
  }
}
```

在上述例子中，`GZipCompressionStrategy`和`ZipCompressionStrategy`两个策略实体类都实现了`CompressionStrategy`接口，后者是一个标准的函数接口，因此一定可以替换为lambda表达式。改函数接口输入一个`OutputStream`对象，输出一个`OutputStream`对象，符合`GZipOutputStream`和`ZipOutputStream`的构造方法的签名，因此可以用其构造方法代替。

```java
// GZIPOutputStream的构造方法
public GZIPOutputStream(OutputStream out) throws IOException {
    this(out, 512, false);
}
```

```java
// 入口
package com.chapter.eight.strategy;

public class CompressDemo {

  public static void main(String[] args) {

    Compressor gzipCompressor = new Compressor(GZipOutputStream::new);

    Compressor zipCompressor = new Compressor(ZipOutputStream::new));
  }
}
```

可以看到lambda表达式和方法引用可以有效替换策略模式中的具体策略类，给策略的使用者传入不同的行为。


