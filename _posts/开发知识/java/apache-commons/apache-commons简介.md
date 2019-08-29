# apache-commons简介

​	Apache Commons是Apache软件基金会的项目，曾隶属于Jakarta项目。Commons的目的是提供可重用的、开源的Java代码。

Apache Commons项目的由三部分组成：

1. The Commons Proper - 一个可重用的[Java](https://baike.baidu.com/item/Java/85979)组件库。(已经发布过的)
2. The Commons Sandbox - Java组件开发工作区. (正在开发的项目)
3. The Commons Dormant - 当前处于非活动状态的组件库.(刚启动或者已经停止维护的项目)

建立和维护可重用的[Java](https://baike.baidu.com/item/Java/85979)组件。使用组件可以提高开发效率和质量。

apache-commons官方地址：<http://commons.apache.org/index.html>

截止到目前，大概有44个包，每一个包都提供相当遍历的功能。

| Components                                                   | Description                                                  | Latest Version   |  Released  |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------- | :--------: |
| [BCEL](http://commons.apache.org/proper/commons-bcel/)       | Byte Code Engineering Library - analyze, create, and manipulate Java class files | 6.3.1            | 2019-03-24 |
| [BeanUtils](http://commons.apache.org/proper/commons-beanutils/) | Easy-to-use wrappers around the Java reflection and introspection APIs. | 1.9.3            | 2016-09-26 |
| [BSF](http://commons.apache.org/proper/commons-bsf/)         | Bean Scripting Framework - interface to scripting languages, including JSR-223 | 3.1              | 2010-06-24 |
| [Chain](http://commons.apache.org/proper/commons-chain/)     | *Chain of Responsibility* pattern implemention.              | 1.2              | 2008-06-02 |
| [CLI](http://commons.apache.org/proper/commons-cli/)         | Command Line arguments parser.                               | 1.4              | 2017-03-09 |
| [Codec](http://commons.apache.org/proper/commons-codec/)     | General encoding/decoding algorithms (for example phonetic, base64, URL). | 1.12             | 2019-02-16 |
| [Collections](http://commons.apache.org/proper/commons-collections/) | Extends or augments the Java Collections Framework.          | 4.4              | 2019-07-08 |
| [Compress](http://commons.apache.org/proper/commons-compress/) | Defines an API for working with tar, zip and bzip2 files.    | 1.18             | 2018-08-16 |
| [Configuration](http://commons.apache.org/proper/commons-configuration/) | Reading of configuration/preferences files in various formats. | 2.5              | 2019-05-27 |
| [Crypto](http://commons.apache.org/proper/commons-crypto/)   | A cryptographic library optimized with AES-NI wrapping Openssl or JCE algorithm implementations. | 1.0.0            | 2016-07-22 |
| [CSV](http://commons.apache.org/proper/commons-csv/)         | Component for reading and writing comma separated value files. | 1.7              | 2019-06-05 |
| [Daemon](http://commons.apache.org/proper/commons-daemon/)   | Alternative invocation mechanism for unix-daemon-like java code. | 1.0.15           | 2013-04-03 |
| [DBCP](http://commons.apache.org/proper/commons-dbcp/)       | Database connection pooling services.                        | 2.6.0            | 2019-02-19 |
| [DbUtils](http://commons.apache.org/proper/commons-dbutils/) | JDBC helper library.                                         | 1.7              | 2017-07-20 |
| [Digester](http://commons.apache.org/proper/commons-digester/) | XML-to-Java-object mapping utility.                          | 3.2              | 2011-12-13 |
| [Email](http://commons.apache.org/proper/commons-email/)     | Library for sending e-mail from Java.                        | 1.5              | 2017-08-01 |
| [Exec](http://commons.apache.org/proper/commons-exec/)       | API for dealing with external process execution and environment management in Java. | 1.3              | 2014-11-06 |
| [FileUpload](http://commons.apache.org/proper/commons-fileupload/) | File upload capability for your servlets and web applications. | 1.4              | 2019-01-16 |
| [Functor](http://commons.apache.org/proper/commons-functor/) | A functor is a function that can be manipulated as an object, or an object representing a single, generic function. | 1.0              | 2011-??-?? |
| [Geometry](http://commons.apache.org/proper/commons-geometry/) | Space and coordinates.                                       | 1.0              | 2018-??-?? |
| [Imaging (previously called Sanselan)](http://commons.apache.org/proper/commons-imaging/) | A pure-Java image library.                                   | 1.0-alpha1       | 2019-05-02 |
| [IO](http://commons.apache.org/proper/commons-io/)           | Collection of I/O utilities.                                 | 2.6              | 2017-10-15 |
| [JCI](http://commons.apache.org/proper/commons-jci/)         | Java Compiler Interface                                      | 1.1              | 2013-10-14 |
| [JCS](http://commons.apache.org/proper/commons-jcs/)         | Java Caching System                                          | 2.2,1            | 2018-08-23 |
| [Jelly](http://commons.apache.org/proper/commons-jelly/)     | XML based scripting and processing engine.                   | 1.0.1            | 2017-09-27 |
| [Jexl](http://commons.apache.org/proper/commons-jexl/)       | Expression language which extends the Expression Language of the JSTL. | 3.1              | 2017-04-14 |
| [JXPath](http://commons.apache.org/proper/commons-jxpath/)   | Utilities for manipulating Java Beans using the XPath syntax. | 1.3              | 2008-08-14 |
| [Lang](http://commons.apache.org/proper/commons-lang/)       | Provides extra functionality for classes in java.lang.       | 3.9              | 2019-04-15 |
| [Logging](http://commons.apache.org/proper/commons-logging/) | Wrapper around a variety of logging API implementations.     | 1.2              | 2014-07-11 |
| [Math](http://commons.apache.org/proper/commons-math/)       | Lightweight, self-contained mathematics and statistics components. | 3.5              | 2015-04-17 |
| [Net](http://commons.apache.org/proper/commons-net/)         | Collection of network utilities and protocol implementations. | 3.6              | 2017-02-15 |
| [Numbers](http://commons.apache.org/proper/commons-numbers/) | Number types (complex, quaternion, fraction) and utilities (arrays, combinatorics). | 1.0              | 2017-??-?? |
| [OGNL](http://commons.apache.org/proper/commons-ognl/)       | An Object-Graph Navigation Language                          | 4.0              | 2013-??-?? |
| [Pool](http://commons.apache.org/proper/commons-pool/)       | Generic object pooling component.                            | 2.6.2            | 2019-04-11 |
| [Proxy](http://commons.apache.org/proper/commons-proxy/)     | Library for creating dynamic proxies.                        | 1.0              | 2008-02-28 |
| [RDF](http://commons.apache.org/proper/commons-rdf/)         | Common implementation of RDF 1.1 that could be implemented by systems on the JVM. | 0.3.0-incubating | 2016-11-15 |
| [RNG](http://commons.apache.org/proper/commons-rng/)         | Implementations of random numbers generators.                | 1.2              | 2018-12-12 |
| [SCXML](http://commons.apache.org/proper/commons-scxml/)     | An implementation of the State Chart XML specification aimed at creating and maintaining a Java SCXML engine. It is capable of executing a state machine defined using a SCXML document, and abstracts out the environment interfaces. | 0.9              | 2008-12-01 |
| [Statistics](http://commons.apache.org/proper/commons-statistics/) | Statistics.                                                  | 0.1              | ????-??-?? |
| [Text](http://commons.apache.org/proper/commons-text/)       | Apache Commons Text is a library focused on algorithms working on strings. | 1.7              | 2019-07-03 |
| [Validator](http://commons.apache.org/proper/commons-validator/) | Framework to define validators and validation rules in an xml file. | 1.6              | 2017-02-21 |
| [VFS](http://commons.apache.org/proper/commons-vfs/)         | Virtual File System component for treating files, FTP, SMB, ZIP and such like as a single logical file system. | 2.4              | 2019-07-17 |
| [Weaver](http://commons.apache.org/proper/commons-weaver/)   | Provides an easy way to enhance (weave) compiled bytecode.   | 2.0              | 2018-09-07 |

## 1、常用的apache-commons包

​	我们只能对一些常用的commons的包进行总结和分析

### 1.1、lang包

​	maven坐标：

```
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-lang3</artifactId>
  <version>3.9</version>
</dependency>
```

​	javadoc地址：<http://commons.apache.org/proper/commons-lang/javadocs/api-3.9/index.html>

​	apache-commons-lang包为java.lang包提供了大量的辅助工具，特别是字符串的操作，基本数值方法，对象的反射，并发性，创建和序列化以及系统属性，此外还有对java.utils.Date的基本增强功能以及一系列用于帮助构建方法的实用程序，如hashcode，toString，equals。

   看一下lang下面主要有哪些包：

1. org.apache.commons.lang3 对应了java.lang下很多类做的工具类，像AnnotationUtils，ArrrayUtils,StringUtils，CharSetUtils，EnumUtils等
2. org.apache.commons.lang3.arch  枚举 定义了处理器的类型 （用不上）
3. org.apache.commons.lang3.builder 构造，提供了实现链式构造类型的对象创建方式，包括有一些实现，如CompareToBuilder，DiffBuilder，EqualsBuilder,HashCodeBuilder等等。
4. org.apache.commons.lang3.concurrent 多线程并发实现
5. org.apache.commons.lang3.event  事件监听实现。
6. org.apache.commons.lang3.exception 异常信息上下文，异常工具类。
7. org.apache.commons.lang3.math  计算工具类NumberUtils，创建，比较，取大，取小，转换等。
8. org.apache.commons.lang3.mutable  提供对基本类型的可变操作。
9. org.apache.commons.lang3.reflect  反射增强，
10. org.apache.commons.lang3.text 对内容格式限定，上层接口FormatFactory 
11. org.apache.commons.lang3.text.translate 内容转换，内容构建。
12. org.apache.commons.lang3.time  时间，日期格式化，处理工具。
13. org.apache.commons.lang3.tuple  提供元组的实现 ，一个pair就是一组，分为左右节点。

​	

[^元组]: 元组 元组（tuple）是关系数据库中的基本概念，关系是一张表，表中的每行（即数据库中的每条记录）就是一个元组，每列就是一个属性。 在二维表里，元组也称为行。

#### lang包常用功能：

1. ArrayUtils ;提供了对数组的操作，如添加，查找，删除，子数组，倒序，元素类型转换等。
2. CharUtils，用于操作char的值，和Character对象互转，判断是否是Ascii码等。
3. ClassPathUtils：处理类路径的一些工具方法
4. ClassUtils：用于对Java类的操作，像获取类名称，包名称，获取所有的父类，获取所有的接口、
5. EnumUtils：对枚举的工具类进行操作
6. JavaVersion： 一个java版本的枚举类
7. RandomStringUtils：随机字符串生成工具类。
8. RandomUtils：随机数字，int，long，float等。
9. RegExUtils：使用正则处理一些字符串操作，替换，删除等。
10. SystenUtils：定义了一些系统底层的常量，比如类路径，操作系统，类型，java版本等等。
11. StringUtils：不多说，很全，几乎涵盖了现行项目使用的所有的对String的操作要求。

### 1.2、io包

maven坐标：

```
<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```

javadoc地址：<http://commons.apache.org/proper/commons-io/javadocs/api-release/index.html>

