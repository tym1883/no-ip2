![Docker Pulls](https://img.shields.io/docker/pulls/aanousakis/no-ip?style=plastic)
![Docker Image Size (tag)](https://img.shields.io/docker/image-size/aanousakis/no-ip/latest)

# Tags

- [latest](https://github.com/aanousakis/no-ip/blob/3.16/Dockerfile)
- [3.16](https://github.com/aanousakis/no-ip/blob/3.16/Dockerfile)
- [3.15](https://github.com/aanousakis/no-ip/blob/3.15/Dockerfile)
- [3.12](https://github.com/aanousakis/no-ip/blob/3.12/Dockerfile)

# NO-IP Dynamic DNS Update Client

A simple [Docker container](https://hub.docker.com/r/aanousakis/no-ip) for running the NO-IP dynamic DNS update script. It will keep current IP address in sync with your No-IP host or domain.

⚠️ Raspberry Pi users running 32 bit systems: The latest alpine update causes high cpu utilization. You can either use tag 3.12, which uses alpine:1.12 or [update libseccomp](https://github.com/alpinelinux/docker-alpine/issues/135#issuecomment-812287338)

## Usage

### Running  the container
   
There are two ways of running this container. As a regular container and as a service.

#### Regular container:
Running the image as a regular container uses environment variables to pass username, password, domains and interval from the host to the container. 



```bash
docker  run -d --name tym-ddns --restart=always  -e USERNAME=user1 -e PASSWORD=123 -e "DOMAINS=domain1.ddns.net" -e INTERVAL=5 aanousakis/no-ip


举个例子:
docker  run -d --name tym-ddns --restart=always -e USERNAME=111993393816@qq.com -e PASSWORD=xxxxxxxxx -e "DOMAINS=xxxxxx.ddns.net" -e INTERVAL=5 aanousakis/no-ip

命令参数说明:
--name tym-ddns  #容器的名字.
--restart=always #容器不管在什么情况下.都自动启动.
-e USERNAME=111993393816@qq.com  #noip 的账号.就是你在网页登录的时候的账号.
-e PASSWORD=xxxxxxxxx   #noip 的密码.就是你在网页登录的时候的密码. 这里注意:是明文的密码.
-e "DOMAINS=xxxxxx.ddns.net"     #  xxxxxx.ddns.net 就是你需要更新ip地址的域名.
-e INTERVAL=5     #更新间隔时间.单位是分钟.一般是5分钟就可以了.
aanousakis/no-ip  #镜像名称. 就是你想要使用哪个镜像 来生成名字是tym-ddns容器.重点是镜像.
docker logs xxx		#xxx 为容器的名字.也可以是容器的ID.  这条命令可以查看容器运行的日志.查看容器成功与否.
```
You can add multiple domains ex "DOMAINS=domain1.ddns.net domain2.ddns.net domain.foo.com"

#### As a service:
Running the image as a service uses secrets to pass username, password, domains and interval from the host to the container.

First you have to install docker compose

[installation instructions](https://docs.docker.com/compose/install/)

Then download docker-compose.yml file and the scripts to set Docker secrets. 

```bash
git clone https://github.com/aanousakis/no-ip.git    
cd no-ip/
```

Then you can deploy the service with :


```bash
docker swarm init
./deploy.sh 
```
The deploy.sh script will ask for your username, password, domains and update interval. Then it will set the secrets and start a service called no-ip_service. 

The service if configured to restart if an error occurs.


### Building the image
The image in Docker hub is build for the x86_64 architecture and cannot run on other platforms. If, for example, you want to run it on a host with arm architecture, you must build the image in that host.

```bash
git clone https://github.com/aanousakis/no-ip.git    
cd no-ip/
docker build --tag=aanousakis/no-ip .

```
## Author

* **Antony Anousakis**
