# Ubuntu 16.04 安装samba服务
 - 安装samba服务
```powershell
sudo apt-get install samba samba-common
```
 - 创建用于共享的目录
```powershell
sudo mkdir /home/long/share
```
 - 给共享目录设置权限
```powershell
sudo chmod 777 /home/long/share
```
 - 添加samba服务器本地账户，添加密码
```powershell
sudo smbpasswd -a long
```
 - 修改samba服务器配置文件

```powershell
sudo vim /etc/samba/smb.conf
[share]
path = /home/long/share
available = yes
browseable = yes
public = yes
writable = yes
valid users = long
create mask = 0700
directory mask =0700
force user = nobody
force group = nogroup
```
 - 重启samba服务器
```powershell
sudo service smbd restart
```
 - windows 系统下输入ubuntu系统的IP，检测samba服务是否正常工作
 

 - 可以解决中文被显示为乱码的问题
```powershell
［global］里面添加如下2行
unix charset = UTF-8
dos charset = cp936
```
 - smb.conf 参数说明
```powershell
[共享名称]:共享中看到的共享目录名
comment = 共享的描述. 
path = 共享目录路径(可以用%u、%m这样的宏来代替路径如:/home/share/%u) 
browseable = yes/no指定该共享是否在“网上邻居”中可见。
writable = yes/no指定该共享路径是否可写。
read only = yes/no设置共享目录为只读(注意设置不要与writable有冲突) 
available = yes/no指定该共享资源是否可用。
admin users = bobyuan，jane指定该共享的管理员,用户验证方式为“security=share”时，此项无效。 
valid users = bobyuan，jane允许访问该共享的用户或组-“@+组名” 
invalid users = 禁止访问该共享的用户与组(同上) 
write list = 允许写入该共享的用户
public = yes/no共享是否允许guest账户访问。 
guest ok = yes/no意义同“public”。**windows 映射不了的时，可以添加这个**
create mask = 0700指定用户通过Samba在该共享目录中创建文件的默认权限。0600代表创建文件的权限为rw-------
directory mask = 0700指定用户通过Samba在该共享目录中创建目录的默认权限。0600代表创建目录的权限为rwx---
```
