# RabbitMQ

[RabbitMQ](https://www.rabbitmq.com/) 是一个在AMQP基础上完成的，可复用的企业消息系统。

## TL;DR;

```bash
$ helm install stable/rabbitmq-cluster
```

## 介绍

该应用模板会使用 [Helm](https://helm.sh) 包管理工具在 [Kubernetes](http://kubernetes.io) 上启动一个 [RabbitMQ](https://github.com/bitnami/bitnami-docker-rabbitmq) 集群.

## 安装要求

- Kubernetes 1.4+ 支持 Beta APIs
- 集群内部满足供给相应PV

## 安装应用

请在对应的namespace下创建serviceAccount:

```bash
kubectl -n (你的namespace) create serviceaccount rabbitmq
kubectl create clusterrolebinding rabbitmq --clusterrole cluster-admin --serviceaccount=(你的namespace):rabbitmq
```

通过应用名来安装应用 `my-release`:

```bash
$ helm install --name my-release stable/rabbitmq-cluster
```

该命令会部署一个默认配置的RabbitMQ集群.  [configuration](#configuration) 配置目录列出了所有安装期间可使用的的参数.

> **Tip**: 查看所有release请使用 helm list

## 卸载应用

卸载/删除 `my-release` :

```bash
$ helm delete my-release
```

该命令会移除所有与之相关的Kubernetes组件,并删除release.

## 配置参数

下面的表格列出了RabbitMQ可配置的参数和参数的默认值.

|         Parameter          |                       Description                       |                         Default                          |
|----------------------------|---------------------------------------------------------|----------------------------------------------------------|
| `image.name`               | RabbitMQ cluster image                                  | `bitnami/rabbitmq:{VERSION}`                             |
| `image.pullPolicy`         | Image pull policy                                       | `Never`                                                  |
| `replicas`                 | RabbitMQ application username                           | `3`                                                      |
| `rabbitmqNodePort`         | Node port                                               | `5672`                                                   |
| `rabbitmqManagerPort`      | RabbitMQ Manager port                                   | `15672`                                                  |
| `serviceAccount`           | serviceAccount                                          | `true`                                                   |
| `serviceAccountName`       | serviceAccountName                                      | `rabbitmq`                                               |
| `namespace`                | namespace                                               | `rabbitmq-cluster`                                       |
| `serviceType`              | serviceType                                             | `NodePort`                                               |
| `httpNodePort`             | httpNodePort                                            | `31672`                                                  |
| `amqpNodePort`             | amqpNodePort                                            | `30672`                                                  |


使用 `--set key=value[,key=value]` 指定各个参数, 在 `helm install` 时使用. 例如,

```bash
$ helm install --name my-release \
  --set namespace=rabbitmq,serviceAccountName=sarabbitmq \
    stable/rabbitmq
```

或者, 可以在安装阶段使用YAML文件来指定参数. 例如,

```bash
$ helm install --name my-release -f values.yaml stable/rabbitmq
```

> **Tip**: 你可以使用默认的 [values.yaml](values.yaml)

