# Logback User Manual

## 引言

**士气的影响力是惊人的。当任何一个系统能运行时,即使是一个简单的系统,热情就开始出现了。而当一个图形系统能显示图片时，即使那仅仅是一个矩形，这样的热情也会翻倍。类似的情形总是会出现在一个可运行系统发展的各个阶段上。我发现这样的团队可以在四个月内构建出比他们想象中更复杂的系统**

--FREDERICK P. BROOKS, JR., *The Mythical Man-Month*

### logback简述

Logback的设计者Ceki Gülcü(同时也是log4j的创始人)，他的初衷是使其成为流行框架log4j项目的继承者。logback框架基于作者在工业级强度日志系统二十几年来获得的经验设计而成，它比目前所有已经存在的日志系统更加显著的性能提升，更重要的是logback提供了一些其他的日志系统所没有的有用特性。

### 开始的第一步

#### 前期准备

- 为了能够运行本章中的实例，你需要保证的你类路径已经包含了特定的jar包。请到[初始页](https://logback.qos.ch/setup.html)面查询更详细的内容。jar的内容如下:

  - logback-core-1.3.0-alpha4.jar
  - logback-classic-1.3.0-alpha4.jar
  - logback-examples-1.3.0-alpha4.jar
  - slf4j-api-1.8.0-beta1.jar

  其中所有的logback-*.jar包都是logback的模块，而slf4j-api-1.8.0-beta1.jar来自于另外一个项目[SLF4J](http://www.slf4j.org/)

- Logback-classic 模块需要依赖slf4j-api.jar和logback-core.jar才能正常工作

下面让我们开始使用logback的第一个示例

示例1.1：记录日志的基础用法([*(logback-examples/src/main/java/chapters/introduction/HelloWorld1.java)*](https://logback.qos.ch/xref/chapters/introduction/HelloWorld1.html))

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

