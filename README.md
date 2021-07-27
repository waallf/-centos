# Install zksg -centos
中科曙光服务器安装centos
### 专用系统下载地址：  
https://pan.baidu.com/s/1gVJz-FzA5jDKEMR-rUCZsw   
提取码： doyq  
### 问题1：U盘安装找不到系统 No Controller found  
1. 等待命令行出现  
2. 输入ls /dev/sd* 找到自己的u盘名称  
3. 在进入到安装界面 Install centos7.6界面时，按’e‘进入编辑界面，改为vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sdc4 quite 就是把hd: 到quite之间的内容改为U盘所挂在的地址  
4. ctrl+X 重新启动 然后正常安装  
### 问题2： 配置ssh服务，没有办法修改端口  
![参考](https://www.laozuo.org/11261.html)  
1. 检查SElinux是否关闭,sestatus 如果是disable即可，否则关闭  
   (1). 修改/etc/selinux/config 文件  
   (2). 将SELINUX=enforcing改为SELINUX=disabled  
   (3). 重启计算机即可  
2. 修改/etc/ssh/sshd_config 文件：
   (1). 将# 去掉： #Port 22; #ListenAddress 0.0.0.0; #ListenAddress::;#PermitRootLogin yes; #PasswordAuthentication yes    
   (2). 启动sshd服务 sudo service sshd restart    
   (3). 查看sshd服务是否启动 ps-e | grep sshd  
3. 如果修改端口，需要在防火墙中放行端口  
   firewall-cmd --permanent --zone=public --add-port=21212/tcp  
   firewall-cmd --reload  
   irewall-cmd --permanent --query-port=21212/tcp #查看端口是否放行  
### 问题3 配置静态IP  
1. cd /etc/sysconfig/network-scripts  
2. 找到对应的网卡 vi ifcfg-***    
3. 设置：BOOTPROTO=static  
        ONBOOT=yes  
        IPADDR=IP地址  
        NETMASK=掩码  
        GATEWAY=网关  
4. 重启网卡 systemctl restart network  
### 问题4：配置时间校正  
1. ntpdate 服务器地址  
2. 设置每隔一小时自动执行    
   (1). vi /etv/crontab  
   (2). * */1 * * * root ntpdate 服务器地址  
