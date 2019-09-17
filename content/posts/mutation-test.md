---
title: "Mutation Test"
date: 2019-09-17T20:27:21+08:00
draft: false
categories: ["Test"]
tags: ["PITest"]
---

# Mutation Test with PITest

## 测试用例代码覆盖率的意义

一般的软件测试关注测试用例的代码复杂度，理想的状态是100%的代码覆盖率，即测试用例能将所有业务代码执行一遍，保证所有的函数、方法、模块都被测试过。一些测试工具，如[JaCoco](https://www.jacoco.org)等，可以统计测试用例的代码覆盖率。

### 代码覆盖率的局限性

举一个简单的例子，判断一个正数是不是自然数：

```java
package com.example.pitest;

public class NaturalNumber {

  private int num;

  public NaturalNumber(int num) {
    this.num = num;
  }
  // wrong condition judgement
  public boolean isNaturalNumber() {
    return num > 0;
  }
}
```

引入JaCoCo gradle插件
```groovy
plugins {
    id 'java'
    id 'jacoco'
}
```

以上函数`isNaturalNumber()`对于自然数的判断条件明显错误（正确应为`num >= 0`）。然而如果我们的测试用例不够完善，就会忽略这个错误，比如下面的测试用例：


```java
package com.example.pitest;

import org.junit.Assert;
import org.junit.Test;

public class TestNaturalNumber {

  @Test
  public void PositiveNumberIsNaturalNumber() {
    Assert.assertTrue(new NaturalNumber(10).isNaturalNumber());
  }

  @Test
  public void NegativeNumberIsNotNaturalNumber() {
    Assert.assertFalse(new NaturalNumber(-10).isNaturalNumber());
  }
}
```

运行JaCoCo test命令
```bash
 ./gradlew jacocoTestReport 
```

以上测试用例使用10和-10作为输入，测试`isNaturalNumber`方法能否做出正确判断。如果我们使用JaCoCo作为代码测试覆盖率工具，因为此时`num > 0`的两个分支均已覆盖到，JaCoCo会显示此时的代码覆盖率是100%。但这里的测试用例显然是不充分的，因为它没有反映最为关键的`num == 0`的情况。而一般的代码覆盖率工具无法有效地反映出这一点。

![JaCoCo Report](https://s2.ax1x.com/2019/09/17/nIcmGD.png)

## mutation test的意义

普通的代码覆盖率检查并不能有效检测出上述测试用例的缺失，背后原因是以上的两个测试用例仅仅是分别将`num > 0`的两个分支路径执行了一遍，而并没有真正体现出此判断条件的作用，如果我们将`num > 0` 改为 `num > 1`，上述测试用例一样可以通过且代码覆盖率为100%。

针对类似情况，我们可以引入[mutation test](https://en.wikipedia.org/wiki/Mutation_testing)框架

> Mutation testing (or mutation analysis or program mutation) is used to design new software tests and evaluate the quality of existing software tests. Mutation testing involves modifying a program in small ways. Each mutated version is called a mutant and tests detect and reject mutants by causing the behavior of the original version to differ from the mutant. This is called killing the mutant. Test suites are measured by the percentage of mutants that they kill. ... The purpose is to help the tester develop effective tests or locate weaknesses in the test data used for the program or in sections of the code that are seldom or never accessed during execution.

简言之，mutation test会在程序编译或运行时插入微小的差异(mutant)，理想的测试用例应当能够检测出这些差异带来的程序行为异常。如果一个mutant引发的程序行为异常能够被testcases捕捉并导致testcases失败，则称mutant被消灭（killed）；反之如果mutant带来的程序行为变化无法被测试用例捕捉，则称mutant存活（survived）。理想的测试用例应当能够100%消灭mutants，这意味着程序中所有的判断、运算的作用均已被测试用例运行并捕捉到。

### mutation test with PITest

[PITest](http://pitest.org)是一个比较流行的mutation test框架，可以在程序编译时期插入差异，并生成mutation coverage报告，结合以上例子，如果使用PITest生成测试报告，会发现测试用例存在覆盖率不足的问题。

引入PITest gradle插件

```groovy
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'info.solidsoft.gradle.pitest:gradle-pitest-plugin:1.4.5'
    }
}
apply plugin: 'info.solidsoft.pitest'
```

运行同样的测试用例

```bash
./gradlew pitest
```

生成的测试报告会显示，代码覆盖度是100%，但mutation覆盖度仅为67%。

| Name |  Line Coverage| Mutation Coverage |
| --- | --- | --- |
| NaturalNumber.java | 100% 4/4 |  67% 2/3 |

我们可以发现，由`changed conditional boundary`([CONDITIONALS_BOUNDARY](http://pitest.org/quickstart/mutators/#CONDITIONALS_BOUNDARY))带来的一个mutation survived。这个mutation的行为是将`num  > 0`改为`num >= 0`，即更改边界条件，但测试用例没能捕捉到这个差异。

![PITest Report](https://s2.ax1x.com/2019/09/17/nIccJU.png)

为了捕捉到这个mutant，我们需要针对`num == 0`这种边界条件添加新的测试用例
，而当我们添加了这个测试用例后，错误的判断条件（`num > 0`）就会被测试出来。


