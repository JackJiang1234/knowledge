### 安装

https://maven.apache.org/install.html

#### 最佳实践

设置M2_HOME环境变量，便于升级Maven

mvn help:system 打印java系统属性和环境变量

#### Maven目录

bin :mvn运行脚本

boot :mvn类加载器框架

conf :  mvn配置目录， settings.xml 全局定制mvn行为, 一般更篇向复制至~/.m2目录下

lib: mvn运行类库

~/.m2/repository 表示本地仓库目录

mvn配置 https://maven.apache.org/configure.html

### MVN使用

#### 最佳实践

指定文件编码和java编译版本

```xml
<!--http://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html -->

<properties>
    
     <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
     <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    
      <!-- 编译时的编码 -->  
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
    
</properties>


<plugins>
     <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
 </plugins>
```

使用Archetype生成项目

```shell
mvn archetype:generate
```

http://maven.apache.org/guides/getting-started/index.html

### 坐标和依赖

#### 坐标定义 

groupId: 定义当前Maven项目隶属的项目组

artifactId：定义实际项目中的一个maven项目模块, 推荐使用实际项目名做为前缀

version:  定义版本

packaging:  jar/war/pom

classifier: 定义构建输出的一些附属构件

项目构件输出文件名一般规则为: artifacId-version.pcakaging

#### 依赖定义

groupId, artifactId, Versin 依赖的基本坐标

type 依赖的类型

scope 依赖的范围, 依赖范围就是控制与三种classpath的关系(编译，测试， 运行) , 选项有compile(编译，测试，运行),test(测试),provided(编译和测试), runtime(测试和运行),system(编译和测试， 通过systemPath显示指定依赖) , import

exclusions 排除传递性依赖

#### 传递性依赖

#### 依赖调解

路径最近者优化

 第一声明者优先

#### 可选依赖

mvn dependency:list

mvn dependency:tree

### 仓库

#### 本地仓库



#### 远程仓库

#### 其它

超级POM位置 maven-model-builder-3.6.2.jar\org\apache\maven\model\

### 生命周期

https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html

https://maven.apache.org/ref/3.8.1/maven-core/lifecycles.html

https://maven.apache.org/ref/3.8.1/maven-core/default-bindings.html

### 聚合与继承

https://maven.apache.org/guides/mini/guide-multiple-modules.html

### 测试

http://maven.apache.org/surefire/maven-surefire-plugin/test-mojo.html

### 版本



### Profile

### 参考

[Setting.xml参考](https://maven.apache.org/settings.html)

[POM 参考](https://maven.apache.org/pom.html)

[POM 元素参考](https://maven.apache.org/ref/3.8.1/maven-model/maven.html)

[Maven帮助索引](https://maven.apache.org/guides/index.html)