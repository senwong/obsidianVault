# K8S核心概念

## Pod
概念：K8S中最小计算单元，一个pod中可以包含多个容器
## 控制面/数据面
Control Plane，Data Plane， 控制面相当于master node，数据面相挡于data node。

## Api对象
k8s所有的资源管理都是面向对象的api，通过`kubectl api-resources`查看所有对象，所有对象接口的操作是类似RESTfull的风格去访问，比如`kubectl get pods`，执行此命令其实是http接口访问的api-server。所以被称为api对象。
![[kubectl-api-resoruces.png]]


## Deployment
### 概念
用来管理pod的方法，一个deployment对象就是k8s中的一次部署

### 常用命令
创建deployment：
```
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
```

查看deployment：
```
kubectl get deployments
```


## Service
Service 是 将运行在一个或一组 [Pod](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/) 上的网络应用程序公开为网络服务的方法。


# 扩容

## 组件和插件
K8S组件是集群运行必须的功能组件，插件是可有可无的优化功能。
![[k8s_get_comp.png]]

常见的Component：
etcd：持久化存储
api-server：和`kubectl`通信的服务。
controller-manager:
参考：[工作机制](https://learn.lianglianglee.com/%e4%b8%93%e6%a0%8f/Kubernetes%e5%85%a5%e9%97%a8%e5%ae%9e%e6%88%98%e8%af%be/10%20%e8%87%aa%e5%8a%a8%e5%8c%96%e7%9a%84%e8%bf%90%e7%bb%b4%e7%ae%a1%e7%90%86%ef%bc%9a%e6%8e%a2%e7%a9%b6Kubernetes%e5%b7%a5%e4%bd%9c%e6%9c%ba%e5%88%b6%e7%9a%84%e5%a5%a5%e7%a7%98.md)。

