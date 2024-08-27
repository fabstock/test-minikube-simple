:
# test-minikube-simple
man on work


### in test no good for the moment


### create a certificate autosigned 
```bash

openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes  -subj "/CN=nodejs" 

kubectl -n kube-system create secret tls mkcert --key key.pem --cert cert.pem


```

### Some Notes testing 

  - minikube start  
  - minikube ip
  - minikube tunnel 
  -
  - minikube addons enable ingress (ingress-controller)
   ```
   
* ingress is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
  - Using image registry.k8s.io/ingress-nginx/controller:v1.10.1
  - Using image registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.4.1
  - Using image registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.4.1
* Verifying ingress addon...
* The 'ingress' addon is enabled

   ``` 
  -
  - simple test configmap  kubectl create configmap web-index --from-file=index.htm
  - 
  - minikube mount /data/web-storage:/data/webstorage  
    (not efficient speed  only testing) p9 .... 

  

  - kubectl  apply -f deploy-node5.yml

```shell 
fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl  apply -f deploy-node5.yml
deployment.apps/node-web created

fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl get nodes
NAME       STATUS   ROLES           AGE    VERSION
minikube   Ready    control-plane   161m   v1.30.0

fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl get deployments.apps
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
node-web   2/2     2            2           3m53s

fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl get pod
NAME                        READY   STATUS    RESTARTS   AGE
node-web-6d78586d44-56f8b   1/1     Running   0          4m
node-web-6d78586d44-n94g9   1/1     Running   0          4m1s

fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl get pod -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS       AGE
default       node-web-6d78586d44-56f8b          1/1     Running   0              4m1s
default       node-web-6d78586d44-n94g9          1/1     Running   0              4m2s
kube-system   coredns-7db6d8ff4d-278zd           1/1     Running   0              161m
kube-system   etcd-minikube                      1/1     Running   0              161m
kube-system   kube-apiserver-minikube            1/1     Running   0              161m
kube-system   kube-controller-manager-minikube   1/1     Running   0              161m
kube-system   kube-proxy-ff9n5                   1/1     Running   0              161m
kube-system   kube-scheduler-minikube            1/1     Running   0              161m
kube-system   storage-provisioner                1/1     Running   3 (100m ago)   161m

fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl get deployments.apps -A
NAMESPACE     NAME       READY   UP-TO-DATE   AVAILABLE   AGE
default       node-web   2/2     2            2           4m6s
kube-system   coredns    1/1     1            1           161m
```

### NodePort

  -  kubectl apply -f  svc-nodeport.yml


```shell
fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl get svc -A
NAMESPACE     NAME               TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)                  AGE
default       kubernetes         ClusterIP   10.96.0.1     <none>        443/TCP                  164m
default       node-web-service   NodePort    10.96.26.25   <none>        80:30946/TCP             30s
kube-system   kube-dns           ClusterIP   10.96.0.10    <none>        53/UDP,53/TCP,9153/TCP   164m
```

  -  kubectl delete  -f  svc-nodeport.yml

### LoadBalancer

  - kubectl apply -f  svc-loadbalancer.yml

```shell 
fab@debian12:~/fab-yaml/test-minikube-simple/sample-svc-pod-deployment$ kubectl get svc -A
NAMESPACE     NAME               TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                  AGE
default       kubernetes         ClusterIP      10.96.0.1        <none>           443/TCP                  166m
default       node-web-service   LoadBalancer   10.100.232.162   10.100.232.162   80:30677/TCP             5s
kube-system   kube-dns           ClusterIP      10.96.0.10       <none>           53/UDP,53/TCP,9153/TCP   166m
```
 
  - kubectl delete  -f svc-loadbalancer.yml


### ClusterIp


  - 
  - 
  -


### Ingress
  - kubectl create -f  ingress-minimal.yml
    plusieurs façons de faire NodePort ClusterIP LoadBalancer  et autres External 

   ```shell
   #type: NodePort          # Ou ClusterIP si vous utilisez Ingress
   externalIPs:
    - 192.168.49.2
   ``` 
   
    modifs de nodeport en externalIP 
    mais  ne corresponds pas a ce que je cherche 

   



  - kubectl delete deployments.apps node-web
  - kubectl delete services node-web-service


    
```shell

* Mounting host path /data/web-storage into VM as /data/webstorage ...
   - Mount type:   9p
   - User ID:      docker
   - Group ID:     docker
   - Version:      9p2000.L
   - Message Size: 262144
   - Options:      map[]
   - Bind Address: 192.168.49.1:44287
 * Userspace file server: ufs starting
 * Successfully mounted /data/web-storage to /data/webstorage

 * NOTE: This process must stay alive for the mount to be accessible ...


```


###  Other: testing vm-driver ... 
####  minikube start  --vm-driver=none 

 - for pv and pvc  hostpath 
 - no ip no tunnel  but pvc hostpath in host work 


```bash
fab@debian12:~/fab-yaml/simple-5$ minikube start --vm-driver=none


* minikube v1.33.1 on Debian 12.6 (vbox/amd64)
* Using the none driver based on user configuration
! --static-ip is only implemented on Docker and Podman drivers, flag will be ignored
* Starting "minikube" primary control-plane node in "minikube" cluster
* Running on localhost (CPUs=4, Memory=6490MB, Disk=126039MB) ...
* OS release is Debian GNU/Linux 12 (bookworm)
    > kubeadm.sha256:  64 B / 64 B [-------------------------] 100.00% ? p/s 0s
    > kubectl:  49.07 MiB / 49.07 MiB [--------------] 100.00% 2.28 MiB p/s 22s
  - Generating certificates and keys ...
  - Booting up control plane ...
  - Configuring RBAC rules ...
* Configuring bridge CNI (Container Networking Interface) ...
* Configuring local host environment ...
*
! The 'none' driver is designed for experts who need to integrate with an existing VM
* Most users should use the newer 'docker' driver instead, which does not require root!
* For more information, see: https://minikube.sigs.k8s.io/docs/reference/drivers/none/
*
! kubectl and minikube configuration will be stored in /home/fab
! To use kubectl or minikube commands as your own user, you may need to relocate them. For example, to overwrite your own settings, run:
*
  - sudo mv /home/fab/.kube /home/fab/.minikube $HOME
  - sudo chown -R $USER $HOME/.kube $HOME/.minikube
*
* This can also be done automatically by setting the env var CHANGE_MINIKUBE_NONE_USER=true
* Verifying Kubernetes components...
  - Using image gcr.io/k8s-minikube/storage-provisioner:v5
* Enabled addons: default-storageclass, storage-provisioner
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```


<div style="background-color: #f0f0f0; padding: 10px; border-radius: 5px;">
  <pre><code>
  # Commande bash
  echo "Hello, World!"
  </code></pre>
</div>

<div style="background-image: url('https://via.placeholder.com/1x1/282a36/282a36.png'); padding: 10px; border-radius: 5px; background-size: cover; color: #f8f8f2; font-family: monospace;">
  <pre><code>
# Commande bash
echo "Hello, World!"
  </code></pre>
</div>

