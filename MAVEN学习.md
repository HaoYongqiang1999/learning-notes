# Maven学习

## IDEA MAVEN按钮

- 循环箭头按钮：读取POM文件，重新拉取依赖
- 文件夹加循环箭头按钮：更彻底和更大范围的更新，可以改变文件夹结构

## 生命周期（只展示关键阶段）

1. **clean**：清理target目录
2. **default（build）**：
    1. **compile**：编译源码，生成字节码
    2. **test**：测试
    3. **package**：打jar/war包
    4. **install**：安装到本地仓库
    5. **deploy**：推送到远程仓库
3. **site**：生成项目文档、站点，并可发布

> 不同生命周期之间无关系，同一生命周期之间，默认顺序执行

## POM文件

```xml
<properties>
    <java.version>17</java.version>
</properties>
<plugin>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <source>${java.version}</source>
        <target>${java.version}</target>
    </configuration>
</plugin>
```

决定编译和打包时候的jdk版本

```xml
<packaging>jar</packaging>
<packaging>pom</packaging>
```

分别决定打出来是jar包还是POM项目

```xml
<scope>runtime</scope>
```

调用了通用接口，没有直接调用该组件实现的方法，因此编译时不需要，但是运行时需要

```xml
<scope>provided</scope>
```

由容器提供，只在编译和测试时候用到

| Scope    | 编译 | 测试 | 运行 | 打包 | 传递 |
| -------- | ---- | ---- | ---- | ---- | ---- |
| compile  | 是   | 是   | 是   | 是   | 是   |
| runtime  | 否   | 否   | 是   | 是   | 是   |
| provided | 是   | 是   | 否   | 否   | 否   |
| test     | 否   | 是   | 否   | 否   | 否   |

```xml
<optional>true</optional>
```

依赖不传递

###### Build配置说明

- 默认先由maven-jar-plugin打包 class + 资源为普通jar，然后再由spring-boot-maven-plugin **在 jar 的基础上重打包**（repackage）为 Spring Boot fat jar（包含class + 资源+依赖的jar）。
- 如果只是工具类的工程，本身不能独立运行，没有启动类，可以配置SKIP。相当于跳过spring-boot-maven-plugin，但如果parent引入该插件，子工程不配置也会生效，因此必须显示SKIP。

```xml
<build>
    <plugins>
       <plugin>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-maven-plugin</artifactId>
          <configuration>
             <skip>true</skip> <!-- 跳过 repackage -->
          </configuration>
       </plugin>
    </plugins>
</build>
```