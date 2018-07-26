
##  构建你的镜像, 并发布到registry

> mvn  compile  jib:build

## Jib如何让开发变得更美好

Jib利用了Docker镜像的分层机制，将其与构建系统集成，并通过以下方式优化 Java容器镜像的构建
简单——Jib使用Java开发，并作为Maven的一部分运行。你不需要编写Dockerfile或运行Docker守护进程，甚至无需创建包含所有依赖的大JAR包。因为Jib与Java构建过程紧密集成，所以它可以访问到打包应用程序所需的所有信息。在后续的容器构建期间，它将自动选择Java构建过的任何变体。
快速——Jib利用镜像分层和注册表缓存来实现快速、增量的构建。它读取你的构建配置，将你的应用程序组织到不同的层（依赖项、资源、类）中，并只重新构建和推送发生变更的层。在项目进行快速迭代时，Jib只讲发生变更的层（而不是整个应用程序）推送到注册表来节省宝贵的构建时间。
可重现——Jib支持根据Maven的构建元数据进行声明式的容器镜像构建，因此，只要输入保持不变，就可以通过配置重复创建相同的镜像。


## 如何使用Jib来容器化你的应用程序

Jib可作为Maven的插件使用，并且只需要做出最少的配置。只需将插件添加到构建定义中并配置目标镜像即可。如果要将镜像推送到私有注册中心，要为Jib配置所需的秘钥。最简单的方法是使用docker-credential-gcr之类的凭证助手。Jib还提供了其他的一些规则，用于将镜像构建到 Docker守护进程。

###  在Maven中使用Jib：

```xml
<plugin>
    <groupId>com.google.cloud.tools</groupId>
    <artifactId>jib-maven-plugin</artifactId>
    <version>0.9.6</version>
    <configuration>
        <from>
            <!--base image-->
            <image>openjdk:alpine</image>
        </from>
        <to>
            <!--<image>registry.cn-hangzhou.aliyuncs.com/m65536/jibtest</image>-->
            <!--目标镜像registry地址，为了方便测试，你需要换成自己的地址，如果你的网络不好，可以选用国内加速器，比如阿里云的-->
            <image>registry.hub.docker.com/moxingwang/jibtest</image>
        </to>
    </configuration>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>build</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

这只是一个最简单的配置，比如registry认证配置，jvm配置等等，可以参考github jib详细说明[jib/jib-maven-plugin/](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#from-object)。

