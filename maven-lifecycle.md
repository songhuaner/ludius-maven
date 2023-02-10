#### 基础概念  lifecycle phase goal

#### 生命周期  lifecycle

Maven 抽象出了一个适合于所有项目的构建生命周期，并将它们统一规范。
具体步骤包括<B>清理、初始化、编译、测试、打包、集成测试、验证、部署和生成站点</B>。
这些步骤几乎适合所有的项目，所有项目的管理构建过程都可以对应到这个生命周期上来。
 Maven 只是规定了生命周期的各个阶段和步骤，具体事情，由集成到 Maven 中的插件完成。

Maven 拥有三套独立的生命周期，它们分别是 clean、default 和 site。

#### clean 生命周期
clean 生命周期的目的是清理项目，它包括以下三个阶段。
| 阶段 | 说明 | 
| :-----:| :----: | 
| pre-clean | 执行清理前需要完成的工作。 |
| clean | 清理上一次构建过程中生成的文件，比如编译后的 class 文件等。 | 
| post-clean | 执行清理后需要完成的工作| 

 #### default 生命周期
default 生命周期定义了构建项目时所需要的执行步骤，它是所有生命周期中最核心部分，包含的阶段如下表所述。

| 阶段 | 说明 | 
| :-----:| :----| 
| validate 	| 验证项目结构是否正常，必要的配置文件是否存在| 
| initialize 	| 做构建前的初始化操作，比如初始化参数、创建必要的目录等| 
| generate-sources 	| 产生在编译过程中需要的源代码| 
| process-sources 	| 处理源代码，比如过滤值| 
| generate-resources 	| 产生主代码中的资源在 classpath 中的包| 
| process-resources 	| 将资源文件复制到 classpath 的对应包中| 
| compile 	| 编译项目中的源代码| 
| process-classes 	| 产生编译过程中生成的文件| 
| generate-test-sources 	| 产生编译过程中测试相关的代码| 
| process-test-sources 	| 处理测试代码| 
| generate-test-resources 	| 产生测试中资源在 classpath 中的包| 
| process-test-resources 	| 将测试资源复制到 classpath 中| 
| test-compile 	| 编译测试代码| 
| process-test-classes 	| 产生编译测试代码过程的文件| 
| test 	| 运行测试案例| 
| prepare-package 	| 处理打包前需要初始化的准备工作| 
| package 	| 将编译后的 class 和资源打包成压缩文件，比如 rar| 
| pre-integration-test 	| 做好集成测试前的准备工作，比如集成环境的参数设置| 
| integration-test 	| 集成测试| 
| post-integration-test 	| 完成集成测试后的收尾工作，比如清理集成环境的值| 
| verify 	| 检测测试后的包是否完好| 
| install 	| 将打包的组件以构件的形式，安装到本地依赖仓库中，以便共享给本地的其他项目| 
| deploy 	| 运行集成和发布环境，将测试后的最终包以构件的方式发布到远程仓库中，方便所有程序员共享| 


#### site 生命周期
site 生命周期的目的是建立和发布项目站点。
Maven 可以基于 pom 所描述的信息自动生成项目的站点，同时还可以根据需要生成相关的报告文档集成在站点中，方便团队交流和发布项目信息。
site 生命周期包括如下阶段。

| 阶段 | 说明 | 
| :-----:| :----| 
|pre-site|执行生成站点前的准备工作。||
|site|生成站点文档。|
|post-site|执行生成站点后需要收尾的工作。|
|site-deploy|将生成的站点发布到服务器上。|


#### 阶段 phase
生命周期由阶段构成
生命周期所包含的阶段是有前后顺序的，并且后面的阶段依赖于前面的阶段

#### 目标 goal
目标是插件对阶段的实现。
phase其实就是goal的容器。实际被执行的都是goal。phase被执行时，实际执行的都是被绑定到该phase的goal。


##### 插件查询
http://maven.apache.org/plugins/index.html
http://mojo.codehaus.org/plugins.html


##### 默认引入的插件
```
 <build>
    <pluginManagement>
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>

        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>

```

##### 默认的绑定
```
  <component>
      <role>org.apache.maven.lifecycle.Lifecycle</role>
      <implementation>org.apache.maven.lifecycle.Lifecycle</implementation>
      <role-hint>default</role-hint>
      <configuration>
        <id>default</id>
        
        <phases>
          <phase>validate</phase>
          <phase>initialize</phase>
          <phase>generate-sources</phase>
          <phase>process-sources</phase>
          <phase>generate-resources</phase>
          <phase>process-resources</phase>
          <phase>compile</phase>
          <phase>process-classes</phase>
          <phase>generate-test-sources</phase>
          <phase>process-test-sources</phase>
          <phase>generate-test-resources</phase>
          <phase>process-test-resources</phase>
          <phase>test-compile</phase>
          <phase>process-test-classes</phase>
          <phase>test</phase>
          <phase>prepare-package</phase>
          <phase>package</phase>
          <phase>pre-integration-test</phase>
          <phase>integration-test</phase>
          <phase>post-integration-test</phase>
          <phase>verify</phase>
          <phase>install</phase>
          <phase>deploy</phase>
        </phases>
        
    </configuration>


    <component>
      <role>org.apache.maven.lifecycle.mapping.LifecycleMapping</role>
      <role-hint>jar</role-hint>
      <implementation>org.apache.maven.lifecycle.mapping.DefaultLifecycleMapping</implementation>
      <configuration>
        <lifecycles>
          <lifecycle>
            <id>default</id>
            
            <phases>
              <process-resources>
                org.apache.maven.plugins:maven-resources-plugin:2.6:resources
              </process-resources>
              <compile>
                org.apache.maven.plugins:maven-compiler-plugin:3.1:compile
              </compile>
              <process-test-resources>
                org.apache.maven.plugins:maven-resources-plugin:2.6:testResources
              </process-test-resources>
              <test-compile>
                org.apache.maven.plugins:maven-compiler-plugin:3.1:testCompile
              </test-compile>
              <test>
                org.apache.maven.plugins:maven-surefire-plugin:2.12.4:test
              </test>
              <package>
                org.apache.maven.plugins:maven-jar-plugin:2.4:jar
              </package>
              <install>
                org.apache.maven.plugins:maven-install-plugin:2.4:install
              </install>
              <deploy>
                org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy
              </deploy>
            </phases>
            
          </lifecycle>
        </lifecycles>
      </configuration>
    </component>

    <component>
      <role>org.apache.maven.lifecycle.Lifecycle</role>
      <implementation>org.apache.maven.lifecycle.Lifecycle</implementation>
      <role-hint>clean</role-hint>
      <configuration>
        <id>clean</id>
        
        <phases>
          <phase>pre-clean</phase>
          <phase>clean</phase>
          <phase>post-clean</phase>
        </phases>
        <default-phases>
          <clean>
            org.apache.maven.plugins:maven-clean-plugin:2.5:clean
          </clean>
        </default-phases>
        
      </configuration>
    </component>


    <component>
      <role>org.apache.maven.lifecycle.Lifecycle</role>
      <implementation>org.apache.maven.lifecycle.Lifecycle</implementation>
      <role-hint>site</role-hint>
      <configuration>
        <id>site</id>
        
        <phases>
          <phase>pre-site</phase>
          <phase>site</phase>
          <phase>post-site</phase>
          <phase>site-deploy</phase>
        </phases>
        <default-phases>
          <site>
            org.apache.maven.plugins:maven-site-plugin:3.3:site
          </site>
          <site-deploy>
            org.apache.maven.plugins:maven-site-plugin:3.3:deploy
          </site-deploy>
        </default-phases>
        
      </configuration>
    </component>

```