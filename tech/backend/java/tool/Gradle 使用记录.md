**1. 引入 bom 的方式**

可以使用 dependency-management-plugin 插件下的 `imports.mavenBom`。
```groovy
dependencyManagement {
	imports {
		mavenBom "org.springframework.cloud:spring-cloud-dependencies：Hoxton.SR8"
	}
}
```
也可以使用 java-platform 插件
```groovy
dependencies {
	api platform("org.springframework.cloud:spring-cloud-dependencies：Hoxton.SR8")
}
```

但以上两种方式均不能引入 spring-boot-dependencies,不知道啥问题，所以就换成了 org.springframework.boot 插件。
> 注意：插件不能传递到子模块

---

**2. 多个模块依赖一个共同的模块**
common 模块需要使用 api 的方式(由 java-library 提供)引入依赖，方能传递依赖。
```groovy
plugins {
	id 'java-library'
}

dependencies {
	api "org.springframework.boot:spring-boot-starter-web"
}
```

---

**3. 使用 Gradld 构建启动项目无法找到依赖其他模块的类**
例如：user-service 模块依赖 common 模块，找不到 common 中的类：需要在 common 模块的 `build.grald` 中添加 `jar.enabled=ture`,方可解决

```ad-warning
注意，加上这个配置以后，生成的jar包就是一个只包含自己模块里面的代码的 jar 包。所以，在最终要运行的 jar 的那个模块中不要使用。假如，我的 common 是一个要执行的 jar 文件(standalone)，就不需要加这个配置。
```


参考: [解决gradle多模块依赖在Idea中能运行，gradle build失败的问题。_easewalk的博客-CSDN博客](https://blog.csdn.net/easewalk/article/details/84867043)