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

## linux/unix
```  
curl https://sh.rustup.rs -sSf | sh # 1. install rust: 
cargo install cargo-luban           # 2. install luban: 
cargo luban --version               # 3. check version: 
```

## windows
1. install rust: following [rust-lang](https://www.rust-lang.org/tools/install)
2. install bullet: cargo install cargo-bullet
3. check version: cargo bullet --version
* please install visual studio to get the msvc compiler

# usage example
```  
# for the first time, install project template
luban install --name=bullet-spring-java-maven 

# generate project
luban fast-create --name=bullet-spring-java-maven --project=com.foo.example
cd example

# make shell executable
chmod 755 gen/gen.sh
chmod 755 run.sh

# generate code and run
./gen/gen.sh 
./run.sh
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

## 数据库代码生成
数据库相关的sql脚本在app/src/main/resouces/db/migration目录下  
代码生成命令如下：
```shell script
chmod 755 gen/gen.sh
./gen/gen.sh
```

## 项目运行
```shell script
chmod 755 run.sh
./run.sh
```


# Supported Templates
## Java Spring Maven
```text
cargo bullet install --name=bullet-spring-java-maven
cargo bullet create  --name=bullet-spring-java-maven
cargo bullet build   --name=bullet-spring-java-maven --output=out
```

# FAQ
## 模板安装目录已经存在，写入失败导致模板安装失败（常发生在安装过历史版本，新老版本不兼容时）
表现形式： 安装模板执行luban install的时候报~/.bullet_templates写入失败  
处理方式： 手工删除重建~/.bullet_templates目录

## 模板安装目录不存在，写入失败导致模板安装失败（常发生在windows用户是管理员的情况）
表现形式： 安装模板执行luban install的时候报~/.bullet_templates无法创建  
处理方式： 使用cmd命令行mkdir ~/.bullet_templates手工创建一下目录  

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
