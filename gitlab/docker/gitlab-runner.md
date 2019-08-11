### 这里要注意gitlab-runner的大版本要与gitlab的大版本一致，不然注册的时候会出现404错误。

docker run -d \
--net host \
 --name gitlab-runner \
--restart always \
-v $HOME/_data/gitlab_runner/srv/gitlab-runner/config:/etc/gitlab-runner \
-v $HOME/_data/gitlab_runner/var/run/docker.sock:/var/run/docker.sock \
gitlab/gitlab-runner:v12.1.0

### 注册命令

docker exec -it gitlab-runner gitlab-ci-multi-runner register

