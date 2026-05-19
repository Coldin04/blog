---
title: "由JUnit单元测试实验开始，浅学 SpringBoot初始化和测试"
date: 2026-05-19T22:48:20+08:00
draft: false
tags: ["Java", "JUnit", "SpringBoot", "单元测试"]
categories: ["Java", "测试"]
author: "Coldin04"
---

前两周软件测试课上，老师刚好提到用JUnit实验来了解软件测试。之前没怎么写过Java，也正好以此机会了解一下Java的基本概念。

## 环境配置

原实验档案建议使用IDEA创建Spring项目，方便快速配置环境、配置依赖。但实际上`SpringBoot` 也可以通过官方的 [https://start.spring.io/](https://start.spring.io/) 快速初始化，界面如下

![图片.png](https://files.seeusercontent.com/2026/05/19/x7Qn/a10f44f.png)

当然，也可以使用命令行工具快速创建项目，我创建的试验项目，列表如下所示：

```bash
curl https://start.aliyun.com/starter.tgz \
  -d groupId=com.cold04 \
  -d artifactId=junit-lab \
  -d dependencies=web \
  -d baseDir=junit-lab | tar -xzvf -
```

创建完成后，可以看到相关文件既创建成功。接下来是完成Lab相关的习题。



## 计数器练习

使用命令快速创建相关函数文件，要求的是实现一个简易计算器项目，并分别对相关函数进行测试。

```bash
mkdir -p src/main/java/com/cold04/junitlab/calculator
mkdir -p src/test/java/com/cold04/junitlab/calculator
```

首先创建`Calculator.java`文件，相关代码如下所示

```java
package com.cold04.calculator;

public class Calculator {
    // 加法
    public int add(int a, int b) {
        return a + b;
    }

    // 减法
    public int subtract(int a, int b) {
        return a - b;
    }

    //乘法
    public int multiply(int a, int b) {
        return a * b;
    }

    //除法
    public double divide(int a, int b) {
        if(b == 0) {
            throw new IllegalArgumentException("除数不能为零");
        }
        return (double) a / b;
    }
}
```

接下来创建测试类文件，位于 `src/test/java/com/cold04/calculator/CalculatorTest.java` ，代码如下：

```java
package com.cold04.calculator;

import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {

    private Calculator calculator;

    @BeforeEach
    public void setUP() {
        calculator = new Calculator();
        System.out.println("初始化 Calculator 对象中");
    }

    @AfterEach
    public void tearDown() {
        System.out.println("测试方法执行完毕");
    }

    @BeforeAll
    public static void initAll() {
        System.out.println("所有测试方法执行前的初始化");
    }

    @AfterAll
    public static void tearDownAll() {
        System.out.println("所有测试方法执行后的清理");
    }

    @Test
    public void testAdd() {
        System.out.println("正在测试 add 方法");
        assertEquals(10, calculator.add(5, 5), "5 + 5 应该等于 10");
        assertEquals(0, calculator.add(-5, 5), "-5 + 5 应该等于 0");
        System.out.println("正在测试 add 方法的负数情况");
        assertEquals(-10, calculator.add(-5, -5), "-5 + -5 应该等于 -10");
        assertEquals(-5, calculator.add(-2, -3), "-2 + -3 应该等于 -5");
    }

    @Test
    public void testSubtract() {
        assertEquals(5, calculator.subtract(10, 5), "10 - 5 应该等于 5");
        assertEquals(0, calculator.subtract(5, 5), "5 - 5 应该等于 0");
    }

    @Test
    public void testMultiply() {
        assertEquals(25, calculator.multiply(5, 5), "5 * 5 应该等于 25");
        assertEquals(-15, calculator.multiply(-5, 3), "-5 * 3 应该等于 -15");
    }

    @Test
    public void testDivide() {
        assertEquals(2.0, calculator.divide(10, 5), "10 / 5 应该等于 2.0");
        assertEquals(2.5, calculator.divide(5, 2), "5 / 2 应该等于 2.5");
    }

    @Test
    public void testDivideByZero() {
        // 测试除以零的情况，应该抛出 IllegalArgumentException
        assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        }, "除以零应该抛出 IllegalArgumentException");
    }
}
```

此时，相关主要文件已经创建完毕，且可以开始运行并测试，使用 `./mvnw -Dtest=CalculatorTest test` 指令既可开始测试。



## 测试和实验

### 基础测试

如下图所示，输入 `./mvnw -Dtest=CalculatorTest test`  后，相关提示如下，显然测试通过。

![图片.png](https://files.seeusercontent.com/2026/05/19/9ehI/8a62284.png)

### 修改断言，测试异常提示

如下图所示，这边假`testMultiply` 中 `-5 * 3` 因为测试者的忽视，意外写错`-15`，再次运行测试。如下图，可显然看到，测试在运行时指出了相关错误，但对于后续测试，是不影响的。对于测试前后的初始化调用，也不会因为某个单元测试不通过而停止。个人对此的理解是，测试前的环境准备和测试后的清理工作必不可少，例如在一些数据库环境中，在测试前写入相关mock数据，在测试后及时清理，避免数据异常影响下一阶段的测试或开发。这样的方式，显著的减少了开发时开发者的负担。

![图片.png](https://files.seeusercontent.com/2026/05/19/arB0/9ac938a.png)

### 测试除以零的情况

在此情况下，我们的计算器会抛出 `IllegalArgumentException` 错误，显然我们需要在测试中捕获这个错误。当test文件成功捕获相关异常的时候，代表相关异常处理行为正常，测试通过（此处可能略微有点反逻辑，可以理解为这个地方是能不能判断出错误的位置，判断出就予以通过）。

相关代码如下：

```java
    @Test
    public void testDivideByZero() {
        // 测试除以零的情况，应该抛出 IllegalArgumentException
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        }, "除以零应该抛出 IllegalArgumentException");
        assertEquals("除数不能为零", exception.getMessage());
    }
```

经过测试，显然测试成功通过。

![图片.png](https://files.seeusercontent.com/2026/05/19/7Aqu/a899f7c.png)

## 字符串判断测试

完成了上述测试后，既趁热打铁，做一个如下要求的程序，并完善边界测试：

> 设计一个简单的字符串工具`StringUtils`，包含以下方法：
> 1. isEmpty(String str) ：判断字符串是否为空（null 或空字符串）
> 1. reverse(String str) ：反转字符串
> 1. concat(String str1, String str2) ：拼接两个字符串
> 为该类编写完整的单元测试，要求：
> 1. 使用 @BeforeEach 和 @AfterEach 注解
> 1. 至少覆盖正常情况、边界情况和异常情况
> 1. 使用至少 4 种不同的断言方法

### 代码准备

简单来说，这个程序的设计思路是：先写一个简单的工具类 `StringUtils`，把字符串相关的常用操作封装起来，然后用 JUnit 单元测试验证每个方法在不同输入下是否符合预期。代码如下：

```java
package com.cold04.stringutils;

public class StringUtils {

    public boolean isEmpty(String str) {
        return str == null || str.isEmpty();
    }

    public String reverse(String str) {
        if (str == null) {
            throw new IllegalArgumentException("字符串不能为 null");
        }
        return new StringBuilder(str).reverse().toString();
    }

    public String concat(String str1, String str2) {
        String first = str1 == null ? "" : str1;
        String second = str2 == null ? "" : str2;
        return first + second;
    }
}
```

### 测试设计

测试文件 `StringUtilsTest.java`  的设计思路是：按方法分别测试，每个方法只验证自己对应的功能，这样出错时能很快知道是哪一块有问题。

`@BeforeEach` 用来在每个测试方法执行前重新创建一个 `StringUtils` 对象。这样每个测试都是独立的，不会互相影响。`@AfterEach` 用来在每个测试结束后输出提示，方便观察测试执行顺序。

测试内容分成三类：

1. 正常情况：比如 `"hello"` 反转成 `"olleh"`，两个普通字符串能正常拼接。这样验证基本功能是否正确。
1. 边界情况：比如 `null`、空字符串 `""`、单个字符 `"a"`。这些输入比较特殊，容易出错，所以要单独测。
1. 异常情况：比如 `reverse(null)` 应该抛出 `IllegalArgumentException`。这样可以确认程序遇到非法输入时不是随便崩溃，而是按设计抛出指定异常。

这样设计的好处是测试比较清晰：正常功能、特殊输入、错误输入都覆盖到了，后面如果代码改坏了，也能通过测试快速发现问题。

```
package com.cold04.stringutils;

import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class StringUtilsTest {

    private StringUtils stringUtils;

    @BeforeEach
    public void setUp() {
        stringUtils = new StringUtils();
        System.out.println("初始化 StringUtils 对象");
    }

    @AfterEach
    public void tearDown() {
        System.out.println("StringUtils 测试方法执行完毕");
    }

    @Test
    public void testIsEmpty() {
        // 边界情况：null 和空字符串都属于“空”的特殊输入。
        assertTrue(stringUtils.isEmpty(null), "null 应该被判断为空");
        assertTrue(stringUtils.isEmpty(""), "空字符串应该被判断为空");
        // 正常情况：有内容的字符串不应该被判断为空。
        assertFalse(stringUtils.isEmpty("hello"), "非空字符串不应该被判断为空");
    }

    @Test
    public void testReverse() {
        // 正常情况：普通字符串应该按字符顺序反转。
        assertEquals("olleh", stringUtils.reverse("hello"), "hello 反转后应该是 olleh");
        // 边界情况：单字符和空字符串反转后都应该保持原样。
        assertEquals("a", stringUtils.reverse("a"), "单个字符反转后应该不变");
        assertEquals("", stringUtils.reverse(""), "空字符串反转后应该仍为空字符串");
    }

    @Test
    public void testReverseWithNull() {
        // 异常情况：reverse 不接受 null，应该抛出指定异常。
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            stringUtils.reverse(null);
        }, "反转 null 应该抛出 IllegalArgumentException");

        assertEquals("字符串不能为 null", exception.getMessage());
    }

    @Test
    public void testConcat() {
        // 正常情况：两个普通字符串直接拼接。
        assertEquals("hello world", stringUtils.concat("hello ", "world"), "两个普通字符串应该正常拼接");
        // 边界情况：空字符串或 null 参与拼接时，不应该导致程序出错。
        assertEquals("hello", stringUtils.concat("hello", ""), "拼接空字符串应该保持原字符串");
        assertEquals("world", stringUtils.concat(null, "world"), "第一个参数为 null 时应按空字符串处理");
        assertEquals("hello", stringUtils.concat("hello", null), "第二个参数为 null 时应按空字符串处理");
        assertEquals("", stringUtils.concat(null, null), "两个参数都为 null 时应返回空字符串");
    }
}
```

### 运行测试

通过如下命令运行测试 `StringUtils`  模块，下图显示，相关测试很快便通过。

```bash
./mvnw -Dtest=StringUtilsTest test
```

![图片.png](https://files.seeusercontent.com/2026/05/19/b5Rs/fa6797b.png)