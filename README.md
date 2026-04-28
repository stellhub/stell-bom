# stell-bom

`stell-bom` 是 Stell Java 业务工程和中间件 SDK 的基础依赖版本管理仓库。

它的职责是统一维护公共组件版本，业务项目只需要继承或导入这个 BOM，即可在不显式声明版本号的前提下使用约定好的依赖版本，避免版本漂移和冲突。

## 适用场景

- Java 业务服务项目
- 中间件 SDK 项目
- 需要统一依赖版本的多模块工程

## 已管理的依赖版本

当前 BOM 统一管理以下依赖版本：

| Dependency | Version |
| --- | --- |
| JUnit | `5.12.2` |
| Lombok | `1.18.46` |
| OpenTelemetry | `1.49.0` |
| SLF4J | `2.0.17` |
| Logback | `1.5.32` |
| Netty | `4.1.132.Final` |
| Jackson | `2.21.2` |
| gRPC | `1.80.0` |
| OkHttp | `5.3.2` |

## 使用方式

推荐在业务工程的 `pom.xml` 中通过 `dependencyManagement` 导入本 BOM。

### 1. 导入 BOM

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.github.stellhub</groupId>
            <artifactId>stell-bom</artifactId>
            <version>0.0.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 2. 业务依赖不再声明版本号

导入 BOM 后，业务工程可以直接声明依赖而不写 `<version>`。

```xml
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>provided</scope>
    </dependency>

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
    </dependency>

    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
    </dependency>

    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
    </dependency>

    <dependency>
        <groupId>io.grpc</groupId>
        <artifactId>grpc-stub</artifactId>
    </dependency>

    <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
    </dependency>

    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## 作为父工程继承

如果你的项目本身已经有自己的父 POM，通常应优先使用 `dependencyManagement import` 方式。

如果项目没有其它父 POM，也可以直接继承本工程：

```xml
<parent>
    <groupId>io.github.stellhub</groupId>
    <artifactId>stell-bom</artifactId>
    <version>0.0.1</version>
    <relativePath/>
</parent>
```

但对于大多数业务项目，仍然更推荐使用 `import BOM`，这样不会限制业务项目自定义自己的父工程结构。

## 版本覆盖

如果某个业务项目确实需要临时覆盖某个依赖版本，可以在自己的 `dependencyManagement` 中重新声明该依赖，后声明的版本会生效。

示例：

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.github.stellhub</groupId>
            <artifactId>stell-bom</artifactId>
            <version>0.0.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.21.3</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## 发布后使用

将本项目发布到 Maven 私服或公共仓库后，其它项目即可直接引用：

```xml
<dependency>
    <groupId>io.github.stellhub</groupId>
    <artifactId>stell-bom</artifactId>
    <version>0.0.1</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

## 建议

- 业务工程不要重复声明已被 BOM 管理的版本号
- 新增公共依赖版本时，优先维护到 `stell-bom`
- 中间件 SDK 与业务服务尽量共用同一套 BOM，降低依赖冲突概率
