maven安装jar包使用以下命令：  
```  
mvn install:install-file -Dfile=jar_location -DgroupId=groupId -DartifactId=artifactId -Dversion=version -Dpackaging=jar
```
项目中依赖的写法如下： 

```
    <dependency>     
       <groupId>org.springframework</groupId>     
       <artifactId>spring-context-support</artifactId>  
       <version>3.1.0.RELEASE</version>  
    </dependency>  
```
依赖中的groupId、artifactId和version分别对应于上述的命令中对应项。命令执行需要进入包含上述jar包的文件夹，同时相同位置还需要一个.pom文件，名称与jar包相同。.pom文件格式如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <modelVersion>4.0.0</modelVersion>
  <groupId>json</groupId>
  <artifactId>json</artifactId>
  <version>1.0.0</version>
  <name>json</name>
  <description>json</description>
  <url></url>
  <!--<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
   </dependencies> --!>
 </project>
  
```
安装完成后即可在项目中引用。
