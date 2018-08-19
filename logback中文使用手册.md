# Logback User Manual

## 引言

**士气的影响力是惊人的。任何一个使是最简单系统的运行,也足以振奋人心。而当一个图形系统能显示图片时，即使那仅仅是一个矩形，这样的热情也会翻倍。类似的情形总是会出现在一个可运行系统发展的各个阶段上。我发现这样的团队可以在四个月内构建出比他们想象中更复杂的系统**

--FREDERICK P. BROOKS, JR., *The Mythical Man-Month*

### logback简述

Logback的设计者Ceki Gülcü(同时也是log4j的创始人)，他的初衷是使其成为流行框架log4j项目的继承者。logback框架基于作者在工业级强度日志系统十几年来的经验设计而成，它比目前所有已经存在的日志系统更加显著的性能提升，更重要的是logback提供了一些其他的日志系统所没有的有用特性。

### 开始的第一步

#### 前期准备

- 为了能够运行本章中的实例，你需要保证的你类路径已经包含了特定的jar包。请到[初始页](https://logback.qos.ch/setup.html)面查看更详细的内容。jar的内容如下:

  - logback-core-1.3.0-alpha4.jar
  - logback-classic-1.3.0-alpha4.jar
  - logback-examples-1.3.0-alpha4.jar
  - slf4j-api-1.8.0-beta1.jar

  其中所有的logback-*.jar包都是logback的模块，而slf4j-api-1.8.0-beta1.jar来自于另外一个项目[SLF4J](http://www.slf4j.org/)

- Logback-classic 模块需要依赖slf4j-api.jar和logback-core.jar才能正常工作

下面让我们开始使用logback的第一个示例

##### 示例1.1：记录日志的基本用法([*(logback-examples/src/main/java/chapters/introduction/HelloWorld1.java)*](https://logback.qos.ch/xref/chapters/introduction/HelloWorld1.html))

```java
package chapters.introduction;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld1 {

  public static void main(String[] args) {

    Logger logger = LoggerFactory.getLogger("chapters.introduction.HelloWorld1");
    logger.debug("Hello world.");

  }
}
```

*HelloWorld1*类被定义在chapters.introduction包下，他的main方法中使用了SELF4J API中定义Logger类和LoggerFactory工厂方法。

分析这个简单的例子我们可以发现main方法中声明了一个打印DEBUG级别内容“Hello world”的日志。具体的过程是：main方法首先调用*LoggerFactory*工厂的静态方法创建了一个*Logger*类的实例*logger*，它名字是“chapters.introduction.HelloWorld1”，接下来调用了logger的debug方法，同时以"Hello world"作为方法入参。

值得注意的是，上面的例子没有引用任何一个logback的类。在大多数情况下，当你需要在你的类中考虑使用日志的时候，你只需要引入SLF4J的类就足够了。因此，如果你的代码使用SLF4J API,你可以完全忽略logback的存在。

你可以使用如下的java 命令来运行上面的示例

```shell
java chapters.introduction.HelloWorld1
```

运行成功之后，你的终端将获得如下的内容

```java
01:01:53.497 [main] DEBUG chapters.introduction.HelloWorld1 - Hello world.
```

根据logback的默认配置策略，当没有默认的配置文件（etc. Logback.xml等），框架会默认添加一个 ConsoleAppender 作为根日志。

Logback可以同过内建的转改系统来报告其状态。StatusManager组件可以汇报logback声明周期内重要的事件，现在，让我们通过调用StatusPrinter类的print()方法来打印logback的内部状态吧

##### 示例1.2：打印日志状态([logback-examples/src/main/java/chapters/introduction/HelloWorld2.java](https://logback.qos.ch/xref/chapters/introduction/HelloWorld2.html))

```java
package chapters.introduction;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import ch.qos.logback.classic.LoggerContext;
import ch.qos.logback.core.util.StatusPrinter;

public class HelloWorld2 {

  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger("chapters.introduction.HelloWorld2");
    logger.debug("Hello world.");

    // print internal state
    LoggerContext lc = (LoggerContext) LoggerFactory.getILoggerFactory();
    StatusPrinter.print(lc);
  }
}	
```

运行Helloworld2将会产生如下的日志输出

```
21:53:15.159 [main] DEBUG chapters.introduction.HelloWorld2 - Hello world.
21:53:15,082 |-INFO in ch.qos.logback.classic.LoggerContext[default] - Could NOT find resource [logback-test.xml]
21:53:15,083 |-INFO in ch.qos.logback.classic.LoggerContext[default] - Could NOT find resource [logback.groovy]
21:53:15,083 |-INFO in ch.qos.logback.classic.LoggerContext[default] - Could NOT find resource [logback.xml]
21:53:15,092 |-INFO in ch.qos.logback.classic.BasicConfigurator@9536ecf - Setting up default configuration.
```

上面的日志表明Logback没有找到 logback-test.xml或者logback.xml配置文件（稍后解释），于是它使用了默认的配置策略：使用ConsoleAppender（Apender类决定了输出日志的方式），Appenders使日志可以通过很多方式输出，包括控制台、文件、系统日志、TCP套接字，JMS甚至更多，使用者甚至可以根据情况定制自己的Appender。

 **注意：**logback会自动打印内部的错误到控制台。

虽然实例1.2相对来说比较简单，然而即使对于一个稍大的应用来说，日志声明模式也不会有什么改变，只是日志的配置过程会有一些不同。如果你想根据自己的需要来定制化logback，接下来章节会给你提供帮助。

在上面的例子里，我们看到logback通过调用静态方法StatusPrinter.print()来打印其内部状态状态。**注意：**

了解logback的内部状态对于诊断logback相关的问题非常有用。

下面的列表说明在你的应用中使用logback的必要步骤：

1. 配置logback环境，方式可繁可简，点击此处查看.
2. 在需要使用日志的类中通过调用org.slf4j.LoggerFactory.getLogger()方法获得Logger实例来记录日志，此方法允许你传入类名或者类本身。
3. 用过调用Logger类的debug()，info()，warn()，error() 方法类记录不同级别的日志，记录的内容将通过配置的appnder进行输出。

#### 构建logback

logback通过[Maven](http://maven.apache.org/)来构建，如果你已经安装Maven，解压Logback发行包，你可以用过mvn install来构建logback项目中所有的模块，Maven会自动下载logback所依赖的所有类库。

logback发行包中包含了所有的源代码，在遵从LGPL或者EPL开源许可下，你可以修改l任意代码甚至构建、发布你自己的版本。

对于logback在IDE（集成开发环境）下的构建，请查看[类路径设置的相关章节](https://logback.qos.ch/setup.html#ide)。