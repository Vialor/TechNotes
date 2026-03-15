# Pod
cluster
	node: 物理上pod的存在位置
	namespace: 逻辑上pod的存在位置
		pod：一个pod内的容器共享网络栈和外存（通过挂载），是k8s中最小的可调度和可部署单位
			container(Docker)
# Commands
```shell
kubectl get nodes
kubectl get namespaces
kubectl get pods # 查看当前default namespace下pods列表
kubectl get pod -o wide -A       # 常用查看命令
-o wide # 更多信息
-A # 全namespace
-w # 持续观察资源变化

# POD
kubectl describe pod my-pod      # 查看Pod详情
kubectl logs my-pod              # 查看容器日志
kubectl logs my-pod -c container-name   # 多容器时要指定查看

kubectl run pod_name -it --image=image_name
kubectl delete pod my-pod        # 删除Pod
kubectl exec -it my-pod -- bash  # 进入容器shell
kubectl attach my-pod -c container-name -it # 回到容器主进程，与docker不同，-it是必要的
```
# Deploy
```shell
# 方案一
kubectl apply -f nginx-pod.yml # 可持续部署

# 方案二
kubectl create -f nginx-pod.yml # 单次使用
kubectl replace -f nginx-pod.yml # 替换create的
```
## 单pod: YAML
```YAML
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx
```
## 多pod：YAML + ReplicaSet
确保刚好有4个label为`app: nginx`的pod在运行
```YAML
apiVersion: apps/v1 # 这个要看官方文档
kind: ReplicaSet
metadata: # 注意这是ReplicaSet的元数据，不是pod的
  name: nginx-replicaset
  labels:
    name: nginx-replica
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template: # 以下是pod的创建模板
    metadata:
      name: nginx-pod
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx
```
### 命令
```shell
kubectl get rs
kubectl scale --replicas=5 -f nginx-replicaset.yml
```
# 还不懂
kubectl scale deploy app --replicas=5  # 扩容
kubect get node       # 查找节点 NAME列为节点名称，有IP标记
kubectl drain kube-system-141.246.7.12 --force --ignore-daemonsets --delete-local-data # 驱逐节点
kubectl uncordon kube-system-141.246.7.12 # 恢复调度