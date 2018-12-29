在 Fedora 环境下安装 Node.js 
==========================

下载地址：

官网下载地址：[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

官网中文地址：[https://nodejs.org/zh-cn/download/](https://nodejs.org/zh-cn/download/)

## 包管理器安装方式

## 编译安装方式

## 二进制安装方式

1、下载页面的软件包选择 `Linux 二进制包 (x64)`。

2、解压压缩包后迁移到自己想放的目录
```
// 解压二进制包
tar -xvf node-v8.9.0-linux-x64.tar.xz
// 移动解压后的文件夹
mv node-v10.15.0-linux-x64 ~/Apps/nodejs
```

3、配置环境变量
```
vim /etc/profile
// 添加下面配置
export NODEJS=/home/zeonto/Apps/nodejs/bin
export PATH=$PATH:$NODEJS
```

4、使环境生效
```
source /etc/profile
```

5、检查环境变量
```
echo $PATH
```

6、查看版本
```
npm -v
node -v
```