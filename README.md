# dnmp
Docker deploying Nginx MySQL PHP7 in one key, support full feature functions.

![Demo Image](./dnmp.png)

### Feature
1. Completely open source.
2. Support Multiple PHP version(PHP5.4, PHP5.6, PHP7.2) switch.
3. Support Multiple domains.
4. Support HTTPS and HTTP/2.
5. PHP source located in host.
6. MySQL data directory in host.
7. All conf files located in host.
8. All log files located in host.
9. Built-in PHP extensions install commands.
10. Promise 100% available.
11. Supported any OS with docker.

### Usage
1. Install `git`, `docker` and `docker-compose`;
2. Clone project:
    ```
    git clone https://github.com/osindex/dnmp.git
    #git clone --recursive #使用EMQ
    ```
3.use registry-mirrors
    ```
	https://zhlnsbrc.mirror.aliyuncs.com 
    ```
4. Start docker containers:
    ```
    docker-compose up #  --build 当某项安装失败时附上
    ```
    You may need use `sudo` before this command in Linux.
5. Go to your browser and type `localhost`, you will see:

![Demo Image](./snapshot.png)

The index file is located in `./www/site1/`.

### Other PHP version?
Default, we start LATEST PHP version by using:
```
docker-compose up
```
we can also start PHP5.4 or PHP5.6 by using:
```
docker-compose -f docker-compose54.yml up
docker-compose -f docker-compose56.yml up
```
We need not change any other files, such as nginx config file or php.ini, everything will work fine in current environment (except code compatibility error).

> Notice: We can only start one php version, for they using same port. We must STOP the running project then START the other one.

### HTTPS and HTTP/2
Default demo include 2 sites:
* http://site1.site.com (same with http://localhost)
* https://site2.site.com

To preview them, add 2 lines to your hosts file (at `/etc/hosts` on Linux and `C:\Windows\System32\drivers\etc\hosts` on Windows):
```
127.0.0.1 site1.site.com
127.0.0.1 site2.site.com
```
Then you can visit from browser.

##[使用EMQ](http://docs.emqtt.cn/zh_CN/latest/guide.html#id2)
```
#git submodule init #已经git clone 但是只有文件夹没有内容
因为使用本地文件会重新编译 比较耗时 简单的办法是
wget http://emqtt.com/downloads/latest/docker
unzip emqttd-docker-v2.3.1.zip # 具体自己更换当前版本
docker load < emqttd-docker-v2.3.1
#修改docker.compose.yml 不再使用build取消注释image
image: emqttd-docker-v2.3.1:latest
docker-compose up --build

#用户名密码认证
#基于 MQTT 登录用户名(username)、密码(password)认证。
#etc/plugins/emq_auth_username.conf 中配置默认用户:
#docker exec -it dnmp_emq_1 vim /opt/emqttd/etc/plugins/emq_auth_username.conf
auth.user.$N.username = admin
auth.user.$N.password = public
#访问localhost:18083
```
##不再使用swoole 
```
将swoole的服务注释掉 
swoole.site.com的nginx配置文件改后缀名
再重新启动即可
```
##常规用法
```
docker-compose up -d #托管启动
docker-compose ps #查看状态
docker-compose restart #重新启动
docker rm -f $(docker ps -qa) #移除所有
docker rm `docker ps -a -q`  #删除非运行的容器 
docker rmi -f `docker images | grep '<none>' | grep -v -E 'mysql|alpine' | awk '{print $3}'` #删除docker无引用的镜像
```
##Reload例子
```
docker exec -it dnmp_nginx_1 /usr/local/openresty/nginx/sbin/nginx -s reload
```
