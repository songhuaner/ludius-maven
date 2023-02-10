### 继承 与 聚合
---

![Alt text](./images/BBBB.jpg)


#### 继承
子POM通过继承父POM，获取公共配置，减少子POM重复配置。


##### 声明父项目
```
<parent>
  <!--被继承的父项目的构件标识符 -->
  <artifactId>com.companyname.project-group</artifactId>
  <!--被继承的父项目的全球唯一标识符 -->
  <groupId>base-project</groupId>
  <!--被继承的父项目的版本 -->
  <version>1.0.1-RELEASE</version>

  <!-- 父项目的pom.xml文件的相对路径,默认值是../pom.xml。 -->
  <!-- 寻找父项目的pom：构建当前项目的地方--)relativePath指定的位置--)本地仓库--)远程仓库 -->
  <relativePath>../pom.xml</relativePath>
</parent>

```

##### 可被继承的元素

| 元素 | 说明 | 
| :-----:| :----: | 
|groupId|项目组ID,项目坐标的核心元素|
|version|项目版本, 项目坐标的核心元素|
|description|项目的描述信息|
|organization|项目的组织信息|
|inceptionYear|项目的创始年份|
|url|项目的URL地址|
|developers|项目开发者信息|
|contributors|项目的贡献者信息|
|distributionManagement|项目的部署配置|
|issueManagement|项目的缺陷跟踪系统信息|
|ciManagement|项目的持续集成系统信息|
|scm|项目的版本控制系统信息|
|mailingLists|项目的邮件列表信息|
|properties|自定义的maven属性|
|dependencies|项目的依赖配置|
|dependencyManagement|项目的依赖管理配置|
|repositories|项目的仓库配置|
|build|包括项目的源码目录配置、输出目录配置、插件配置、插件管理配置等|
|reporting|包括项目的报告输出目录配置、报告插件配置等|


#### 聚合

将多个maven项目合并为一个大的maven项目
```
<modules>
    <module>my-project</module>
    <module>another-project</module>
</modules>
```