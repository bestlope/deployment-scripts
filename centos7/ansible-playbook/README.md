### Centos7初始化说明
参考网站：http://www.jdccie.com/?p=3722

### 使用说明
1、安装ansible后，修改/etc/ansible/hosts，添加[initialize-group]主机。
2、[initialize-group]的主机需要将远程操作主机设置ssh免密登录。
3、启动命令：ansible-playbook initialize.yaml

