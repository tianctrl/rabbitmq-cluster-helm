# RabbitMQ

[RabbitMQ](https://www.rabbitmq.com/) 是一个在AMQP基础上完成的, 可复用的企业消息系统.


## 介绍

该应用模板会使用 [Helm](https://helm.sh) 包管理工具在 [Kubernetes](http://kubernetes.io) 上启动一个 [RabbitMQ](https://github.com/bitnami/bitnami-docker-rabbitmq) 集群.

设计集群的目的

- 允许消费者和生产者在RabbitMQ节点崩溃的情况下继续运行
- 通过增加更多的节点来扩展消息通信的吞吐量


该集群默认启动3个节点, 1个master节点, 2个普通节点. 可根据需求扩容伸缩. 

第一个启动的节点默认成为master节点.

## 安装要求

- Kubernetes 1.4+ 支持 Beta APIs
- 集群内部满足供给相应PV

## 安装应用

<!--请在对应的namespace下创建serviceAccount:  -->

<!-- ```bash
kubectl -n (你的namespace) create serviceaccount rabbitmq
kubectl create clusterrolebinding rabbitmq --clusterrole cluster-admin --serviceaccount=(你的namespace):rabbitmq
``` -->


## 配置参数

下面的表格列出了RabbitMQ可配置的参数和参数的默认值.

|         Parameter          |                       Description                       |                         Default                          |
|----------------------------|---------------------------------------------------------|----------------------------------------------------------|
| `image.name`               | RabbitMQ 集群镜像名                                      | `bitnami/rabbitmq:{VERSION}`                             |
| `image.pullPolicy`         | 镜像拉取规则                                             | `IfNotPresent`                                                  |
| `replicas`                 | RabbitMQ 节点副本数                               | `3`                                                      |
| `rabbitmqNodePort`         | RabbitMQ Node port                                      | `5672`                                                   |
| `rabbitmqManagerPort`      | RabbitMQ Manager port                                   | `15672`                                                  |
| `serviceAccount`           | serviceAccount(RBAC)                                    | `true`                                                   |
| `serviceAccountName`       | serviceAccount名称                                      | `rabbitmq`                                               |
| `serviceType`              | 服务类型(NodePort, ClusterIP等)                  | `NodePort`                                               |
| `httpNodePort`             | Service NodePort端口号(http)                                            | `31672`                                                  |
| `amqpNodePort`             | Service NodePort端口号(amqp)                                           | `30672`                                                  |
| `storage.enabled`             | 是否开启外部存储                                 | `false`         |
| `metrics.enabled`             | 是否开启标量支持                                 | `false`         |



如需修改上面参数,请参照( 以下为默认参数 ):

```
{
    "name": "rabbitmq-cluster-example",
    "namespace": "rabbitmq-cluster",
    "repo": "dcos",
    "chart": "rabbitmq-cluster",
    "version": "latest",
    "values": {
        "image": {
          "name": "neunnsy/rabbitmq-cluster",
          "pullPolicy": "IfNotPresent"
        },
        "replicas": 3,
        "rabbitmqNodePort": 5672,
        "rabbitmqManagerPort": 15672,
        "serviceAccount": true,
        "serviceAccountName": "rabbitmq",
        "serviceType": "NodePort",
        "httpNodePort": 31672,
        "amqpNodePort": 30672,
        "exporterNodePort": 32672,
        "storage": {
            "enabled": false
        },
        "metrics": {
            "enabled": false
        }
    }  
}

```



> **Tip**: 查看集群状态可使用:

```
kubectl exec -n (你的namespace) (第一个pod的name) rabbitmqctl cluster_status
```


