# test-minikube-simple
man on work


### in test no good for the moment

```bash

openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes  -subj "/CN=nodejs" 

kubectl -n kube-system create secret tls mkcert --key key.pem --cert cert.pem


```

- Some Notes testing 

  minkube start 
  minkube ip
  minikube tunnel 
  minikube mount /data/web-storage:/data/webstorage


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


###  testing ... 
###  minikube start  --vm-driver=none 

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
