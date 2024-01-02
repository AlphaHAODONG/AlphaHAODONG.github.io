# Maven


Apache Maven 是一个项目管理和构建工具，它基于项目对象模型(POM)的概念

通过一小段描述信息来管理项目的构建、报告和文档 

官网：http://maven.apache.org/ 

### 1.1Maven安装配置

1. 解压 apache-maven-3.6.1.rar 既安装完成
2. 配置环境变量 MAVEN_HOME 为安装路径的bin目录
3. 配置本地仓库：修改 conf/settings.xml 中的  为一个指定目录
4. 配置阿里云私服：修改 conf/settings.xml 中的 标签，为其添加如下子标签：  

```xml
<mirror>
<id>alimaven</id>
<name>aliyun maven</name>
<url>http://maven.aliyun.com/nexus/content/groups/public/</url><mirrorOf>central</mirrorOf>
</mirror>
```

### 1.2Maven的基本使用

#### 1.2.1Maven的常用命令

- compile :编译

- clean:清理

- test:测试

- package:打包

- install:安装

#### 1.2.2Maven生命周期

- Maven构建项目生命周期描述的是一次构建过程经历经历了多少个事件

- Maven对项目构建的生命周期划分为3套

1. clean:清理工作
2. default:核心工作，例如编译，测试，打包，安装等
3. site:产生报告，发布站点等

同一生命周期内，执行后边的命令，前边的所有命令会自动执行


