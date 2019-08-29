# Gradle入门

​		Gradle是一个基于JVM的构建工具，是一款通用灵活的构建工具，支持maven， Ivy仓库，支持传递性依赖管理，而不需要远程仓库或者是pom.xml和ivy.xml配置文件，基于Groovy，build脚本使用Groovy编写。

​     Gradle脚本不是像传统的xml文件那样，而是一种基于Groovy的动态DSL，而Groovy语言是一种基于jvm的动态语言。 基于这种设计呢， gradle是支持我们像写脚本一样的去写项目的构建规则。

## 1、gradle的基本组成

### 	1.1、project 和task

   对于gradle来讲，每一个待构建的工程就是一个project，构建一个project需要执行一系列Task，比如编译，打包这些构建过程的子过程多对应一个task。

###    1.2、插件

​	gradle中的插件有两个核心工作，一个是定义Task，一个是执行Task。

## 2、安装gradle

1. 从 官网 (https://gradle.org/install/)下载二进制文件。
2. 解压Zip文件，加入环境变量（在PATH中加入GRADLE_HOME/bin目录）。
3. 执行 gradle -h 返回gradle 操作API即说明完成安装。

## 3、gradle使用









​	