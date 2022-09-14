## Gradle 简介
[Gradle](https://gradle.org/)是一款Google 推出的**基于 JVM**、通用灵活的项目构建工具，支持 Maven，JCenter 多种第三方仓库;支持传递性依赖管理、废弃了繁杂的xml 文件，转而使用**简洁的**、**支持多种语言**(例如：java、groovy 等)的 **build 脚本文件**。

学习 Gradle 的原因：
1.  目前已经有相当一部分公司在逐渐使用Gradle作为项目构建工具了。
2.  作为Java开发程序员,如果想下载Spring、SpringBoot等Spring家族的源码，基本上基于Gradle构建的。

| 项目构建工具对比   | Ant              | Maven        | Gradle                   |
| ------------------ | ---------------- | ------------ | ------------------------ |
| 构建性能           | 最高             | 最低         | 居中                     |
| 仓库               | 开发者自己处理   | Maven 仓库   | 支持多种远程仓库         |
| 依赖管理           | ivy 管理         | GAV 坐标管理 | GNV 坐标管理             |
| 插件支持           | 实现方便         | 实现较难     | 实现方便                 |
| 遵循特定的目录结构 | No               | 遵循         | 同 Maven 一样            |
| 配置文件           | XML 文件最为繁琐 | XML 文件     | 代码脚本，便于写业务逻辑 |
| 侧重点             | 小型项目构建     | 项目包管理   | 大型项目构建             |
| 目前地位           | 使用较少         | 目前主流     | 未来趋势(Spring家族)     | 

> <font weight="500">总之，虽然目前市面上常见的项目构建工具有 Ant、Maven、Gradle，主流还是Maven，但是未来趋势 Gradle。</font>

## Gradle 安装
[Spring Boot](https://docs.spring.io/spring-boot/docs/2.5.0/gradle-plugin/reference/htmlsingle/#getting-started) 官方文档明确指出,目前 SpringBoot 的 Gradle 插件需要 Gradle6.8 版本及以上，所以我们这里选择 7.x 版本。
![[Pasted image 20220907133358.png]]
其中SpringBoot 与 Gradle 存在版本兼容问题，Gradle 与 Idea 也存在兼容问题，所以考虑到 Java 程序员会使用SpringBoot，所以要选择 6.8 版本及高于 6.8 版本的 Gradle,那么相应的 Idea 版本也要升级,不能太老哦。

> 下载完成之后配置环境变量：`GRADLE_HOME` 和 `GRADLE_USER_HOME`。`GRADLE_USER_HOME` 相当于配置了 Gradle 的本地仓库位置和 Gradle Wrapper 缓存目录。


## 开始 Gradle 项目
### Gradle 项目结构
Gradle 项目**默认目录结构和 Maven 项目的目录结构一致**,都是基于**约定大于配置** (Convention Over Configuration)。其完整项目目录结构如下所示：
![600](image.jpeg)

### Gradle 中常用指令
| 常用 Gradle 指令     | 作用                       |
| -------------------- | -------------------------- |
| gradle clean         | 清空 build 目录            |
| gradle classes       | 编译业务代码和配置文件     |
| gradle test          | 编译测试代码, 生成测试报告 |
| gradle build         | 构建项目, 并测试           |
| gradle build -x test | 跳过测试构建               |

> 需要注意的是：`gradle` 的指令要在含有 `build.gradle` 的目录中执行

### 修改maven 下载源
Gradle 自带的 Maven 源地址是国外的，需要修改 Maven 源。在 Gradle 目录的 `init.d` 文件加下创建以为 `.gradle` 结尾的文件(**.gradle 文件在 build 开始前执行**), 添加如下配置:
```groovy
allprojects {
    repositories {
        mavenLocal()
        maven { name "Alibaba" ; url "https://maven.aliyun.com/repository/public" } 
        maven { name "Bstek" ; url "https://nexus.bsdn.org/content/groups/public/" } 
        mavenCentral()
    }
    
    buildscript {
        repositories {
            maven { name "Alibaba" ; url 'https://maven.aliyun.com/repository/public' } 
            maven { name "Bstek" ; url 'https://nexus.bsdn.org/content/groups/public/' } 
            maven { name "M2" ; url 'https://plugins.gradle.org/m2/' }
        }
    }
}
```

Gradle 可以通过指定仓库地址为本地 Maven 仓库地址和远程仓库地址相结合的方式，避免每次都会去远程仓库下载依赖库。这种方式也有一定的问题，如果本地 Maven 仓库有这个依赖，就会从直接加载本地依赖，如果本地仓库没有该依赖，那么还是会从远程下载。但是下载的 jar 不是存储在本地 Maven 仓库中，而是放在自己的缓存目录中，默认在 `USER_HOME/.gradle/caches` 目录,当然如果我们配置过 `GRADLE_USER_HOME` 环境变量，则会放在 `GRADLE_USER_HOME/caches`目录。不可以将 Gradle caches 指向 Maven repository，caches下载文件不是按照 Maven 仓库中存放的方式。

### Wrapper 包装器
Gradle Wrapper 实际上就是对 Gradle 的一层包装，用于解决实际开发中可能会遇到的不同的项目需要不同版本的 Gradle 问题，例如：把自己的代码共享给其他人使用，可能出现如下情况:
1.  对方电脑没有安装 gradle
2.  对方电脑安装过 gradle，但是版本太旧了

这时候，我们就可以考虑使用 Gradle Wrapper 了。这也是官方建议使用 Gradle Wrapper 的原因。实际上有了 Gradle Wrapper 之后，我们本地是可以不配置 Gradle 的,下载Gradle 项目后，使用 gradle 项目自带的wrapper 操作也是可以的。

**GradleWrapper 的执行流程：**
1.  当我们第一次执行 `./gradlew build` 命令的时候，gradlew 会读取 gradle-wrapper.properties 文件的配置信息
2.  准确的将指定版本的 Gradle 下载并解压到指定的位置(`GRADLE_USER_HOME` 目录下的 wrapper/dists 目录中)
3. 构建本地缓存(`GRADLE_USER_HOME` 目录下的 caches 目录中),下载再使用相同版本的 Gradle 就不用下载了
4. 之后执行的 `./gradlew` 所有命令都是使用指定的 Gradle 版本。

**gradle-wrapper.properties 文件解读 **
![700](image2.jpeg)
> 注意：前面提到的 GRALE_USER_HOME 环境变量用于这里的 Gradle Wrapper 下载的特定版本的 gradle 存储目录。如果我们没有配置过 GRALE_USER_HOME 环境变量,默认在当前用户家目录下的 .gradle 文件夹中。

### 
