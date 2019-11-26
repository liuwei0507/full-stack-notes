# 一、基础入门

## 1、Kubernetes 简介

Kubernetes 是谷歌开源的容器集群管理系统，是 Google 多年大规模容器管理技术 Borg 的开源版本，主要功能包括：

- 基于容器的应用部署、维护和滚动升级
- 负载均衡和服务发现
- 跨机器和跨地区的集群调度
- 自动伸缩
- 无状态服务和有状态服务
- 广泛的 Volume 支持
- 插件机制保证扩展性

Kubernetes 发展非常迅速，已经成为容器编排领域的领导者。

### 1.1 Kubernetes 是一个平台

​		Kubernetes 提供了很多的功能，它可以简化应用程序的工作流，加快开发速度。通常，一个成功的应用编排系统需要有较强的自动化能力，这也是为什么 Kubernetes 被设计作为构建组件和工具的生态系统平台，以便更轻松地部署、扩展和管理应用程序。

​		用户可以使用 Label 以自己的方式组织管理资源，还可以使用 Annotation 来自定义资源的描述信息，比如为管理工具提供状态检查等。

​		此外，Kubernetes 控制器也是构建在跟开发人员和用户使用的相同的 API 之上。用户还可以编写自己的控制器和调度器，也可以通过各种插件机制扩展系统的功能。

这种设计使得可以方便地在 Kubernetes 之上构建各种应用系统。

### 1.2 Kubernetes 不是什么

Kubernetes 不是一个传统意义上，包罗万象的 PaaS (平台即服务) 系统。它给用户预留了选择的自由。

- 不限制支持的应用程序类型，它不插手应用程序框架, 也不限制支持的语言 (如 Java, Python, Ruby 等)，只要应用符合 [12 因素](http://12factor.net/) 即可。Kubernetes 旨在支持极其多样化的工作负载，包括无状态、有状态和数据处理工作负载。只要应用可以在容器中运行，那么它就可以很好的在 Kubernetes 上运行。
- 不提供内置的中间件 (如消息中间件)、数据处理框架 (如 Spark)、数据库 (如 mysql) 或集群存储系统 (如 Ceph) 等。这些应用直接运行在 Kubernetes 之上。
- 不提供点击即部署的服务市场。
- 不直接部署代码，也不会构建您的应用程序，但您可以在 Kubernetes 之上构建需要的持续集成 (CI) 工作流。
- 允许用户选择自己的日志、监控和告警系统。
- 不提供应用程序配置语言或系统 (如 [jsonnet](https://github.com/google/jsonnet))。
- 不提供机器配置、维护、管理或自愈系统。

另外，已经有很多 PaaS 系统运行在 Kubernetes 之上，如 [Openshift](https://github.com/openshift/origin), [Deis](http://deis.io/) 和 [Eldarion](http://eldarion.cloud/) 等。 您也可以构建自己的 PaaS 系统，或者只使用 Kubernetes 管理您的容器应用。

当然了，Kubernetes 不仅仅是一个 “编排系统”，它消除了编排的需要。Kubernetes 通过声明式的 API 和一系列独立、可组合的控制器保证了应用总是在期望的状态，而用户并不需要关心中间状态是如何转换的。这使得整个系统更容易使用，而且更强大、更可靠、更具弹性和可扩展性。

### 1.3 核心组件

Kubernetes 主要由以下几个核心组件组成：

- etcd 保存了整个集群的状态；
- apiserver 提供了资源操作的唯一入口，并提供认证、授权、访问控制、API 注册和发现等机制；
- controller manager 负责维护集群的状态，比如故障检测、自动扩展、滚动更新等；
- scheduler 负责资源的调度，按照预定的调度策略将 Pod 调度到相应的机器上；
- kubelet 负责维护容器的生命周期，同时也负责 Volume（CVI）和网络（CNI）的管理；
- Container runtime 负责镜像管理以及 Pod 和容器的真正运行（CRI）；
- kube-proxy 负责为 Service 提供 cluster 内部的服务发现和负载均衡

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LDAOok5ngY4pc1lEDes%2F-LpOIkR-zouVcB8QsFj_%2F-LpOIpZIYxaDoF-FJMZk%2Farchitecture.png?generation=1569161437087842&alt=media)



## 2、Kubernetes 基本概念

### 2.1 Container

Container（容器）是一种便携式、轻量级的操作系统级虚拟化技术。它使用 namespace 隔离不同的软件运行环境，并通过镜像自包含软件的运行环境，从而使得容器可以很方便的在任何地方运行。

由于容器体积小且启动快，因此可以在每个容器镜像中打包一个应用程序。这种一对一的应用镜像关系拥有很多好处。使用容器，不需要与外部的基础架构环境绑定, 因为每一个应用程序都不需要外部依赖，更不需要与外部的基础架构环境依赖。完美解决了从开发到生产环境的一致性问题。

容器同样比虚拟机更加透明，这有助于监测和管理。尤其是容器进程的生命周期由基础设施管理，而不是被进程管理器隐藏在容器内部。最后，每个应用程序用容器封装，管理容器部署就等同于管理应用程序部署。

其他容器的优点还包括

- 敏捷的应用程序创建和部署: 与虚拟机镜像相比，容器镜像更易用、更高效。
- 持续开发、集成和部署: 提供可靠与频繁的容器镜像构建、部署和快速简便的回滚（镜像是不可变的）。
- 开发与运维的关注分离: 在构建/发布时即创建容器镜像，从而将应用与基础架构分离。
- 开发、测试与生产环境的一致性: 在笔记本电脑上运行和云中一样。
- 可观测：不仅显示操作系统的信息和度量，还显示应用自身的信息和度量。
- 云和操作系统的分发可移植性: 可运行在 Ubuntu, RHEL, CoreOS, 物理机, GKE 以及其他任何地方。
- 以应用为中心的管理: 从传统的硬件上部署操作系统提升到操作系统中部署应用程序。
- 松耦合、分布式、弹性伸缩、微服务: 应用程序被分成更小，更独立的模块，并可以动态管理和部署 - 而不是运行在专用设备上的大型单体程序。
- 资源隔离：可预测的应用程序性能。
- 资源利用：高效率和高密度。

### 2.2 Pod

Kubernetes 使用 Pod 来管理容器，每个 Pod 可以包含一个或多个紧密关联的容器。

Pod 是一组紧密关联的容器集合，它们共享 PID、IPC、Network 和 UTS namespace，是 Kubernetes 调度的基本单位。Pod 内的多个容器共享网络和文件系统，可以通过进程间通信和文件共享这种简单高效的方式组合完成服务。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LDAOok5ngY4pc1lEDes%2F-LpOIkR-zouVcB8QsFj_%2F-LpOIpZEWjsArXqZpSuN%2Fpod.png?generation=1569161437022859&alt=media)

pod

在 Kubernetes 中，所有对象都使用 manifest（yaml 或 json）来定义，比如一个简单的 nginx 服务可以定义为 nginx.yaml，它包含一个镜像为 nginx 的容器：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```

### 2.3 Node

Node 是 Pod 真正运行的主机，可以是物理机，也可以是虚拟机。为了管理 Pod，每个 Node 节点上至少要运行 container runtime（比如 docker 或者 rkt）、`kubelet` 和 `kube-proxy` 服务。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LDAOok5ngY4pc1lEDes%2F-LpOIkR-zouVcB8QsFj_%2F-LpOIpZNK7_D9lT7C57d%2Fnode.png?generation=1569161441558542&alt=media)

​                                                                           node

### 2.4 Namespace

Namespace 是对一组资源和对象的抽象集合，比如可以用来将系统内部的对象划分为不同的项目组或用户组。常见的 pods, services, replication controllers 和 deployments 等都是属于某一个 namespace 的（默认是 default），而 node, persistentVolumes 等则不属于任何 namespace。

### 2.5 Service

Service 是应用服务的抽象，通过 labels 为应用提供负载均衡和服务发现。匹配 labels 的 Pod IP 和端口列表组成 endpoints，由 kube-proxy 负责将服务 IP 负载均衡到这些 endpoints 上。

每个 Service 都会自动分配一个 cluster IP（仅在集群内部可访问的虚拟地址）和 DNS 名，其他容器可以通过该地址或 DNS 来访问服务，而不需要了解后端容器的运行。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LDAOok5ngY4pc1lEDes%2F-LpOIkR-zouVcB8QsFj_%2F-LpOIpZQ8P49qNDyiHUJ%2F14731220608865.png?generation=1569161437146749&alt=media)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  ports:
  - port: 8078 # the port that this service should serve on
    name: http
    # the container on each pod to connect to, can be a name
    # (e.g. 'www') or a number (e.g. 80)
    targetPort: 80
    protocol: TCP
  selector:
    app: nginx
```

### 2.6 Label

Label 是识别 Kubernetes 对象的标签，以 key/value 的方式附加到对象上（key 最长不能超过 63 字节，value 可以为空，也可以是不超过 253 字节的字符串）。

Label 不提供唯一性，并且实际上经常是很多对象（如 Pods）都使用相同的 label 来标志具体的应用。

Label 定义好后其他对象可以使用 Label Selector 来选择一组相同 label 的对象（比如 ReplicaSet 和 Service 用 label 来选择一组 Pod）。Label Selector 支持以下几种方式：

- 等式，如 `app=nginx` 和 `env!=production`
- 集合，如 `env in (production, qa)`
- 多个 label（它们之间是 AND 关系），如 `app=nginx,env=test`

### 2.7 Annotations

Annotations 是 key/value 形式附加于对象的注解。不同于 Labels 用于标志和选择对象，Annotations 则是用来记录一些附加信息，用来辅助应用部署、安全策略以及调度策略等。比如 deployment 使用 annotations 来记录 rolling update 的状态。

## 3、Kubernetes 101

体验 Kubernetes 最简单的方法是跑一个 nginx 容器，然后使用 kubectl 操作该容器。Kubernetes 提供了一个类似于 `docker run` 的命令 `kubectl run`，可以方便的创建一个容器（实际上创建的是一个由 deployment 来管理的 Pod）：

```
$ kubectl run --image=nginx:alpine nginx-app --port=80
deployment "nginx-app" created
$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
nginx-app-4028413181-cnt1i   1/1       Running   0          52s
```

等到容器变成 Running 后，就可以用 `kubectl` 命令来操作它了，比如

- `kubectl get` - 类似于 `docker ps`，查询资源列表
- `kubectl describe` - 类似于 `docker inspect`，获取资源的详细信息
- `kubectl logs` - 类似于 `docker logs`，获取容器的日志
- `kubectl exec` - 类似于 `docker exec`，在容器内执行一个命令

```
$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
nginx-app-4028413181-cnt1i   1/1       Running   0          6m

$ kubectl exec nginx-app-4028413181-cnt1i ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.5  31736  5108 ?        Ss   00:19   0:00 nginx: master process nginx -g daemon off;
nginx        5  0.0  0.2  32124  2844 ?        S    00:19   0:00 nginx: worker process
root        18  0.0  0.2  17500  2112 ?        Rs   00:25   0:00 ps aux

$ kubectl describe pod nginx-app-4028413181-cnt1i
Name:          nginx-app-4028413181-cnt1i
Namespace:         default
Node:          boot2docker/192.168.64.12
Start Time:        Tue, 06 Sep 2016 08:18:41 +0800
Labels:        pod-template-hash=4028413181
               run=nginx-app
Status:        Running
IP:            172.17.0.3
Controllers:       ReplicaSet/nginx-app-4028413181
Containers:
  nginx-app:
    Container ID:              docker://4ef989b57d0a7638ad9c5bbc22e16d5ea5b459281c77074fc982eba50973107f
    Image:                 nginx
    Image ID:              docker://sha256:4efb2fcdb1ab05fb03c9435234343c1cc65289eeb016be86193e88d3a5d84f6b
    Port:                  80/TCP
    State:                 Running
      Started:             Tue, 06 Sep 2016 08:19:30 +0800
    Ready:                 True
    Restart Count:             0
    Environment Variables:         <none>
Conditions:
  Type         Status
  Initialized      True
  Ready            True
  PodScheduled     True
Volumes:
  default-token-9o8ks:
    Type:          Secret (a volume populated by a Secret)
    SecretName:    default-token-9o8ks
QoS Tier:          BestEffort
Events:
  FirstSeen        LastSeen           Count      From               SubobjectPath              Type           Reason         Message
  ---------        --------           -----      ----               -------------              --------           ------         -------
  8m           8m             1          {default-scheduler}                       Normal         Scheduled          Successfully assigned nginx-app-4028413181-cnt1i to boot2docker
  8m           8m             1          {kubelet boot2docker}      spec.containers{nginx-app}         Normal         Pulling        pulling image "nginx"
  7m           7m             1          {kubelet boot2docker}      spec.containers{nginx-app}         Normal         Pulled         Successfully pulled image "nginx"
  7m           7m             1          {kubelet boot2docker}      spec.containers{nginx-app}         Normal         Created        Created container with docker id 4ef989b57d0a
  7m           7m             1          {kubelet boot2docker}      spec.containers{nginx-app}         Normal         Started        Started container with docker id 4ef989b57d0a

$ curl http://172.17.0.3
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>
<p><em>Thank you for using nginx.</em></p>
</body>
</html>

$ kubectl logs nginx-app-4028413181-cnt1i
127.0.0.1 - - [06/Sep/2016:00:27:13 +0000] "GET / HTTP/1.0" 200 612 "-" "-" "-"
```



### 3.1 使用 yaml 定义 Pod

上面是通过 `kubectl run` 来启动了第一个 Pod，但是 `kubectl run` 并不支持所有的功能。在 Kubernetes 中，更经常使用 yaml 文件来定义资源，并通过 `kubectl create -f file.yaml` 来创建资源。比如，一个简单的 nginx Pod 可以定义为：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```

前面提到，`kubectl run` 并不是直接创建一个 Pod，而是先创建一个 Deployment 资源（replicas=1），再由与 Deployment 关联的 ReplicaSet 来自动创建 Pod，这等价于这样一个配置：

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    run: nginx-app
  name: nginx-app
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      run: nginx-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        run: nginx-app
    spec:
      containers:
      - image: nginx
        name: nginx-app
        ports:
        - containerPort: 80
          protocol: TCP
      dnsPolicy: ClusterFirst
      restartPolicy: Always
```



### 3.2 使用 Volume

Pod 的生命周期通常比较短，只要出现了异常，就会创建一个新的 Pod 来代替它。那容器产生的数据呢？容器内的数据会随着 Pod 消亡而自动消失。Volume 就是为了持久化容器数据而生，比如可以为 redis 容器指定一个 hostPath 来存储 redis 数据：

```
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - name: redis
    image: redis
    volumeMounts:
    - name: redis-persistent-storage
      mountPath: /data/redis
  volumes:
  - name: redis-persistent-storage
    hostPath:
      path: /data/
```

Kubernetes volume 支持非常多的插件，可以根据实际需要来选择：

- emptyDir
- hostPath
- gcePersistentDisk
- awsElasticBlockStore
- nfs
- iscsi
- flocker
- glusterfs
- rbd
- cephfs
- gitRepo
- secret
- persistentVolumeClaim
- downwardAPI
- azureFileVolume
- vsphereVolume

### 3.3 使用 Service

前面虽然创建了 Pod，但是在 kubernetes 中，Pod 的 IP 地址会随着 Pod 的重启而变化，并不建议直接拿 Pod 的 IP 来交互。那如何来访问这些 Pod 提供的服务呢？使用 Service。Service 为一组 Pod（通过 labels 来选择）提供一个统一的入口，并为它们提供负载均衡和自动服务发现。比如，可以为前面的 `nginx-app` 创建一个 service：

```
$ kubectl expose deployment nginx-app --port=80 --target-port=80 --type=NodePort
service "nginx-app" exposed
$ kubectl describe service nginx-app
Name:              nginx-app
Namespace:             default
Labels:            run=nginx-app
Selector:              run=nginx-app
Type:              ClusterIP
IP:                10.0.0.66
Port:              <unset>    80/TCP
NodePort:              <unset>    30772/TCP
Endpoints:             172.17.0.3:80
Session Affinity:          None
No events.
```

这样，在 cluster 内部就可以通过 `http://10.0.0.66` 和 `http://node-ip:30772` 来访问 nginx-app。而在 cluster 外面，则只能通过 `http://node-ip:30772` 来访问。



## 4、Kubernetes 201