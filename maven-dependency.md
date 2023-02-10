### 依赖
---
```
 <dependencies>
        <dependency>
            <groupId>junit</groupId>     
            <artifactId>junit</artifactId>     
            <version>3.8.1</version>
            <type>...</type>
            <scope>...</scope>
            <optional>...</optional>
            <exclusions>     
                <exclusion>     
                  <groupId>...</groupId>     
                  <artifactId>...</artifactId>     
                </exclusion>
          </exclusions>     
        </dependency>        
 </dependencies>     
```

| 元素 | 说明 | 
| :-----:| :----: | 
|dependencies|一个 pom.xml 文件中只能存在一个这样的标签。用来管理依赖的总标签。|
|dependency|包含在dependencies标签中，可以有无数个，每一个表示一个依赖|
|groupId,artifactId和version|依赖的基本坐标，对于任何一个依赖来说，基本坐标是最重要的，Maven根据坐标才能找到需要的依赖。|
|type|依赖的类型，对应于项目坐标定义的packaging。大部分情况下，该元素不必声明，其默认值是jar。|
|scope|依赖的范围，默认值是 compile。|
|optional|标记依赖是否可选|
|exclusions|用来排除传递性依赖|


#### 依赖的范围  scope
maven 项目不同的阶段引入到classpath中的依赖是不同的，就是用来控制这三种 classpath 的关系（编译 classpath、测试 classpath 和运行 classpath）
| 元素 | 说明 | 
| :-----:| :----: | 
|compile|对于编译、测试、运行三种classpath都有效|
|test|测试依赖范围  只对于测试classpath有效，在编译主代码或者运行项目的使用时将无法使用此类依赖。典型的例子就是JUnit，它只有在编译测试代码及运行测试的时候才需要|
|provided|对于编译和测试classpath有效，但在运行时无效,在编译测试时需要该jar包，但是运行时已存在，不需要重复引入，例子是servlet-api，编译和测试项目的时候需要该依赖，但在运行项目的时候，由于容器已经提供|
|runtime|运行时依赖范围，对于测试和运行classpath有效，但在编译主代码时无效，典型的例子是JDBC驱动实现，项目主代码的编译只需要JDK提供的JDBC接口，只有在执行测试或者运行项目的时候才需要实现上述接口的具体JDBC驱动|
|system|和provided依赖范围完全一致，使用system范围依赖时必须通过systemPath元素显式地指定依赖文件的路径，依赖不是通过Maven仓库解析的，而且往往与本机系统绑定|
|import|不会对三种classpath产生实际的影响，它的作用是将其他模块定义好的 dependencyManagement 导入当前 Maven 项目 pom 的 dependencyManagement 中，scope为import只能在dependencyManagement中使用，且type为pom类型|


#### 依赖传递 
举例说明
A>>B>>C  
项目A依赖项目B，项目B依赖C，那项目A也间接的引入了C

##### 依赖传递与依赖范围的关系
![Alt text](./images/AAA.png)

1 、当第二直接依赖的范围是compile的时候，传递性依赖与第一直接依赖的范围一致； 
2 、当第二直接依赖的范围是test的时候，依赖不会得以传递；
3 、当第二直接依赖是provided的时候，值传递第一直接依赖范围也为provided的依赖，且传递性依赖范围同样为provided; 
4 、当第二依赖的范围是runtime的时候，传递性范围与第一直接依赖的范围一致，但compile例外，此时传递性依赖的范围为runtime。

#### 可选依赖  optional 
A>>B>>C
默认false，如果为true，如下例子，C会对当前项目B产生影响，当其他项目依赖于B的时候，依赖不会被传递。
```
<dependency>
    <groupId>C</groupId>     
    <artifactId>C</artifactId>     
    <version>3.8.1</version>
    <optional>true</optional>   
</dependency>  
```


#### 依赖排除 exclusions
A>>B>>C

C是A的传递依赖，但A不希望依赖C，通过exclusions，可将C排除

```
 <dependencies>
        <dependency>
            <groupId>junit</groupId>     
            <artifactId>junit</artifactId>     
            <version>3.8.1</version>
            <exclusions>     
                <exclusion>     
                  <groupId>A</groupId>     
                  <artifactId>B</artifactId>     
                </exclusion>
          </exclusions>     
        </dependency>        
 </dependencies>     
```


#### 依赖冲突
Maven依赖调解原则
1 、路径最近者优先。
2 、第一声明者优先。

举例：
A->B->C->X(1.0)
A->D->X(2.0)

X是A的传递依赖，X(1.0)的路径长度为3，而X(2.0)的路径长度为2，X(2.0)会被解析使用。在依赖路径长度相同的情况下，在POM中依赖声明的顺序决定了谁会被解析使用，顺序最靠前的那个依赖优胜。

当发生依赖冲突时，可依据原则，结合依赖排除与可选依赖，进行处理

#### 依赖版本控制  dependencyManagement

dependencyManagement 申明依赖版本，不实际引入依赖。
dependency中配置的version，则使用当前version;
dependency未配置version，且在dependencyManagement中存在，则使用dependencyManagement中的version

```
<dependencyManagement>  
      <dependencies>  
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-core</artifactId>  
                <version>3.2.7</version>  
            </dependency>  
    </dependencies>  
</dependencyManagement>  

<dependencies>  
  <dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-core</artifactId>  
  </dependency>  
</dependencies>  
```