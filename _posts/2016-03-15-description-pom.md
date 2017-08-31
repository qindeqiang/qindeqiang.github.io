---
author: Deqiang Qin
title:  POM文件详解
featimg: pom.jpg
---


## POM文件详解

**目录**

[TOCM]

[TOC]

### 概述

该文档主要是整体分析POM文件的各个功能模块，并详细讲解全部标签的作用，代表的意义和使用方法。

Maven主要的功能是解决依赖的关系；

### pom.xml

POM=Project Object Model 即项目对象模型。

Pom文件中主要包含：项目信息(如版本、成员)项目的依赖、插件和goal、build选项；

大型的项目中通常包含了多个模块，模块之间的pom文件可以继承。


参考：http://www.trinea.cn/android/maven/

### 基础设置

基础设置主要包含的标签如下：
```xml
  <groupId>…</groupId>  
  <artifactId>…</artifactId>  
  <version>…</version>  
  <packaging>…</packaging>  
  <parent>…</parent>    
  <modules>…</modules>  
  <properties>…</properties>  
```

+ groupId : 组织标识，例如：org.codehaus.mojo，在M2_REPO目录下，将是: org/codehaus/mojo目录。
+ artifactId : 项目名称，例如：my-project，在M2_REPO目录下，将是：org/codehaus/mojo/my-project目录。
+ version : 版本号，例如：1.0，在M2_REPO目录下，将是：org/codehaus/mojo/my-project/1.0目录。
+ packaging : 打包的格式，可以为：pom , jar , maven-plugin , ejb , war , ear , rar , par
+ parent : 表示该POM文件继承于哪个POM，子POM文件除了继承父POM的依赖，还继承了其他的，如：developers and contributors、plugin lists、reports lists、plugin executions with matching ids、plugin configuration。具体使用案例如下：

```xml
    <parent>
        <groupId>org.apache</groupId>
        <artifactId>apache</artifactId>
        <version>17</version>
        <relativePath />
    </parent>
```

+ modules:用来指定该项目还包含哪些子模块，如下面：
```xml
<modules>
        <module>nifi-commons</module>
        <module>nifi-api</module>
</modules>
```

### 依赖设置
依赖设置主要对应的标签如下：
```xml
<dependencyManagement>…</dependencyManagement>
<dependencies>…</dependencies>
```

+ dependencyManagement :该标签主要是在父POM中使用。用于管理子模块对依赖的实际引用。在该标签中定义的依赖在子模块中并不是实际的被引用了。只有当子模块的POM文件中重新引入了才是真正的引用，此时可以不用指定版本。关于dependencyManagement的使用可以参考文档：http://www.cnblogs.com/youzhibing/p/5427130.html。如下案例：

```xml
modules>
    <!-- 模块都写在此处 -->
    <module>account-register</module>
    <module>account-persist</module>
</modules>

<dependencyManagement>
    <dependencies> <!-- 配置共有依赖 -->
    <!-- spring 依赖 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>4.0.2.RELEASE</version>
  </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>4.0.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.0.2.RELEASE</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>4.0.2.RELEASE</version>
  </dependency>
  
    <!-- junit 依赖 -->
    <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.7</version>
    <scope>test</scope>
  </dependency>
</dependencies>
</dependencyManagement>
```
+ dependencies: 主要来包含全部的依赖
```xml
<dependencies>
      <!-- spring 依赖，使用共有的依赖的时候可以不用指定版本了 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
    </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
    </dependency>
    
      <!-- junit 依赖 -->
      <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>4.0.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.0.16</version>
    </dependency>
</dependencies>
```

在依赖设置中，可以将所有的所有的依赖放到该标签下，每一个子依赖的模型如下：
```xml
<dependency>
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <scope>...</scope>
    <version>...</version>
</dependency>
```

### Build 编译设置
```xml
<build>…</build>  
<reporting>…</reporting> 
```
build下主要有两种标签：Resources和Plugins。

#### Resources 资源文件管理文件

resources 标签主要臃余排除或者包含某些资源文件，使用案例如下：
```xml
<resources>  
  <resource>  
    <targetPath>META-INF/plexus</targetPath>  
    <filtering>false</filtering>  
    <directory>${basedir}/src/main/plexus</directory>  
    <includes>  
      <include>configuration.xml</include>  
    </includes>  
    <excludes>  
      <exclude>**/*.properties</exclude>  
    </excludes>  
  </resource>  
</resources> 
```

#### Plugins 项目构建插件
使用案例如下：
```xml
<plugins>  
     <plugin>  
       <groupId>org.apache.maven.plugins</groupId>  
       <artifactId>maven-jar-plugin</artifactId>  
       <version>2.0</version>  
       <extensions>false</extensions>  
       <inherited>true</inherited>  
       <configuration>  
         <classifier>test</classifier>  
       </configuration>  
       <dependencies>…</dependencies>  
       <executions>…</executions>  
     </plugin> 
</plugins>
```

插件有很多，但是maven插件是最常用的。

插件是maven的核心，所有执行的操作都是基于插件来完成的

使用插件的时候同样需要指定插件的version，groupId,artifactId等；

##### maven的插件列表
常用的插件列表：http://maven.apache.org/plugins/index.html

常用的插件的介绍：

+ maven-archetype-plugin

+ maven-assembly-plugin

```
maven-assembly-plugin的用途是制作项目分发包，该分发包可能包含了项目的可执行文件、源代码、readme、平台脚本等等。
maven-assembly-plugin支持各种主流的格式如zip、tar.gz、jar和war等，具体打包哪些文件是高度可控的.
例如用户可以按文件级别的粒度、文件集级别的粒度、模块级别的粒度、以及依赖级别的粒度控制打包，此外，包含和排除配置也是支持的。
maven-assembly-plugin要求用户使用一个名为assembly.xml的元数据文件来表述打包，
它的single目标可以直接在命令行调用，也可以被绑定至生命周期。

```

+ maven-release-plugin 

+ maven-compiler-plugin：主要用来编译源代码；

##### maven插件的两种调用方式：

参考文档：http://www.infoq.com/cn/news/2011/04/xxb-maven-7-plugin

###### 方式一：

###### 方式二：

### 环境设置
环境设置对应的标签列表主要如下：
```xml
  <issueManagement>…</issueManagement>  
  <ciManagement>…</ciManagement>  
  <mailingLists>…</mailingLists>  
  <scm>…</scm>  
  <prerequisites>…</prerequisites>  
  <repositories>…</repositories>  
  <pluginRepositories>…</pluginRepositories>  
  <distributionManagement>…</distributionManagement>  
  <profiles>…</profiles> 
```
###### Plugin
``` xml
<plugin>
<!-- //插件的坐标 -->
<groupId>xxx</groupId>
<artifactId>xxx</artifactId>
<version>xxxx</version>
<executions>
  <execution>
    <!-- //指定绑定到compile阶段 -->
    <phase>compile</phase>
    <!-- //指定运行该插件的aaa目标 -->
    <goals>
      <goal>aaa</goal>
    </goals>
    <!-- //为插件的目标配置参数 -->
    <configuration>
      <mainClass>xxx.App</mainClass>
    </configuration>
  </execution>
</executions>
</plugin>
参考：https://www.zhihu.com/question/60204814
```
