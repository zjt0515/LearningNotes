# ssh

## 登录服务器

登录服务器：`ssh user@hostname`



配置文件

`~/.ssh/config`

```
Host myserver1
    HostName IP地址或域名
    User 用户名

Host myserver2
    HostName IP地址或域名
    User 用户名
```

`ssh myserver1`




##  密钥登陆

### 客户端操作
创建密钥：`ssh-keygen`，然后一直回车

完成后`~/.ssh/`新增2个文件

- id_rsa
- id_rsa.pub

### 服务器端操作

将id_rsa.pub中的内容复制到复制到服务器中的~/.ssh/authorized_keys文件里

也可以在客户端使用如下命令一键添加公钥：`ssh-copy-id myserver`





## 本地执行服务端上的命令

`ssh user@hostname [command]`



`ssh myserver 'for ((i = 0; i < 10; i ++ )) do echo $i; done`

`# 单引号中的$i可以求值`

# scp

复制多个文件

`scp source1 source2 destination`

复制文件夹

`scp -r myserver:homework .`





指定服务器的端口号：

`scp -P 22 source1 source2 destination`

