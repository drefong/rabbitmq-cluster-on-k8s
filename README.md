# rabbitmq-cluster-in-k8s

       rabbitmq Cluster在kubernetes中部署是基于rabbitmq的Cluster Formation and Peer Discovery机制，进行节点的自动发现，非常方便实现集群的横向扩展。实现Cluster Formation and Peer Discovery是通过启用rabbitmq-peer-discovery-k8s插件实现的。此插件适用于rabbitmq 3.7以上版本，3.6版本可以参考autocluster。本部署范例采用的官方的rabbitmq:3.7.4镜像。
       1、创建serviceAccount
        kubectl create -f  rbac.yml

       2、创建rabbitmq的配置文件
       配置文件包含rabbitmq.conf和enabled_plugins。其中rabbitmq.conf为主配置文件，enabled_plugins是rabbitmq启用的插件。
       其中rabbitmq.conf有部分参数用于peer discovery。

       cluster_formation.k8s.address_type = ip
       cluster_formation.node_cleanup.only_log_warning = false
       cluster_formation.node_cleanup.interval = 30
       cluster_partition_handling = autoheal
       loopback_users.guest = false

       3、创建statefulset资源
       4、创建service
      采用NodePort暴露web管理端口15672和业务端口5672，可以根据实际需求，采用ingress进行端口的暴露。
      
       5、集群扩展
       集群扩展通过kubectl scale扩展即可。
       kubectl scale statefulset rabbitmq --replicas=num