# JUnit简介

## 1、什么是JUnit

​		JUnit 是一个 Java 编程语言的单元测试框架（回归测试框架）。JUnit 在测试驱动的开发方面有很重要的发展，是起源于 JUnit 的一个统称为 xUnit 的单元测试框架之一。

​	    JUnit测试是程序员测试，也就是所谓的白盒测试，需要代码开发者知道测试的功能是如何完成的。、

​		通常我们写完代码想要测试这段代码的正确性，那么必须新建一个类，然后创建一个 main() 方法，然后编写测试代码。如果需要测试的代码很多呢？那么要么就会建很多main() 方法来测试，要么将其全部写在一个 main() 方法里面。这也会大大的增加测试的复杂度，降低程序员的测试积极性。而 Junit 能很好的解决这个问题，简化单元测试，写一点测一点，在编写以后的代码中如果发现问题可以较快的追踪到问题的原因，减小回归错误的纠错难度。

## 2、JUnit功能特性

###   2.1、基本功能

1. @Test，测试方法注释
2. @BeforeClass，测试类执行前操作
3. @AfterClass，测试类执行后操作
4. @Before，@Test方法执行前逻辑
5. @After，@Test方法执行后逻辑
6. @Ignored，忽略@Test方法执行

 其中最常用的就是@Test。

### 2.2、JUnit中的断言机制

​	断言是单元测试最基本，最核心，最重要的概念。使用格式如下：`谓主宾格式`

```
Assert.assertEquals([optional]message, expected, actual);
```

​    JUnit 在后期版本中，增加了更易懂的方式`主谓宾`。引入了`assertThat`，它使用Matcher（匹配器）来完成它的职责，Matcher本质是使用链式编程的方式实现引用代入。

```
assertThat(actual, Matcher<? super T> matcher);
```

[^断言]: 在JDK1.4之后，java中增加了断言的功能。断言就是肯定某一个结果的返回值是正确的，如果最终此结果的返回值是错误的，则通过断言检查肯定是会提示错误信息，断言的定义格式如下：assert boolean表达式；assert boolean表达式 : 详细的信息。 如果最终结果的返回值是true，则什么错误信息都不会提示。如果返回结果是false，则会提示错误信息。如果没有声明详细的描述，则系统会使用默认的的错误信息提示方式。请注意，在java设计assert关键字，考虑到系统的应用，为了防止某些用户使用assert作为关键字，所以在程序正常运行时断言并不会起任何的作用，如果要想要让断言起作用，则在使用java运行时应该加入以下参数：-enableassertions      简写：-ea。
[^链式编程]: 所谓的链式编程就是可以通过"点"语法，将需要执行的代码块连续的书写下去，使得代码简单易读，书写方便。举一个常用的例子，我们给dto设置属性的时候，可以使用链式设置，只需要在lombok增加注释，@Accessors(charn = true) 就可以。同样lombok支持build类型，直接加上@Builder即可。

### 2.3、自定义matcher

​	  junit自带的Matcher并不能完成我们想要完成的匹配，这时我们就需要`自定义Matcher`，以此来用于特定语境下的特定处理。继承org.hamcrest.BaseMatcher实现自定义matcher。

### 2.4、指定测试方法顺序

​		指定方法的执行顺序，可以使用注解FixMethodOrder，参数使用MethodSorter，<是一个枚举>。

## 3、JUnit5 新特性

### 	3.1、架构变化

​		与之间的JUnit架构不同，以前是所有的功能都放到一个Junit一个包中，现在准确来讲分为了三个包，关系可以用下面的表达式来描述：

​		`JUnit5 = JUnit Platform + JUnit Jupiter + JUnit Vintage`

**JUnit platform** ： Junit已不仅仅想简单作为一个测试框架，更多是希望能作为一个测试平台，也就是说通过JUnit platform，其他的自动化测试引擎或自己定制的引擎都可以接入这个平台实现对接和执行。

**JUnit Jupiter**： 则是Junit5的核心，它包含了很多丰富的新特性来使JUnit自动化测试更加方便、功能更加丰富和强大。

**JUnit Vintage**： 则是一个对JUnit3，JUnit4兼容的测试引擎，使旧版本junit的自动化测试脚本可以顺畅运行在junit5下。

### 3.2、新特性

​	除了老版本的常规断言之外，新版本支持了AssertThrow和AssertAll两种断言。

​	@Tag标签支持，测试执行的时候可以根据标签来选择执行。

3.3、新老注解比对

|                       FEATURE                        |   JUNIT 4    |   JUNIT 5    |
| :--------------------------------------------------: | :----------: | :----------: |
|                Declare a test method                 |    @Test     |     @Te      |
| Execute before all test methods in the current class | @BeforeClass |  @BeforeAll  |
| Execute after all test methods in the current class  | @AfterClass  |  @AfterAll   |
|           Execute before each test method            |   @Before    | @BeforeEach  |
|            Execute after each test method            |    @After    |  @AfterEach  |
|            Disable a test method / class             |   @Ignore    |  @Disabled   |
|            Test factory for dynamic tests            |      NA      | @TestFactory |
|                     Nested tests                     |      NA      |   @Nested    |
|                Tagging and filtering                 |  @Category   |     @Tag     |

