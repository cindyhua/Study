### Q:安装ubuntu-16.10 box后，用apt-get更新文件时，发现链接的是美国IP

### A:更新镜像
修改/etc/apt/sources.list

`sudo gedit /etc/apt/sources.list`

编辑/etc/apt/sources.list文件, 在文件最前面添加以下条目(操作前请做好相应备份)

```
deb http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
````
参考：http://mirrors.163.com/.help/ubuntu.html

其中`precise`代表的是ubuntu各个版本的内部代号，要替换成对应版本的代号，例如16.10的代号为`yakkety`,通过`lsb_realse -a`可以查看
```
vagrant@db:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 16.10
Release:        16.10
Codename:       yakkety
```


修改之后， 执行

`sudo apt-get update`

### Q:安装Cent-OS7 后，用SecureCRT登录失败

` SSH error:a public key file has not been specified by this session `

![image](https://github.com/cindyhua/Study/blob/master/vgrant/images/SecureCRT_fail_0.png)

### A:SSH客户端开启密码验证

通过virtualbox登录

修改/etc/ssh/sshd_config的PasswordAuthentication项为yes

重启服务
`sudo service sshd restart`


### Q:如何获取root账号密码
### A:设置root密码

`sudo passwd root`

### Q:配置转发端口失败
按照教程Vagrantfile文件中添加如下配置：

` config.vm.network :forwarded_port, guest: 80, host: 8080`

执行`vagrant relaod`后报错
```
==> web: You assigned a static IP ending in ".1" to this machine.
==> web: This is very often used by the router and can cause the
==> web: network to not work properly. If the network doesn't work
==> web: properly, try changing this IP.
==> web: You assigned a static IP ending in ".1" to this machine.
==> web: This is very often used by the router and can cause the
==> web: network to not work properly. If the network doesn't work
==> web: properly, try changing this IP.
D:/Program Files/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.3/lib/vagrant/util/is_port_open.rb:21:in `initialize': The requested address is not valid in its context. - connect(2) for "0.0.0.0" port 8080 (Errno::EADDRNOTAVAIL)
        from D:/Program Files/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.3/lib/vagrant/util/is_port_open.rb:21:in `new'
        from D:/Program Files/HashiCorp/Vagrant/embedded/gems/gems/vagrant-1.9.3/lib/vagrant/util/is_port_open.rb:21:in `block in is_port_open?'
        ...
```

### A:配置项添加`host_ip`
完整配置项修改为：

` config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1", auto_correct: true`

解决方案来自：
https://github.com/mitchellh/vagrant/issues/8395
 
