## Docker & K8s Training Plan 

<img src="plan.png">

### creating deployment 

```
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl create  deployment  ashu-dep1 --image=dockerashu/ashuapp:v001  --port 80 --dry-run=client -o yaml >deploy.yaml 
fire@ashutoshhs-MacBook-Air k8s-app-deploy % ls
deploy.yaml
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl apply -f deploy.yaml 
deployment.apps/ashu-dep1 created
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl get  deploy 
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
ashu-dep1   1/1     1            1           20s
```

### creating service 

```
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl expose deployment  ashu-dep1 --type NodePort --port 8
0 --name ashulb2 --dry-run=client -o yaml >np.yaml 
fire@ashutoshhs-MacBook-Air k8s-app-deploy % ls
deploy.yaml     np.yaml
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl apply -f np.yaml 
service/ashulb2 created
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl get svc 
NAME      TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
ashulb2   NodePort   10.98.71.31   <none>        80:31285/TCP   6s
fire@ashutoshhs-MacBook-Air k8s-app-deploy % 
```



