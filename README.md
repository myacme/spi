# Java SPI 服务发现机制演示项目

## 项目概述

这是一个基于Java标准SPI（Service Provider Interface）机制的轻量级演示项目，用于展示Java原生的服务发现与动态加载功能。项目通过简单的HelloService接口演示了如何实现插件化架构和服务的运行时发现。

## 技术栈

- **开发语言**: Java 8
- **构建工具**: Maven
- **核心机制**: Java SPI (Service Provider Interface)
- **服务加载器**: `java.util.ServiceLoader`

## 项目结构

```
spi/
├── src/main/java/
│   ├── groupId/
│   │   └── Main.java                  # 程序入口点
│   └── spi/
│       ├── GetImpl.java               # SPI服务加载演示
│       └── service/
│           ├── HelloService.java      # 服务接口定义
│           ├── HelloServiceImpl1.java # 服务实现1
│           └── HelloServiceImpl2.java # 服务实现2
├── src/main/resources/
│   └── META-INF/services/
│       └── spi.service.HelloService   # 服务提供者配置文件
└── pom.xml                           # Maven配置文件
```

## 核心组件

### 1. 服务接口 (HelloService)
```java
public interface HelloService {
    void sayHello();
}
```

### 2. 服务实现类
- **HelloServiceImpl1**: 输出 "hello HelloServiceImpl1"
- **HelloServiceImpl2**: 输出 "hello HelloServiceImpl2"

### 3. 服务配置文件
`META-INF/services/spi.service.HelloService` 文件中声明了所有实现类的全限定名：
```
spi.service.HelloServiceImpl1
spi.service.HelloServiceImpl2
```

### 4. 服务加载器
使用 `ServiceLoader.load(HelloService.class)` 动态发现和加载所有服务实现。

## 运行方式

### 方式一：通过GetImpl运行
```bash
# 编译项目
mvn compile

# 运行SPI演示
mvn exec:java -Dexec.mainClass="spi.GetImpl"
```

### 方式二：打包后运行
```bash
# 打包项目
mvn package

# 运行jar包
java -cp target/spi-1.0-SNAPSHOT.jar spi.GetImpl
```

## 运行结果

程序执行后将输出：
```
hello HelloServiceImpl1
hello HelloServiceImpl2
```

## SPI机制工作原理

1. **服务发现**: `ServiceLoader` 扫描classpath下所有 `META-INF/services/` 目录
2. **配置读取**: 根据接口全限定名找到对应的配置文件
3. **类加载**: 读取配置文件中的实现类全限定名并动态加载
4. **实例化**: 通过反射创建服务实现类的实例
5. **迭代使用**: 通过迭代器遍历所有可用的服务实现

## 项目特点

- ✅ **零依赖**: 纯Java标准库实现，无需第三方依赖
- ✅ **插件化架构**: 支持运行时动态添加新的服务实现
- ✅ **解耦设计**: 接口与实现分离，符合开闭原则
- ✅ **自动发现**: 无需手动注册，自动发现classpath中的服务实现
- ✅ **易于扩展**: 只需添加新的实现类和配置即可扩展功能

## 应用场景

- 数据库驱动加载
- 日志框架适配
- 插件系统开发
- 微服务治理
- 框架扩展点设计

## 开发规范

- Java版本: 8
- 编码格式: UTF-8
- 代码风格: 遵循Java标准命名规范
- 注释语言: 中文注释

## 作者信息

- **开发者**: ljx
- **创建时间**: 2025/5/12
- **版本**: 1.0.0

## 许可证

本项目仅供学习和演示使用。