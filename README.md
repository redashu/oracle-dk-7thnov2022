## Docker & K8s Training Plan 

<img src="plan.png">

### Revision 

<img src="rev.png">

### access docker volume from backend of docker server with root access 

```
[root@docker-ce-server ~]# cd  /var/lib/docker/
[root@docker-ce-server docker]# ls
builder  buildkit  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes
[root@docker-ce-server docker]# cd  volumes/
[root@docker-ce-server volumes]# ls
2bcfb1cc2bf3d43eb0e6bdeb98c88f8e200229bfec5d64fe0ab51bd0fd9e5d61  gitavol10    narasimhavol9  sonvol9
821c0943f31ce6b13359ceb6fbf398f61604f06cac8f8647d44e62d9be3540a8  gitavol9     navneet-1      sonvol99
926e3be0776608b60356c316cea3a9be11dfd404a800e9646b3e6ebfc57fe913  hari_db      navneetvol9    sooryavol9
aishvol9                                                          harivo19     navneetvol99   sooryavol99
aishvol99                                                         harivo199    sbvol9         venkatvol9
e328256af4bc21885aa4246aa938bb464ddec22177d9ff2773efafc01090030a  metadata.db  sbvol99        venkatvol99
[root@docker-ce-server volumes]# cd hari_db/
[root@docker-ce-server hari_db]# ls
_data
[root@docker-ce-server hari_db]# cd _data/
[root@docker-ce-server _data]# ls
#ib_16384_0.dblwr  #innodb_temp   binlog.000002  ca.pem           hari            ibtmp1     mysql.sock          public_key.pem   sys
#ib_16384_1.dblwr  auto.cnf       binlog.index   client-cert.pem  ib_buffer_pool  mysql      performance_schema  server-cert.pem  undo_001
#innodb_redo       binlog.000001  ca-key.pem     client-key.pem   ibdata1         mysql.ibd  private_key.pem     server-key.pem   undo_002
[root@docker-ce-server _data]# 

```

### Docker volume documentation 

[volume](https://docs.docker.com/storage/volumes/)


## Webapp contiainerization 

### Dockerfile

<img src="ep.png">

### nginx iNfo 

<img src="nginx.png">

### taking sample html based web app 

```
[ashu@docker-ce-server webapps]$ git clone https://github.com/schoolofdevops/html-sample-app.git
Cloning into 'html-sample-app'...
remote: Enumerating objects: 74, done.
remote: Total 74 (delta 0), reused 0 (delta 0), pack-reused 74
Unpacking objects: 100% (74/74), done.
[ashu@docker-ce-server webapps]$ ls
html-sample-app
[ashu@docker-ce-server webapps]$ 
```

### Dockerfile 

```
FROM nginx
LABEL name=ashutoshh
COPY html-sample-app /usr/share/nginx/html/
# using copy or add we can copy entire Folder data also
# Note: if we are not using CMD /ENTRYPOINT then base image CMD /entrypoint will be 
# inherited
```

### lets build image 

```
[ashu@docker-ce-server webapps]$ ls
Dockerfile  html-sample-app
[ashu@docker-ce-server webapps]$ docker build -t  ashunginx:appv1 . 
Sending build context to Docker daemon  3.744MB
Step 1/3 : FROM nginx
Trying to pull repository docker.io/library/nginx ... 
latest: Pulling from docker.io/library/nginx
e9995326b091: Pull complete 
71689475aec2: Pull complete 
f88a23025338: Pull complete 
0df440342e26: Pull complete 
eef26ceb3309: Pull complete 
8e3ed6a9e43a: Pull complete 
Digest: sha256:943c25b4b66b332184d5ba6bb18234273551593016c0e0ae906bab111548239f
Status: Downloaded newer image for nginx:latest
 ---> 76c69feac34e
Step 2/3 : LABEL name=ashutoshh
 ---> Running in 519e59b2abeb
Removing intermediate container 519e59b2abeb
 ---> d9a2ee99cb68
Step 3/3 : COPY html-sample-app /usr/share/nginx/html/
 ---> 8d86b31cd611
Successfully built 8d86b31cd611
Successfully tagged ashunginx:appv1
```

### checking image 

```
321  docker build -t  ashunginx:appv1 . 
  322  history 
[ashu@docker-ce-server webapps]$ docker images  |   grep ashu
ashunginx                               appv1               8d86b31cd611        34 seconds ago      145MB
ashustorage                             v1                  97bc1dfcde59        20 hours ago        5.54MB
dockerashu/oracleashu                   pyappv1             b866ef88a55e        23 hours ago        55.8MB
[ashu@docker-ce-server webapps]$ 
```

### creating port mapping 

```
ashu@docker-ce-server webapps]$ docker run -d --name ashuwebc1 -p 1234:80 ashunginx:appv1  
ff6b8e99a78323a48c9f55f518120cb06a30a7da01a2278c5d29bcad15ffca68
[ashu@docker-ce-server webapps]$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
2d5a936b8602        gitanx:appv1        "/docker-entrypoint.…"   5 seconds ago       Up 4 seconds        0.0.0.0:1029->80/tcp   gitawebc1
ff6b8e99a783        ashunginx:appv1     "/docker-entrypoint.…"   7 seconds ago       Up 6 seconds        0.0.0.0:1234->80/tcp   ashuwebc1
```

### Cgroups in Docker 

<img src="cg.png">

### putting memory limit in container 

```
[ashu@docker-ce-server webapps]$ docker run -d --name ashuwebc1 -p 1234:80  --memory 2G     ashunginx:appv1  
691021eb182f11ce44c53e33c70d17d490451f2e041ea0a79a233a56976f4ece
```

### cpu shares 

<img src="share.png">

## Docker compose -- to Scriptify all docker instructions

### compose info 

<img src="info.png">

### installing docker-compose 

```
[root@docker-ce-server ~]# curl -SL https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-x86_64  -o /usr/bin/docker-compose 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 42.8M  100 42.8M    0     0  7383k      0  0:00:05  0:00:05 --:--:-- 7168k
[root@docker-ce-server ~]# 
[root@docker-ce-server ~]# 
[root@docker-ce-server ~]# chmod +x /usr/bin/docker-compose 
[root@docker-ce-server ~]# docker-compose version 
Docker Compose version v2.12.2
[root@docker-ce-server ~]# 

```

### compose file example 1

```
version: '3.8' # compose file latest version 
services: # write app component 
  ashuwebapp:
    image: ashunginx:appv1 # docker image 
    container_name: ashuwebc111 # name of container 
    ports: # port mapping
    - "1234:80"
   
```

### running compose file 

```
[ashu@docker-ce-server ashuimages]$ ls
ashu-compose  java  python  storage  tasks  webapps
[ashu@docker-ce-server ashuimages]$ cd  ashu-compose/
[ashu@docker-ce-server ashu-compose]$ ls
docker-compose.yaml
[ashu@docker-ce-server ashu-compose]$ docker-compose up -d 
[+] Running 2/2
 ⠿ Network ashu-compose_default  Created                                                           0.1s
 ⠿ Container ashuwebc111         Started                                                           0.7s
[ashu@docker-ce-server ashu-compose]$ docker-compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuwebc111         "/docker-entrypoint.…"   ashuwebapp          running             0.0.0.0:1234->80/tcp
```

### more compose operations 

```
[ashu@docker-ce-server ashu-compose]$ docker-compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuwebc111         "/docker-entrypoint.…"   ashuwebapp          running             0.0.0.0:1234->80/tcp
[ashu@docker-ce-server ashu-compose]$ docker-compose  stop 
[+] Running 1/1
 ⠿ Container ashuwebc111  Stopped                                                                                             0.5s
[ashu@docker-ce-server ashu-compose]$ docker-compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuwebc111         "/docker-entrypoint.…"   ashuwebapp          exited (0)          
[ashu@docker-ce-server ashu-compose]$ docker-compose start
[+] Running 1/1
 ⠿ Container ashuwebc111  Started                                                                                             0.4s
[ashu@docker-ce-server ashu-compose]$ docker-compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuwebc111         "/docker-entrypoint.…"   ashuwebapp          running             0.0.0.0:1234->80/tcp
[ashu@docker-ce-server ashu-compose]$ 
```

### clean up 

```
[ashu@docker-ce-server ashu-compose]$ docker-compose down 
[+] Running 2/2
 ⠿ Container ashuwebc111         Removed                                                                                      0.5s
 ⠿ Network ashu-compose_default  Removed       
```


### Compose example 2

```
version: '3.8'
volumes: # to create volume
  ashudb-vol009: # name of volume 
services:
  ashuapp1:
    image: alpine
    container_name: ashuc111
    command: ping fb.com 
  ashuapp2:
    image: mysql
    container_name: ashudbc1 
    environment:
      MYSQL_ROOT_PASSWORD: "Oracle@098#"
    volumes: # attaching volume 
    - "ashudb-vol009:/var/lib/mysql/"

```

### lets run it 

```
[ashu@docker-ce-server ashu-compose]$ docker-compose down 
[ashu@docker-ce-server ashu-compose]$ ls
docker-compose.yaml  multic.yaml
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml  up  -d 
[+] Running 4/4
 ⠿ Network ashu-compose_default         Created                                                    0.1s
 ⠿ Volume "ashu-compose_ashudb-vol009"  Created                                                    0.0s
 ⠿ Container ashudbc1                   Started                                                    0.9s
 ⠿ Container ashuc111                   Started                                                    0.9s
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml ps 
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuc111            "ping fb.com"            ashuapp1            running             
ashudbc1            "docker-entrypoint.s…"   ashuapp2            running             3306/tcp, 33060/tcp
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml  down 
[+] Running 3/3
 ⠿ Container ashudbc1            Removed                                                           1.6s
 ⠿ Container ashuc111            Removed                                                          10.5s
 ⠿ Network ashu-compose_default  Removed                                                           0.1s
[ashu@docker-ce-server ashu-compose]$ 
```

### more compose examples 

```
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml  up -d 
[+] Running 3/3
 ⠿ Network ashu-compose_default  Created                                                           0.1s
 ⠿ Container ashudbc1            Started                                                           0.8s
 ⠿ Container ashuc111            Started                                                           0.9s
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml  ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuc111            "ping fb.com"            ashuapp1            running             
ashudbc1            "docker-entrypoint.s…"   ashuapp2            running             3306/tcp, 33060/tcp
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml  stop 
[+] Running 2/2
 ⠿ Container ashuc111  Stopped                                                                    10.4s
 ⠿ Container ashudbc1  Stopped                                                                     1.1s
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml  start ashuapp2
[+] Running 1/1
 ⠿ Container ashudbc1  Started                                                                     0.6s
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml  ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuc111            "ping fb.com"            ashuapp1            exited (137)        
ashudbc1            "docker-entrypoint.s…"   ashuapp2            running             3306/tcp, 33060/tcp
[ashu@docker-ce-server ashu-compose]$ 
```

### final compose example 

```
version: '3.8'
volumes: # to create volume
  ashudb-vol009: # name of volume 
services:
  ashuapp3: 
    image: ashunginx:appv001 # this image i want to build 
    build: # call dockerfile 
      context: ../webapps # location of dockerfile 
      dockerfile: Dockerfile # name of Dockerfile 
    container_name: ashuwebcc999 
    ports:
    - 1234:80 
  ashuapp1:
    image: alpine
    container_name: ashuc111
    command: ping fb.com 
  ashuapp2:
    image: mysql
    container_name: ashudbc1 
    environment:
      MYSQL_ROOT_PASSWORD: "Oracle@098#"
    volumes: # attaching volume 
    - "ashudb-vol009:/var/lib/mysql/"

```

### lets run it 

```
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml up -d --build 
[+] Building 1.0s (7/7) FINISHED                                                                        
 => [internal] load .dockerignore                                                                  0.0s
 => => transferring context: 2B                                                                    0.0s
 => [internal] load build definition from Dockerfile                                               0.0s
 => => transferring dockerfile: 326B                                                               0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                    0.0s
 => [internal] load build context                                                                  0.1s
 => => transferring context: 3.61MB                                                                0.0s
 => [1/2] FROM docker.io/library/nginx                                                             0.0s
 => => resolve docker.io/library/nginx:latest                                                      0.0s
 => [2/2] COPY html-sample-app /usr/share/nginx/html/                                              0.1s
 => exporting to image                                                                             0.9s
 => => exporting layers                                                                            0.9s
 => => writing image sha256:a6be2b5f496d1387762761dd8e9f4e11456288df337c9611c8b893278a591769       0.0s
 => => naming to docker.io/library/ashunginx:appv001                                               0.0s
[+] Running 3/3
 ⠿ Container ashuwebcc999  Started                                                                 0.6s
 ⠿ Container ashudbc1      Running                                                                 0.0s
 ⠿ Container ashuc111      Running                                                                 0.0s
[ashu@docker-ce-server ashu-compose]$ docker-compose -f multic.yaml ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
ashuc111            "ping fb.com"            ashuapp1            running             
ashudbc1            "docker-entrypoint.s…"   ashuapp2            running             3306/tcp, 33060/tcp
ashuwebcc999        "/docker-entrypoint.…"   ashuapp3            running             0.0.0.0:1234->80/tcp
```

### solution of docker container to docker image 

```
138  docker run -itd --name  ashucimg oraclelinux:8.4  
  139  docker  ps
  140  docker  exec -it ashucimg  bash 
  141  docker commit  ashucimg  ashucming:v007  
  142  docker images  |   grep ashu
  143  docker run -itd --name ashucc11 ashucming:v007
  144  docker  ps
  145  history 
[root@docker-ce-server ~]# docker  update  --restart always ashucc11 
ashucc11
[root@docker-ce-server ~]# docker tag  ashucming:v007  docker.io/dockerashu/ashucming:v007
[root@docker-ce-server ~]# docker login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[root@docker-ce-server ~]# docker push docker.io/dockerashu/ashucming:v007
The push refers to repository [docker.io/dockerashu/ashucming]
6636afd656a9: Pushed 
2d3586eacb61: Layer already exists 
v007: digest: sha256:e4e714e1c898d82a37c1a658c41e806b524eab4508ac936348a75e32903497c4 size: 742
[root@docker-ce-server ~]# docker logout 
Removing login credentials for https://index.docker.io/v1/
[root@docker-ce-server ~]# 

```

### cleaning up 

```
 152  docker rm $(docker ps -aq) -f
  153  docker rmi $(docker images -q) -f
  154  docker volume prune
  155  docker network prune 
  156  history 
[root@docker-ce-server ~]# docker  ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@docker-ce-server ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
[root@docker-ce-server ~]# docker volume ls
DRIVER              VOLUME NAME
[root@docker-ce-server ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
12ecbe47c660        bridge              bridge              local
61e2e20c26dd        host                host                local
424ed77057ea        none                null                local
[root@docker-ce-server ~]# 

```

## Understanding container orchestration problems 

<img src="prob.png">

### COntainer orchestration Engines 

<img src="carch.png">

### info about K8s 

<img src="k8s.png">

### k8s arch 1 

<img src="k8sarch1.png">

### method for setting k8s cluster 

<img src="cluster.png">

### INstalling k8s client software on CLient machine on LInux  -- kubectl 

```
[root@docker-ce-server ~]# curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   138  100   138    0     0    788      0 --:--:-- --:--:-- --:--:--   788
100 42.9M  100 42.9M    0     0  29.5M      0  0:00:01  0:00:01 --:--:-- 71.9M
[root@docker-ce-server ~]# ls
kubectl
[root@docker-ce-server ~]# mv kubectl  /usr/bin/
[root@docker-ce-server ~]# chmod  +x /usr/bin/kubectl 
[root@docker-ce-server ~]# 
[root@docker-ce-server ~]# kubectl  version --client 
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.3", GitCommit:"434bfd82814af038ad94d62ebe59b133fcb50506", GitTreeState:"clean", BuildDate:"2022-10-12T10:57:26Z", GoVersion:"go1.19.2", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
[root@docker-ce-server ~]# 

```

### Installing on MAC 

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   138  100   138    0     0    349      0 --:--:-- --:--:-- --:--:--   348
100 47.8M  100 47.8M    0     0  6054k      0  0:00:08  0:00:08 --:--:-- 7077k
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % sudo mv kubectl /usr/local/bin 
Password:
fire@ashutoshhs-MacBook-Air ~ % sudo chmod +x /usr/local/bin/kubectl
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % kubectl version --client 
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.3", GitCommit:"434bfd82814af038ad94d62ebe59b133fcb50506", GitTreeState:"clean", BuildDate:"2022-10-12T10:57:26Z", GoVersion:"go1.19.2", Compiler:"gc", Platform:"darwin/amd64"}
Kustomize Version: v4.5.7

```

### kubectl download LINK 

[Download](https://kubernetes.io/docs/tasks/tools/)









