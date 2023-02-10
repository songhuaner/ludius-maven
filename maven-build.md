
### 构建 build
---

build标签描述了如何编译及打包项目，具体的编译和打包工作是通过其中的plugin配置来实现的。

#### 两种build
| 元素 | 说明 | 
| :-----:| :----: | 
|project build|project 的直接子元素  针对整个项目的所有情况都有效|
|profile build|profile 的直接子元素 针对不同的profile配置 对于多个环境的不同配置  比如测试 开发  生产|


```
project build:
<build>  
  <defaultGoal>install</defaultGoal>  
  <directory>${basedir}/target</directory>  
  <finalName>${artifactId}-${version}</finalName> 
  <filters>
   <filter>filters/filter1.properties</filter>
  </filters> 
  ...
</build> 


profile build:
<profiles>  
  <profile> 
    <build>...</build>  
  </profile>  
</profiles>  
```

#### 共用的基本元素
| 元素 | 说明 | 
| :-----:| :----: | 
| defaultGoal|  执行build任务时，如果没有指定目标，将使用的默认值 |
| directory |  build目标文件的存放目录，默认在${basedir}/target目录 | 
| finalName | build目标文件的名称，默认情况为\${artifactId}-\${version}|

#### resources 元素  资源 
构建过程中将资源文件从源路径复制到指定的目标路径，resources则给出各个资源在maven项目中的具体路径

```
<build>  
        ...  
       <resources>  
          <resource>  
             <targetPath>META-INF/plexus</targetPath>  
             <filtering>true</filtering>  
            <directory>${basedir}/src/main/plexus</directory>  
            <includes>  
                <include>configuration.xml</include>  
            </includes>  
            <excludes>  
                <exclude>**/*.properties</exclude>  
            </excludes>  
         </resource>  
    </resources>  
    <testResources>  
        ...  
    </testResources>  
    ...  
</build> 
```

| 元素 | 说明 | 
| :-----:| :----: | 
|resources|一个resources元素的列表。|
|targetPath|目标路径|
|directory|资源文件源路径，默认位于${basedir}/src/main/resources/目录下|
|includes|源目录中需要包含的文件|
|excludes|源目录中需要排除的文件|
|testResources|test过程中涉及的资源文件，默认位于${basedir}/src/test/resources/目录下|
|filtering|表示资源文件中的占位符是否需要被替换，true为需要替换|


#### filters 元素
默认目录:
${basedir}/src/main/filters/

```
<build>
    ...
    <filters> <!-- 指定使用的 filter -->
      <filter>src/main/filters/AAA.properties</filter>
    </filters>
    ...
</build>

```
在资源文件中可以使用\${key}来表示某一配置内容的占位符，maven通过filters，指定属性文件(properties \<key,value>)地址，当某一资源配置filtering为true时，属性文件中的key对应的内容将会替换掉资源文件中key的占位符


##### maven 中properties 加载顺序
1、\<filters> 中的配置
2、pom.xml 中的 \<properties>
3、mvn -Dproperty=value 中定义的 property
<B>相同 key 的 property，以最后一个文件中的配置为最终配置。<B>



#### profile 元素