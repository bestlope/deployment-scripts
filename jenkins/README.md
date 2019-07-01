
## 使用jenkins创建docker镜像
### 登录jenkins
使用以下命令创建jenkins服务：
```
docker run -d -u root --name jenkins \
    -p 8089:8080 -p 50000:50000 \
    -v /jenkins/jenkins_21384:/var/jenkins_home \
    jenkins/jenkins:2.138.4
```

### 登录jenkins面板
在网页上打开：http://IP:8089 ,进入配置界面需要输入密码，通过docker命令获取密码：
```
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

配置jenkins的时候，确保git插件安装上了，下面操作需要用到git仓库下载代码。

### 配置docker插件
#### 安装docker插件
1、在jenkins网页面板，选择面板左边的’系统管理‘。

2、在’系统管理‘，选择’插件管理‘。

3、选择’可选插件‘，然后再’过滤‘中输入：docker；选择：’Docker plugin：This plugin integrates Jenkins with Docker‘,直接安装。

4、安装完成，jenkins自动重启后docker插件安装完成。

### 配置Docker Host URI
此URI是用于jenkins使用的docker，可以是jenkins上的主机，也可以是内网能连接的其他主机。使用以下命令配置URI：

```
# 修改docker.service文件
vim /usr/lib/systemd/system/docker.service

# 注释掉：
ExecStart=/usr/bin/dockerd -H fd://

# 添加：
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2345 -H unix:///var/run/docker.sock

# 保存Docker.service文件

# 刷新配置
systemctl daemon-reload

# 重新启动Docker守护进程
systemctl restart docker

```
如果是在192.168.123.152这个IP主机上配置的，那么Docker Host URI就是：’tcp：//192.168.123.152:2345‘


### 添加docker agent
#### 配置docker插件
这个步骤的配置是为了让docker插件连接docker主机/进程。

1、进入’系统管理‘。

2、进入’系统设置‘全局设置。

3、在最后，’新增一个云‘，选择’docker‘。

4、添入刚配置的’Docker Host URI‘，在这里是：'tcp://192.168.123.152：2345'

5、使用’Test Connection‘,如果显示出你的docker版本，就是测试通过，可以使用了。

#### 配置Docker Agent Template
1、点击’Docker Agent templates‘，然后’Add Docker Template‘。

2、在labels添入：’docker-agent‘。

3、在Docker Image添入：’benhall/dind-jenkins-agent:v2‘。   这个镜像的地址是:https://hub.docker.com/r/benhall/dind-jenkins-agent/

4、在Volumes添入：’/var/run/docker.sock:/var/run/docker.sock‘，让创建的容器可以和主机联系。

5、在’Connect Method‘选择’Connect with SSH‘。

6、保存设置。

### 创建容器制作项目
使用katacoda上的一个网页示例来制作镜像，代码地址：https://github.com/katacoda/katacoda-jenkins-demo

#### 创建一个新项目
1、在jenkins面板，创建新项目。

2、取一个易懂的项目名称：Katacoda Jenkins Demo，选择：自由风格项目。

3、进入项目后，在’标签表达式‘添入：’docker-agent‘，如果显示：’	
Label docker-agent is serviced by no nodes and 1 cloud. Permissions or other restrictions provided by plugins may prevent this job from running on those nodes.‘就可以继续；如果显示：’There’s no agent/cloud that matches this assignment. Did you mean ‘master’ instead of ‘docker-agent’?‘，说明上面配置docker的步骤可能有问题，返回去检查下配置。

4、源码管理，使用Git，代码地址：https://github.com/katacoda/katacoda-jenkins-demo

5、在最下面的’增加构建步骤‘，选择’Execute Shell‘,使用脚本命令操作。

6、输入以下命令：
```
ls
docker info
docker build -t katacoda/jenkins-demo:${BUILD_NUMBER} .
docker tag katacoda/jenkins-demo:${BUILD_NUMBER} katacoda/jenkins-demo:latest
docker images

```
7、项目配置完成，现在可以点击’立即构建‘，等待镜像构建完成。

8、镜像完成后，跑一下该镜像，证明构建是成功的：
```
docker run -d -p 81:80 katacoda/jenkins-demo:latest

curl 192.168.123.152:81
```


