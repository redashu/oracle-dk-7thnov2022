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

### creating externla LB 

<img src="lb.png">

### creating Loadbalancer & NodepOrt service 

```
 kubectl expose deployment ashu-dep1  --type LoadBalancer --port 80 --name ashulb999       --dry-run=client -o yaml >loadbalancer.yaml
fire@ashutoshhs-MacBook-Air k8s-app-deploy % 
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl  get  svc 
NAME        TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
ashulb2     NodePort       10.98.71.31    <none>        80:31285/TCP   52m
ashulb999   LoadBalancer   10.96.154.10   <pending>     80:30038/TCP   41s
fire@ashutoshhs-MacBook-Air k8s-app-deploy % 

```

### problem in Having app loadbalancer -- in case of multi app hosting 

<img src="apph.png">

### Ingress controller usage in k8s 

<img src="ingress.png">

### options in INgress controller 

<img src="ingressc.png">

### setup Ingress controller -- 

```
fire@ashutoshhs-MacBook-Air Desktop % kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/baremetal/deploy.yaml
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
serviceaccount/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx created
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
configmap/ingress-nginx-controller created
service/ingress-nginx-controller created
service/ingress-nginx-controller-admission created
deployment.apps/ingress-nginx-controller created
job.batch/ingress-nginx-admission-create created
job.batch/ingress-nginx-admission-patch created
ingressclass.networking.k8s.io/nginx created
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
fire@ashutoshhs-MacBook-Air Desktop % 
```

### verify it 

```
fire@ashutoshhs-MacBook-Air Desktop % kubectl get  ns
NAME              STATUS   AGE
aishh-project     Active   135m
ashu-project      Active   139m
default           Active   23d
gita-project      Active   139m
hari-project      Active   138m
ingress-nginx     Active   30s
kube-node-lease   Active   23d
kube-public       Active   23d
kube-system       Active   23d
manju-project     Active   139m
navneet-project   Active   138m
rubi-project      Active   139m
sb-project        Active   138m
sontosh-project   Active   139m
soorya-project    Active   133m
venkat-project    Active   139m
fire@ashutoshhs-MacBook-Air Desktop % kubectl get  deploy -n ingress-nginx
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
ingress-nginx-controller   1/1     1            1           38s
fire@ashutoshhs-MacBook-Air Desktop % kubectl get  deploy -n ingress-nginx
fire@ashutoshhs-MacBook-Air Desktop % kubectl get  svc -n ingress-nginx   
NAME                                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             NodePort    10.109.170.190   <none>        80:31782/TCP,443:32292/TCP   57s
ingress-nginx-controller-admission   ClusterIP   10.97.103.129    <none>        443/TCP                      57s
fire@ashutoshhs-MacBook-Air Desktop % 

```

### along with Ingress -- we have to prefer ClusterIP type service 

<img src="clusterip.png">

### YAML to perform task

```
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: ashuk8s1
spec: {}
status: {}
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod1
  name: ashupod1
  namespace: ashuk8s1 # namespace info 
spec:
  containers:
  - image: ubuntu
    name: ashupod1
    command: ['sh','-c','sleep 10000'] # process of container 
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashusvc1
  name: ashusvc1
  namespace: ashuk8s1 
spec:
  ports:
  - name: 1234-80
    port: 1234
    protocol: TCP
    targetPort: 80
    nodePort: 31009
  selector:
    app: ashusvc1
  type: NodePort
status:
  loadBalancer: {}

```

###

```
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl apply -f mytask.yaml 
namespace/ashuk8s1 created
pod/ashupod1 created
service/ashusvc1 created
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl  get ns
NAME              STATUS   AGE
aishh-project     Active   4h2m
ashu-project      Active   4h6m
ashuk8s1          Active   8s
default           Active   23d
gita-project      Active   4h5m
fire@ashutoshhs-MacBook-Air k8s-app-deploy % 
fire@ashutoshhs-MacBook-Air k8s-app-deploy % 
fire@ashutoshhs-MacBook-Air k8s-app-deploy % 
fire@ashutoshhs-MacBook-Air k8s-app-deploy % kubectl  get po,svc -n ashuk8s1
NAME           READY   STATUS    RESTARTS   AGE
pod/ashupod1   1/1     Running   0          21s

NAME               TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/ashusvc1   NodePort   10.108.90.238   <none>        1234:31009/TCP   21s
fire@ashutoshhs-MacBook-Air k8s-app-deploy % 
```


