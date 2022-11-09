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

<img src="dockerfile.png">

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







