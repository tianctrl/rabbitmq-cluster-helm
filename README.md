# RabbitMQ

[RabbitMQ](https://www.rabbitmq.com/) 是一个在AMQP基础上完成的, 可复用的企业消息系统.

[RabbitMQ](https://www.rabbitmq.com/) is an open source message broker software that implements the Advanced Message Queuing Protocol (AMQP).


## 介绍

该应用模板会使用 [Helm](https://helm.sh) 包管理工具在 [Kubernetes](http://kubernetes.io) 上启动一个 [RabbitMQ](https://github.com/bitnami/bitnami-docker-rabbitmq) 集群.

设计集群的目的

- 允许消费者和生产者在RabbitMQ节点崩溃的情况下继续运行
- 通过增加更多的节点来扩展消息通信的吞吐量


该集群默认启动3个节点, 1个master节点, 2个普通节点. 可根据需求扩容伸缩. 

第一个启动的节点默认成为master节点.

This cluster will create 1 master node, 2 slave nodes. The first one is master.

## 安装要求

- Kubernetes 1.4+ 支持 Beta APIs
- 集群内部满足供给相应PV

## 安装应用

请在对应的namespace下创建serviceAccount:

Create a serviceaccount in your namespace.

```bash
kubectl -n (你的namespace) create serviceaccount rabbitmq
kubectl create clusterrolebinding rabbitmq --clusterrole cluster-admin --serviceaccount=(你的namespace):rabbitmq
```


## 配置参数 Configuration

下面的表格列出了RabbitMQ可配置的参数和参数的默认值.

The table shows cluster default params.

|         Parameter          |                       Description                       |                         Default                          |
|----------------------------|---------------------------------------------------------|----------------------------------------------------------|
| `image.name`               | RabbitMQ 集群镜像名                                      | `bitnami/rabbitmq:{VERSION}`                             |
| `image.pullPolicy`         | 镜像拉取规则                                             | `Never`                                                  |
| `replicas`                 | RabbitMQ 节点副本数                               | `3`                                                      |
| `rabbitmqNodePort`         | RabbitMQ Node port                                      | `5672`                                                   |
| `rabbitmqManagerPort`      | RabbitMQ Manager port                                   | `15672`                                                  |
| `serviceAccount`           | serviceAccount(RBAC)                                    | `true`                                                   |
| `serviceAccountName`       | serviceAccount名称                                      | `rabbitmq`                                               |
| `namespace`                | Namespace(命名空间)                                               | `rabbitmq-cluster`                                       |
| `serviceType`              | 服务类型(NodePort, ClusterIP等)                  | `NodePort`                                               |
| `httpNodePort`             | Service NodePort端口号(http)                                            | `31672`                                                  |
| `amqpNodePort`             | Service NodePort端口号(amqp)                                           | `30672`                                                  |





> **Tip**: 查看集群状态可使用:

```
kubectl exec -n (你的namespace) (第一个pod的name) rabbitmqctl cluster_status
```


If you want to check the cluster status:

```
kubectl exec -n (your namespace) (the first pod's name) rabbitmqctl cluster_status
```

