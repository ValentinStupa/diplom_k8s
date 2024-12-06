## Deploy k8s cluster for diplom project -- in progress....

[Установка k8s кластера при помощи kubespray](https://github.com/kubernetes-incubator/kubespray.git) / [или](https://sysadmintalks.ru/deploy-kubernetes-cluster-kubespray/)  

Выполнить ряд команд на мастере:

```
git clone https://github.com/kubernetes-incubator/kubespray.git
cd kubespray/
```
 Копируем заготовку инвентори и вносим свои правки:
```
cp -rfp inventory/sample/ inventory/cluster
vim inventory/cluster/inventory.ini
```
Готовый инветори файл в моем случае выглядит так [inventory.ini](./kubespray/inventory/cluster/inventory.ini):
Вставить свой приватный ключ в файл
```
vim ~/.ssh/id_key
chmod 600 ~/.ssh/id_key
```
Установить зависимости:
```
python3.11 -m pip install -r requirements.txt
```
Включить helm/ingress
```
vim inventory/cluster/group_vars/k8s_cluster/addons.yml
```
Запустить плейбук
```
ansible-playbook -i inventory/cluster/inventory.ini cluster.yml -b -v
```
Скопировать конфиг для текущего пользователя
```  
mkdir ~/.kube
sudo cp -i /etc/kubernetes/admin.conf ~/.kube/config
sudo chown `id -u`:`id -g` ~/.kube/config
```
Проверить работу и доступность кластера
```
  kubectl get pods --all-namespaces

[almalinux@master01 ~]$ kk get po --all-namespaces
NAMESPACE       NAME                                       READY   STATUS    RESTARTS      AGE
ingress-nginx   ingress-nginx-controller-58nrx             1/1     Running   0             3h36m
ingress-nginx   ingress-nginx-controller-tbqsh             1/1     Running   0             3h36m
kube-system     calico-kube-controllers-55d498b656-dxrfp   1/1     Running   0             3h36m
kube-system     calico-node-5c6d8                          1/1     Running   0             3h38m
kube-system     calico-node-dlv28                          1/1     Running   0             3h38m
kube-system     calico-node-fz9kn                          1/1     Running   0             3h38m
kube-system     coredns-d665d669-2lssb                     1/1     Running   0             3h33m
kube-system     coredns-d665d669-zvzw9                     1/1     Running   0             3h35m
kube-system     dns-autoscaler-5cb4578f5f-fjjzr            1/1     Running   0             3h35m
kube-system     kube-apiserver-master01                    1/1     Running   1             3h40m
kube-system     kube-controller-manager-master01           1/1     Running   5 (98m ago)   3h40m
kube-system     kube-proxy-5btqc                           1/1     Running   0             3h39m
kube-system     kube-proxy-jptbh                           1/1     Running   0             3h39m
kube-system     kube-proxy-k54nq                           1/1     Running   0             3h39m
kube-system     kube-scheduler-master01                    1/1     Running   3 (98m ago)   3h40m
kube-system     nginx-proxy-worker01                       1/1     Running   0             3h39m
kube-system     nginx-proxy-worker02                       1/1     Running   0             3h39m
kube-system     nodelocaldns-ktz65                         1/1     Running   0             3h35m
kube-system     nodelocaldns-nvbnt                         1/1     Running   0             3h35m
kube-system     nodelocaldns-xfw7t                         1/1     Running   0             3h35m

```
---

## Манифесты:

 - [Nginx+Service](./manifest_files/nginx_deployment.yml)
 - [Ingress](./manifest_files/ingress.yml)