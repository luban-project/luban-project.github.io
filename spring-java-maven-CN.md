---
permalink: /spring-java-maven-CN.html
---


# Spring-Java-Maven模板项目文档

## 基本生成和运行
```  
# 第一次运行的时候，需要先安装一下项目模板
# windows用户需要先创建一下模板目录，防止权限问题
# mkdir %USERPROFILE%\.bullet_templates
luban install --name=bullet-spring-java-maven 

# 生成项目
luban fast-create --name=bullet-spring-java-maven --project=com.foo.example --app=example
cd example

# mac/linux用户需要给脚本添加可执行权限
chmod 755 gen/gen.sh
chmod 755 run.sh

# mac/linux用户生成代码并启动
./gen/gen.sh 
./run.sh

# windows用户生成代码并启动
gen\gen.bat
run.bat
```

## 基本项目结构
```text
root --|
    api --|  #对外提供的二方包，只能包含接口和POJO类，不能包含实现
             #后续dubbo接口/grpc接口也需要定义在这里
    app --|  #主体应用程序
        biz --| #业务逻辑层，应该只包含逻辑结构的组织
        clg --| #核心逻辑层，应该只包含核心的领域模型和逻辑，要求纯函数
        dal --| #数据操作层，mysql/redis/hbase/file等
        ext --| #外部防腐层，外部调用，mq接入等
        fun --| #通用方法层，纯函数
    gen --|  #自动代码生成的插件和配置
```

![micro-structure](https://luban-project.github.io/micro-structure.png)

## 如何根据表的变化生成新的数据层代码
### 建立数据库建表或者插数据语句
数据库sql建表语句需要创建在app/src/main/resouces/db/migration，
例如V1__create_entity_basic_info.sql如下：
```  
CREATE TABLE IF NOT EXISTS `entity_basic_info` (
  `id` BIGINT NOT NULL COMMENT '主键',
  `name` VARCHAR(128) NOT NULL COMMENT '用户昵称',
  `nickname` VARCHAR(64) NOT NULL COMMENT '昵称',
  `helloname` VARCHAR(64) NOT NULL COMMENT '测试',
  `worldname` VARCHAR(64) NOT NULL COMMENT '测试',
  PRIMARY KEY (`id`)
);
```
这里需要注意文件的命名方式,需要以V{x}__开头，注意这里是双下划线，例如：
```   
V1__create_xxx.sql
V2__create_xxx.sql
V3__insert_xxx.sql
```
### 关注一下自动生成代码的gen/pom.xml中的配置
``` 
<generator>
    <database>
        <name>org.jooq.meta.h2.H2Database</name>
        <outputSchemaToDefault>true</outputSchemaToDefault> <!-- 不生成库名，防止h2问题-->
        <inputSchema>PUBLIC</inputSchema> <!-- 需要生成代码的数据库,h2使用PUBLIC,mysql实际库名 -->
        <includes>.*</includes>                             <!-- 需要生成代码的表 -->
        <excludes>flyway_schema_history</excludes>          <!-- 需要排除的表 -->
    </database>
                        
    <target>
        <packageName>com.example.example.dal.gen</packageName> <!-- 生成的包名 -->
        <directory>${project.parent.basedir}/app/src/main/java/</directory>          <!-- 生成的代码位置 -->
    </target>

    <generate>
        <daos>true</daos>                           
        <springAnnotations>true</springAnnotations>
    </generate>
</generator>
```

### 执行生成脚本
```
# for mac/linux user
chmod 755 gen/gen.sh
./gen/gen.sh

# for windows user
gen\gen.bat
```
自动生成的数据库代码放在app/src/main/java/{group}/{project}/dal/gen下面   
每次运行数据库层代码生成，dal/gen下面的代码都会被覆盖   
dal/gen下面生成的代码包含了基本的增删改查  
复杂的sql需要自己封装，封装的dal层代码直接放到dal/下，不要放到dal/gen下面  
建议自定义的dal类命名为xxxRepository，其中xxx是操作的表名  

### 运行项目
项目运行可以通过简单的脚本命令
``` 
# for mac/linux user
chmod 755 run.sh
./run.sh

# for windows user
run.bat
```
当然也可以直接输入命令执行
```
mvn clean package # 需要显式执行这条mvn命令让编译期AOP织入生效
java -jar app/target/app-1.0.jar # 通过jar形式直接启动项目
```

### 项目运行到实际数据库上
默认情况下，项目直接跑在H2数据库模式上，可以通过修改配置变更数据源  
修改app/src/main/resources/application.yaml
```
spring:
  profiles:
    active: h2 # 修改到mysql
```  

修改app/src/main/resources/application-mysql.yaml    
```  
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test # 改到实际的库
    username: root                        # 改到实际账户
    password: 123456                      # 改到实际密码
```
