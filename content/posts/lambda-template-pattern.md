---
title: "Lambda表达式在模板模式中的应用"
date: 2021-02-17T21:30:10+08:00
draft: false
categories: ["Programming"]
tags: ["Java"]
---

# 模板模式

模板模式在父类（抽象类）中实现算法的框架，将算法中的具体步骤放在子类（具体类）中实现。

其中算法的各个步骤可能实现为函数接口的形式，可以使用lambda表达式代替。如下例:

```java
// 父类中的算法步骤抽象为函数接口
package com.chapter.eight.template;

@FunctionalInterface
public interface Criteria {
  void check();
}


// 父类
package com.chapter.eight.template;

public class LoanApplication {

  private final Criteria identity;
  private final Criteria creditHistory;
  private final Criteria incomeHistory;

  public LoanApplication(Criteria identity, Criteria creditHistory, Criteria incomeHistory) {
    this.identity = identity;
    this.creditHistory = creditHistory;
    this.incomeHistory = incomeHistory;
  }

  public void checkLoanApplication() {
    identity.check();
    creditHistory.check();
    incomeHistory.check();
    reportFindings();
  }

  private void reportFindings() {
  }

}

// 子类
package com.chapter.eight.template;

public class CompanyLoanApplication extends LoanApplication {
  public CompanyLoanApplication(Company company) {
    super(company::checkIdentity, company::checkHistoricalDebt, company::checkProfitAndLoss);
  }
}

package com.chapter.eight.template;

public abstract class Company {

  public abstract void checkIdentity();

  public abstract void checkProfitAndLoss();

  public abstract void checkHistoricalDebt();

}
```

父类中，算法的各个步骤使用函数接口表示，因此在子类中，我们可以使用lambda表达式或者方法引用实现这些步骤。

