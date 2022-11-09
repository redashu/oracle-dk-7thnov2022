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


### DOcker networking --






