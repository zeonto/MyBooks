网易云音乐
=========

网易云官网已经出了 Linux 版本的客户端，不过只有 deepin 和 ubuntu 发行版的安装包。

[官网下载地址](https://music.163.com/#/download)

针对 RPM 系列没有官网包，但是有人移植了 RPM 系的安装包

## 方式一 repo 

安装步骤：
```
wget https://dl.senorsen.com/pub/package/linux/add_repo.sh -qO - | sudo sh
sudo dnf install http://dl-http.senorsen.com/pub/package/linux/rpm/senorsen-repo-0.0.1-1.noarch.rpm
sudo dnf install netease-cloud-music
```

卸载步骤：
```
sudo yum remove netease-cloud-music
rm /usr/bin/netease-cloud-music
rm -rf /usr/lib/netease-cloud-music
rm /usr/share/applications/netease-cloud-music.desktop
```

详情参考深度的文章 [深度合作应用 › 网易云音乐](https://www.deepin.org/cooperative/netease-cloud-music/)

使用存在的缺点：

- 右键菜单显示乱码

## 方式二 copr（推荐）

项目地址：[vitzy/netease-cloud-music](https://copr.fedorainfracloud.org/coprs/vitzy/netease-cloud-music/)

安装步骤：
```
# 启用源
sudo dnf copr enable vitzy/netease-cloud-music
# 注意看仓库是否为 vitzy-netease-cloud-music
sudo yum install netease-cloud-music
```

卸载步骤：
```
sudo yum remove netease-cloud-music
rm /usr/bin/netease-cloud-music
rm -rf /usr/lib/netease-cloud-music
rm /usr/share/applications/netease-cloud-music.desktop
```


