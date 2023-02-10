

#### 插件
 Maven 只是对项目的构建过程进行了统一的抽象定义和管理。至于每个阶段由谁来做，Maven 自己不去实现，而是让对应的插件去完成。这就是插件的作用。
```
<plugins>
    <!--参见build/pluginManagement/plugins/plugin元素 -->
    <plugin>
        <groupId />
        <artifactId />
        <version />
        <extensions />
        <executions>
            <execution>
                <id />
                <phase />
                <goals />
                <inherited />
                <configuration />
            </execution>
        </executions>
        <dependencies>
            <!--参见dependencies/dependency元素 -->
            <dependency>
                ......
            </dependency>
        </dependencies>
        <goals />
        <inherited />
        <configuration />
    </plugin>
</plugins>
```

| 元素 | 说明 | 
| :-----:| :----: | 
|plugins|构建过程中所用到的插件|
|plugin|插件配置|
|groupId||
|artifactId||
|version||
|extensions|是否加载当前插件的扩展，默认false|
|inherited|该插件的configuration中的配置是否可以被继承,默认true|
|configuration|该插件所需要的特殊配置,在父子项目之间可以进行覆盖或者合并|
|executions|当前插件的某个goal(目标)的执行方式|
|dependencies|插件所特有的依赖类库|
|goals||

| 元素 | 说明 | 
| :-----:| :----: | 
|executions|当前插件的某个goal(目标)的执行方式|
|execution||
|id|唯一标识|
|goals|要执行的goal列表|
|goal|要执行的goal|
|inherited|该execution是否可以被子项目继承|
|configuration|该execution的其他配置参数|
|phase|插件的goal在哪一个phase(阶段)执行|





##### 常用插件
##### maven-compiler-plugin
```
<plugin>
    <!-- 指定maven编译的jdk版本,如果不指定,maven3默认用jdk 1.5 maven2默认用jdk1.3 -->
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <!-- 一般而言，target与source是保持一致的，
        但是，有时候为了让程序能在其他版本的jdk中运行(对于低版本目标jdk，源代码中不能使用低版本jdk中不支持的语法)，
        会存在target不同于source的情况 -->                    
        <source>1.8</source> <!-- 源代码使用的JDK版本 -->
        <target>1.8</target> <!-- 需要生成的目标class文件的编译版本 -->
        <encoding>UTF-8</encoding><!-- 字符集编码 -->
        <skipTests>true</skipTests><!-- 跳过测试 -->
        <verbose>true</verbose>
        <showWarnings>true</showWarnings>
        <fork>true</fork><!-- 要使compilerVersion标签生效，还需要将fork设为true，用于明确表示编译版本配置的可用 -->
        <executable><!-- path-to-javac --></executable><!-- 使用指定的javac命令，例如：<executable>${JAVA_1_4_HOME}/bin/javac</executable> -->
        <compilerVersion>1.3</compilerVersion><!-- 指定插件将使用的编译器的版本 -->
        <meminitial>128m</meminitial><!-- 编译器使用的初始内存 -->
        <maxmem>512m</maxmem><!-- 编译器使用的最大内存 -->
        <compilerArgument>-verbose -bootclasspath ${java.home}\lib\rt.jar</compilerArgument><!-- 这个选项用来传递编译器自身不包含但是却支持的参数选项 -->
    </configuration>
</plugin>

```

##### maven-jar-plugin  插件

生成的 JAR 文件是不包含第三方依赖

| 配置 | 项目 | 子项目 |说明 | 
| :-----:| :----|  :----| :----| 
|archive|addMavenDescriptor||创建的存档是否包含以下两个Maven文件：(1)pom文件，位于 META-INF / maven / ${groupId} / ${artifactId}/pom.xml (2)pom.properties文件，位于META-INF / maven / ${groupId} / ${artifactId} /pom.propertie|
||compress||是否压缩|
||forced||是否强制重建存档|
||index||创建的存档是否包含 INDEX.LIST文件|
||manifest||清单|
|||addClasspath|是否创建Class-Path清单条目|
|||addDefaultImplementationEntries|manifest 将会包含以下内容：1.Implementation-Title:\${project.name} 2.Implementation-Version:\${project.version} 3.Implementation-Vendor-Id: \${project.groupId} 4.Implementation-Vendor: \${project.organization.name} 5.Implementation-URL:\${project.url}。|
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


```
<plugin>  
    <groupId>org.apache.maven.plugins</groupId>  
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.3.0</version>
    <configuration>
        <archive>                           <!-- 存档 -->
          <addMavenDescriptor/>                 <!-- 添加maven 描述 -->
          <compress/>                           <!-- 压缩 -->
          <forced/> 
          <index/>

          <manifest>                            <!-- 配置清单（MANIFEST）-->
            <addClasspath/>                         <!-- 添加到classpath 开关 -->
            <addDefaultImplementationEntries/> 
            <addDefaultSpecificationEntries/>
            <addExtensions/>
            <classpathLayoutType/>
            <classpathMavenRepositoryLayout/>
            <classpathPrefix/>                      <!-- classpath 前缀 -->
            <customClasspathLayout/>
            <mainClass/>                            <!-- 程序主函数入口 -->
            <packageName/>                          <!-- 打包名称 -->
            <useUniqueVersions/>                    <!-- 使用唯一版本 -->
          </manifest>

          <manifestEntries>                     <!-- 配置清单（MANIFEST）属性 -->                       
            <key>value</key>
          </manifestEntries>

          <manifestFile/>                       <!-- MANIFEST 文件位置 -->

          <manifestSections>
            <manifestSection>
              <name/>
              <manifestEntries>
                <key>value</key>
              </manifestEntries>
            <manifestSection/>
          </manifestSections>

          <pomPropertiesFile/>
        </archive>
         
        <excludes>                          <!-- 过滤掉不希望包含在jar中的文件  --> 
            <exclude/>
        </excludes>  
        
        <includes>                          <!-- 添加文件到jar中的文件  --> 
            <include/>
        </includes>
    </configuration>  
</plugin>



<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-jar-plugin</artifactId>
			<version>2.6</version>
			<configuration>
				<archive>
					<manifest>
						<addClasspath>true</addClasspath>
						<classpathPrefix>lib/</classpathPrefix>
						<mainClass>com.lala.shop.App</mainClass>
					</manifest>
				</archive>
			</configuration>
		</plugin>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-dependency-plugin</artifactId>
			<version>2.10</version>
			<executions>
				<execution>
					<id>copy</id>
					<phase>package</phase>
					<goals>
						<goal>copy-dependencies</goal>
					</goals>
					<configuration>
						<outputDirectory>
							${project.build.directory}/lib
						</outputDirectory>
					</configuration>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```

##### maven-shade-plugin  插件

##### maven-assembly-plugin 插件

##### spring-boot-maven-plugin  插件
```

<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <version>1.5.4.RELEASE</version>
</plugin>

<build>
<plugins>
    <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <version>2.0.1.RELEASE</version>
        <executions>
            <execution>
                <goals>
                    <goal>repackage</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
</plugins>
</build>

```

| 目标 | 说明 | 
| :-----:| :----| 
|spring-boot:repackage|默认goal。在mvn package之后，再次打包可执行的jar/war，同时保留mvn package生成的jar/war为.origin|
|spring-boot:run|运行Spring Boot应用|
|spring-boot:start|在mvn integration-test阶段，进行Spring Boot应用生命周期的管理|
|spring-boot:stop|在mvn integration-test阶段，进行Spring Boot应用生命周期的管理|
|spring-boot:build-info|生成Actuator使用的构建信息文件build-info.properties|





#### 插件管理



