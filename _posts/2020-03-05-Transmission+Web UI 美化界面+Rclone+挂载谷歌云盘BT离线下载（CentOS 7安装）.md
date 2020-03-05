---

layout: post

title: Transmission+Web UI 美化界面+Rclone+挂载谷歌云盘BT离线下载（CentOS 7安装）

categories: Transmission

description: 可以安装Transmission使台VPS变身成一台BT下载机

keywords: Transmission, Rclone, 谷歌云盘, BT, 离线下载

---

# 序言
VPS，一般除了科学上网、搭网站之类，我们还可以安装Transmission，使这台VPS变身成一台BT下载机，本文在Centos 7上安装Transmission。

# Transmission
## 安装
Transmission 包含在 EPEL 拓展仓库中，如果没有安装 EPEL 源，安装前需要输入以下命令安装 EPEL 源（需要 root 权限）：

```
yum -y install epel-release
yum -y update
```
EPEL 源安装成功后，即可安装 Transmission：
```
yum install transmission-daemon
```
## 配置
安装成功后，输入

```
systemctl start transmission-daemon.service
```
即可启动 Transmission。如果用浏览器打开 web 端（http://域名:9091 或 http:// IP 地址:9091），会提示“403: Forbidden”（页面打不开可能是防火墙没有放行相应端口），这是因为 Transmission 还没配置好。
因而，需要输入

```
systemctl stop transmission-daemon.service
```
停止 Transmission 服务，再进行配置。
注意，如果安装后没有启动过 Transmission，是不会生成配置文件。因而，需要先启动服务，再停止服务，生成 json 配置文件后再进行配置。

接下来，就可以打开配置文件：

```
vi /var/lib/transmission/.config/transmission-daemon/settings.json
```
配置文件的参数非常多，可按自己需求进行修改。如果对 vi 编辑器不了解，Google 或百度一下就有简单的使用介绍。以下条目的修改是本人自己在用的参数：
```
"rpc-authentication-required": true,
"rpc-enabled": true,
"rpc-password": "密码",
"rpc-username": "账户",
"rpc-whitelist-enabled": false,
"rpc-whitelist": "0.0.0.0",
```
更多配置参数，可去[Editing-Configuration-Files](https://github.com/transmission/transmission/wiki/Editing-Configuration-Files)探索。
配置好后，保存退出 json 文件，再次输入

```
systemctl start transmission-daemon.service
```
启动 Transmission 服务，即可用浏览器打开 web 端（http://域名:9091 或 http:// IP 地址:9091），上传种子进行下载。
注意，如果 Transmission 下载上传失败，大多是设置了防火墙导致的，这时还需要放行 9091 端口。CentOS 7 默认开启的一般是 firewalld，分别输入

```
firewall-cmd --permanent --zone=public --add-port=9091/tcp
```
和
```
firewall-cmd --reload
```
即可放行 9091 端口的 TCP 协议。
如果不需要启用防火墙，也可以分别输入

```
systemctl stop firewalld
```
和
```
systemctl disable firewalld
```
关闭防火墙。
可以设置开启自启。

```
systemctl enable transmission-daemon.service
```
默认下载路径一般不需更改。如果要修改，除了要在配置文件中修改“download-dir”参数，还需要修改新下载文件夹的权限和用户组：
```
chown -R transmission 新下载文件路径
chgrp -R transmission 新下载文件路径
```
## Web UI 美化界面
Transmission 自带的网页 UI 比较简陋，可以安装[transmission-web-control](https://github.com/ronggang/transmission-web-control)进行美化：

```
wget https://github.com/ronggang/transmission-web-control/raw/master/release/install-tr-control.sh --no-check-certificate
bash install-tr-control.sh
```
再次打开网页，即可使用 transmission-web-control 的 UI。
## 其他安装方式
**Ubuntu 安装**

Ubuntu 系统的安装配置可参考[https://luodaoyi.com/p/install-transmission-to-hang-pt-and-configure-domain-name-access-under-ubuntu.html](https://luodaoyi.com/p/install-transmission-to-hang-pt-and-configure-domain-name-access-under-ubuntu.html%22)

**Docker 安装**

Transmission 也有 Docker 镜像（[https://hub.docker.com/r/linuxserver/transmission/](https://hub.docker.com/r/linuxserver/transmission/)），也可使用 Docker 安装。

# Rclone
由vps上的transmission下载到本地后，自动上传到谷歌云。tr有缓存文件夹和默认目录，在完成后会自动复制到默认目录，将谷歌云挂载到默认目录即刻实现下载后上传到谷歌云。

## 安装
Rclone 挂载Google Drive，安装Rclone依次将代码输入SSH内，耐心等待一条代码执行完成在输入下一条代码

```
cd ~
yum -y install unzip
curl https://rclone.org/install.sh | sudo bash
```
## 配置
配置Rclone依次将代码输入SSH内，耐心等待一条代码执行完成在输入下一条代码

1. 输入  rclone config  回车
2. 提示n/s/q> 输入‘n’回车
3. 提示 name> 输入 ‘lance’ 回车
4. 提示 Storage> 找到Google Drive，前的序列号输入序列号回车。我这里是10，所以输入10
5. 提示 client_id> 回车跳过，提示client_secret> 也回车跳过
6. 提示 scope> 输入‘1’
7. 提示 root_folder_id> 回车跳过，提示 service_account_file> 也回车跳过
8. 提示 y/n> 输入‘n’回车
9. 提示 y/n> 输入‘n’回车
10. 复制 If your browser doesn’t open automatically go to the following link: 后的链接到浏览器打开
11. 登入要搭建的谷歌网盘账号，点击允许，复制代码到SSH内粘贴后回车
12. 提示 y/n> 输入‘y’回车
13. 提示 y/e/d> 输入‘y’回车
14. 提示 e/n/d/r/c/s/q> 输入‘q’回车

rclone config过程如下

```
[root@instance-5 ~]# rclone config
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n  #新建配置
name> lance  #随便填，后面要用到
Type of storage to configure.
Choose a number from below, or type in your own value
 1 / Amazon Drive
   \ "amazon cloud drive"
 2 / Amazon S3 (also Dreamhost, Ceph, Minio)
   \ "s3"
 3 / Backblaze B2
   \ "b2"
 4 / Box
   \ "box"
 5 / Cache a remote
   \ "cache"
 6 / Dropbox
   \ "dropbox"
 7 / Encrypt/Decrypt a remote
   \ "crypt"
 8 / FTP Connection
   \ "ftp"
 9 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
10 / Google Drive
   \ "drive"
11 / Hubic
   \ "hubic"
12 / Local Disk
   \ "local"
13 / Microsoft Azure Blob Storage
   \ "azureblob"
14 / Microsoft OneDrive
   \ "onedrive"
15 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
16 / Pcloud
   \ "pcloud"
17 / QingCloud Object Storage
   \ "qingstor"
18 / SSH/SFTP Connection
   \ "sftp"
19 / Webdav
   \ "webdav"
20 / Yandex Disk
   \ "yandex"
21 / http Connection
   \ "http"
Storage> 10  #选择10，Google Drive
Google Application Client Id - leave blank normally.
client_id>  #留空 
Google Application Client Secret - leave blank normally.
client_secret>  #留空
Service Account Credentials JSON file path - needed only if you want use SA instead of interactive login.
service_account_file> 
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine or Y didn't work
y) Yes
n) No
y/n> n  #选择n
If your browser doesn't open automatically go to the following link: https://accounts.google.com/o/oauth2/auth....  #复制到浏览器打开，获取验证码
Log in and authorize rclone for access
Enter verification code>  #填入上面获取到的验证码
Configure this as a team drive?
y) Yes
n) No
y/n> y  #选择y
Fetching team drive list...
No team drives found in your account--------------------
[ct]
client_id = 
client_secret = 
service_account_file = 
token = {"access_token":"ya29.GltFBd7UJN2qrxdG8FnG_rMjdL7glvXtfV"}
team_drive = 
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y  #选择y
Current remotes:
 
Name                 Type
====                 ====
lance                drive
 
e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q  #选择q退出
```
## 挂载
1. 在Google drive创建一个名为share的目录   mkdir -p /data/GoogleDrive
2. 将codesofun:share挂载到/data/GoogleDrive  rclone mount lance:share /data/GoogleDrive --allow-other --allow-non-empty --vfs-cache-mode writes &
>注意：
>Fatal error:failed to mount FUSE fs:fusermount:exec:"fusermount"：executable file not found in $PATH
>没有安装fuse，自行安装就可以了 yum -y install fuse

完成后挂载为磁盘

```
rclone mount lance:share /data/GoogleDrive --allow-other --allow-non-empty --vfs-cache-mode writes &
#格式为[name]:[google drive dir] [mount dir]
#[name]就是配置文件是输入的name，例如我的就是lance
#[google drive dir] 这个是谷歌云盘的目录，根目录的花直接空开就可以了
#[mount dir]就是本地挂载位置，/data/GoogleDrive
```
挂载完后的效果

```
[root@instance-5 ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        1.8G     0  1.8G   0% /dev
tmpfs           1.9G     0  1.9G   0% /dev/shm
tmpfs           1.9G  8.4M  1.8G   1% /run
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/sda1        10G  7.5G  2.6G  75% /
lance:share     1.0P     0  1.0P   0% /data/GoogleDrive
tmpfs           370M     0  370M   0% /run/user/1000
/dev/sdb        500G  374G  126G  75% /home/transmission/tmp
tmpfs           370M     0  370M   0% /run/user/0
```
这里谷歌云盘的1PB空间为教育版谷歌云盘
## 设置开机启动
```
wget http://ctnmb.com/dl/rcloned && vim rcloned
#然后修改文件内如下内容
NAME=""  #[name]
REMOTE=''  #[google drive dir]
LOCAL=''  #[mount dir]
```
设置自启动
```
mv rcloned /etc/init.d/rcloned
chmod +x /etc/init.d/rcloned
vim /etc/rc.d/rc.local #在末尾加入bash /etc/init.d/rcloned start
chmod +x /etc/rc.d/rc.local
```
设置完后以后重启也会自启动了
# 小结
之后登陆 web 端（http://域名:9091 或 http:// IP 地址:9091）就可以了，可以在里面设置你的云盘挂载地址（/data/GoogleDrive）为下载文件地址了，文件就可以下到你的网盘了，不至于谷歌云盘哦！

不过请注意你下载资源的版权，最好不要下载最近最火的电影和美剧，要不下完不要做种。



