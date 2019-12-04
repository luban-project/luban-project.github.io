# documents
[[English](https://luban-project.github.io "English Homepage")]
[[中文](https://luban-project.github.io/index-CN.html "中文首页")]

# luban
luban is a generic project generator

# install
## mac
```  
brew install luban-project/luban/luban
luban --version
```
## windows
you can download the pre-built binary for windows
```  
https://github.com/luban-project/luban-win/raw/master/latest/luban.exe
```

## linux/unix
```  
curl https://sh.rustup.rs -sSf | sh # 1. install rust: 
cargo install cargo-luban           # 2. install luban: 
cargo luban --version               # 3. check version: 
```

# usage example
```  
# for the first time, install project template
# for windows user, you need create template dir mannually
# mkdir %USERPROFILE%\.bullet_templates
luban install --name=bullet-spring-java-maven 

# generate project
luban fast-create --name=bullet-spring-java-maven --project=com.foo.example
cd example

# for mac/linux user, make shell executable
chmod 755 gen/gen.sh
chmod 755 run.sh

# for mac/linux user, generate code and run
./gen/gen.sh 
./run.sh

# for windows user, generate code and run
gen\gen.bat
run.bat
```

# bullet-spring-java-maven模板说明

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

## 项目详细文档  
[[Spring-Maven-文档](https://luban-project.github.io/spring-java-maven-CN.html "Maven项目文档")]  


## 数据库代码生成
数据库相关的sql脚本在app/src/main/resouces/db/migration目录下  
代码生成命令如下：
```
# for mac/linux user
chmod 755 gen/gen.sh
./gen/gen.sh

# for windows user
gen\gen.bat
```

## 项目运行
```
# for mac/linux user
chmod 755 run.sh
./run.sh

# for windows user
run.bat
```


# Supported Templates
## Java Spring Maven
```text
luban install --name=bullet-spring-java-maven
```

# FAQ
## 模板安装目录已经存在，写入失败导致模板安装失败（常发生在安装过历史版本，新老版本不兼容时）
表现形式： 安装模板执行luban install的时候报~/.bullet_templates写入失败  
处理方式： 手工删除重建~/.bullet_templates目录

## 模板安装目录不存在，写入失败导致模板安装失败（常发生在windows用户是管理员的情况）
表现形式： 安装模板执行luban install的时候报~/.bullet_templates无法创建  
处理方式： 手工创建一下目录   
mkdir ~/.bullet_templates             # mac/linux  
mkdir %USERPROFILE%\.bullet_templates # windows  

## mac系统/linux系统openssl没有安装的问题
表现形式： 报一堆openssl的错误  
处理方式：  
mac: brew install openssl  
centos: yum install openssl-devel  
ubuntu: sudo apt install libssl-dev & sudo apt instll pkg-config  

## linux系统没有安装基本构建工具的问题
表现形式： linker `cc` not found  
处理方式：  
ubuntu: sudo apt-get install build-essential
