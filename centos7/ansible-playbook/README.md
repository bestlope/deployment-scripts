### Centos7初始化说明
参考网站：http://www.jdccie.com/?p=3722

### 安装ansible
```
#Centos7
yum install -y epel-release
yum install git python-pip -y
# pip安装ansible(国内如果安装太慢可以直接用pip阿里云加速)
#pip install pip --upgrade
#pip install ansible==2.6.12
pip install pip --upgrade -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
pip install ansible==2.6.12 -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

```


### 使用说明
1、安装ansible后，修改/etc/ansible/hosts，添加[initialize-group]主机。
2、[initialize-group]的主机需要将远程操作主机设置ssh免密登录。
3、启动命令：ansible-playbook initialize.yaml

