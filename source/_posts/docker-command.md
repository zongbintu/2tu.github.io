title: docker-command
tags: []
date: 2026-01-27 09:09:07
categories:
---

启动docker服务
```
# 启动 Docker 服务
sudo systemctl start docker

# 2. 重启 Docker
sudo systemctl daemon-reload
sudo systemctl restart docker

```

换源
```
sudo vi /etc/docker/daemon.json
# 内容
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://mirror.baidubce.com",
    "https://hub-mirror.c.163.com"
  ],
  "ipv6": false
}

# 重新加载 重启服务
sudo systemctl daemon-reload
sudo systemctl restart docker

```

构建镜像
```
# 构建镜像
docker build -t postgres:latest .

# 持久化 docker Volume挂载
# 创建 volume（一次即可）
docker volume create pgdata

# 启动镜像
docker run -d --name postgres-container  --restart=always -p 5432:5432 -v pgdata:/var/lib/postgresql/data postgres


# 下次再启动该镜像
docker start postgres-container

# 删除镜像
docker rm postgres-container
```

```


```
# 查看容器日志 
docker logs postgres-container
# 进入容器
docker exec -it container_name bash
```

# 操作pgsql
```
psql -U myuser -d stock

```

