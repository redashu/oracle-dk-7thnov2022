## Docker & K8s Training Plan 

<img src="plan.png">

### REvision 

<img src="rev.png">

### docker root directory system 

```
[root@docker-ce-server ~]# cd  /var/lib/docker/
[root@docker-ce-server docker]# ls
builder  buildkit  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes
[root@docker-ce-server docker]# 


```

### Docker Client & server connection 

<img src="dk.png">


### PYthon code -- dockerfile 

```
FROM oraclelinux:8.4
LABEL name=ashutoshh
LABEL email=ashutoshh@linux.com 
# Info about image creator 
RUN yum install python3 -y && mkdir /newcode
ADD https://raw.githubusercontent.com/redashu/pythonLang/main/while.py /newcode/
# COPY and ADD both are same keyword but ADD can take data from URL as well
WORKDIR /newcode
# changing directory for this image 
CMD ["python3","while.py"]
# we are setting default process for this new image 
```

### lets build it 

```
[ashu@docker-ce-server ashuimages]$ ls
java  python  webapps
[ashu@docker-ce-server ashuimages]$ docker  build -t  ashupython:v1 python/ 
Sending build context to Docker daemon  2.048kB
Step 1/7 : FROM oraclelinux:8.4
 ---> 97e22ab49eea
Step 2/7 : LABEL name=ashutoshh
 ---> Running in 3b446f0534fc
Removing intermediate container 3b446f0534fc
 ---> 5acdbef27c75
Step 3/7 : LABEL email=ashutoshh@linux.com
 ---> Running in 8c074afa9d00
```
### checking it 

```
  149  docker  build -t  ashupython:v1 python/ 
  150  history 
[ashu@docker-ce-server ashuimages]$ docker images  |  grep ashu
ashupython                                     v1                  fe3d3c9e1a1a        35 seconds ago      448MB
ashujava                                       jdk8_v1             aa8d560c81d5        18 hours ago        652MB
ashujava                                       1.2                 7a5157c011f5        18 hours ago        464MB
ashujava                                       1.1                 56c67dfaca7a        18 hours ago        464MB
```

### creating container 

```
[ashu@docker-ce-server ashuimages]$ docker run -itd  --name ashupyc1  ashupython:v1  
03715d1e0a3d8ceac97c9797b2d1ac4bc6e8b3689b0e5a1b8fd0e2da131a3d32
[ashu@docker-ce-server ashuimages]$ docker  ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
62c40dfb90ab        gitapython:v1       "python3 while.py"       7 seconds ago       Up 3 seconds                            gitanjalic1
a0132d910d73        sooryapython:v1     "python3 /newcode/wh…"   8 seconds ago       Up 3 seconds                            soopythonc
aa0b1a6e1da7        99bda13f92d2        "python3 /newcode/wh…"   8 seconds ago       Up 3 seconds                            manjunathpythonc1
03715d1e0a3d        ashupython:v1       "python3 while.py"       11 seconds ago      Up 3 seconds                            ashupyc1
```

### checking logs 

```
[ashu@docker-ce-server ashuimages]$ docker  ps  |   grep ashu
03715d1e0a3d        ashupython:v1       "python3 while.py"       56 seconds ago      Up 48 seconds                           ashupyc1
[ashu@docker-ce-server ashuimages]$ docker logs  ashupyc1 
Hello all , welcome to python..!!
Welcome to LnB..
Welcome to Containers ..!!
______________________
Hello all , welcome to python..!!
Welcome to LnB..
```

### lets clean up 

```
  159  history 
[ashu@docker-ce-server ashuimages]$ docker  kill ashupyc1 
ashupyc1
[ashu@docker-ce-server ashuimages]$ docker rm ashupyc1 
ashupyc1
[ashu@docker-ce-server ashuimages]$ 
```





