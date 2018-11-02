
## maven 命令

#### 创建项目

        mvn archetype:create -DgroupId=com.zang.maven  -DartifactId=system-parent -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
> maven3.0.5以上版本舍弃了create，使用generate生成项目

#### 发布第三方Jar到本地库中：

        mvn install:install-file -DgroupId=com -DartifactId=test -Dversion=0.1.0 -Dpackaging=jar -Dfile=d:\test-0.1.0.jar

#### 查看依赖项树

        mvn dependency:tree

***

## <a id="lifecycle">[maven 生命周期](https://www.cnblogs.com/EasonJim/p/6816340.html)</a>    
生命周期（lifecycle）由各个阶段组成，每个阶段由maven的插件plugin来执行完成。生命周期（lifecycle）主要包括clean、resources、complie、install、package、testResources、testCompile等，其中带test开头的都是用业编译测试代码或运行单元测试用例的。 

### package、install、deploy 区别
* mvn package

依次执行了resources、compile、testResources、testCompile、test、jar(打包)等阶段。
package命令完成了项目编译、单元测试、打包功能；但没有把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库和远程maven私服仓库

* mvn install
依次执行了resources、compile、testResources、testCompile、test、jar(打包)、install等阶段。   
install命令完成了项目编译、单元测试、打包功能，同时把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库；但没有布署到远程maven私服仓库

* mvn deploy
依次执行了resources、compile、testResources、testCompile、test、jar(打包)、install、deploy等阶段。
deploy命令完成了项目编译、单元测试、打包功能，同时把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库和远程maven私服仓库


## maven 插件

####  maven-jar-plugin
maven 默认的打包插件。使用 Maven 构建一个 JAR 文件比较容易：只要定义项目包装为 “jar”，然后执行包装[生命周期](#lifecycle)阶段即可。但是定义一个可执行 JAR 文件却比较麻烦。

```java
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <configuration>
                <archive>
                    <manifest>
                        <addClasspath>true</addClasspath>
                        <classpathPrefix>lib/</classpathPrefix>
                        <mainClass>com.mypackage.MyClass</mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>
```


#### spring-boot-maven-plugin
maven springboot 打包插件：

```java
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${spring.boot.version}</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>    
```
> 打完包会多一个目录BOOT-INF

## maven 错误
* Remove all your failed downloads  

Linux                   

        find ~/.m2  -name "*.lastUpdated" -exec grep -q "Could not transfer" {} \; -print -exec rm {} \;

Windows                 

        cd %userprofile%\.m2\repository
        for /r %i in (*.lastUpdated) do del %i

* maven 项目关于Eclipse报:" cannot be read or is not a valid ZIP file"     

这中情况 说明依赖的jar包没有完全下好。到repository 中删去相应的jar目录 让maven重新去下载。
如果还报错，删除maven报错，eclipse中打开pom.xml 编辑状态 重新保存。看看最新的报错信息，进入对应的目录，删除即可