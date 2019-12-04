---
permalink: /index-CN.html
---

# 鲁班
鲁班是一套高效的项目/项目模板生成工具

# 安装
## mac环境（brew一键安装💖）
```
brew install luban-project/luban/luban
luban --version
```

## windows环境安装
windows用户可以直接下载已经打包好的可执行程序，  
如果要方便后续使用，最好设置一下环境变量，保证luban.exe的路径在PATH环境变量中。
```  
https://github.com/luban-project/luban-win/raw/master/latest/luban.exe
```

## linux/unix环境
```
curl https://sh.rustup.rs -sSf | sh # 1. install rust: 
cargo install cargo-luban           # 2. install luban: 
cargo luban --version               # 3. check version: 
```

## 任意支持rust语言的环境
1. 安装语言: following [rust-lang](https://www.rust-lang.org/tools/install)
2. 安装鲁班: cargo install cargo-luban
3. 检查版本: cargo luban --version
* 在windows上，请先安装visual studio，因为需要msvc的编译器

# 使用示例
```
# 第一次运行的时候，需要先安装一下项目模板
luban install --name=bullet-spring-java-maven 

# 生成项目
luban fast-create --name=bullet-spring-java-maven --project=com.foo.example
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
```  
chmod 755 gen/gen.sh
./gen/gen.sh
```

## 项目运行
```
chmod 755 run.sh
./run.sh
```


# 支持的模板项目
## Java Spring Maven
```text
luban install --name=bullet-spring-java-maven
luban create  --name=bullet-spring-java-maven
luban build   --name=bullet-spring-java-maven --output=out
```

# 常见问题
## 版本更新和兼容问题
处理方式：卸载旧版本，重新安装新版本
mac
```  
brew uninstall luban                 # 卸载鲁班
brew untap thegenius/luban/luban     # 最早的用户可能安装的老版本
brew untap luban-project/luban/luban # 现在的用户安装的版本
rm -rf ~/.bullet_templates           # 删除模板安装目录

brew install luban-project/luban/luban # 安装新版鲁班
luban --version                        # 检查版本
```

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