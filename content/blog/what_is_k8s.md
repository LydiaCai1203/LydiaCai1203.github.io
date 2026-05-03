+++
title = "what is k8s?"
description = "组长教学系列"
tags = [
    "2019",
    "技术",
]
date = "2019-05-04"
categories = [
    "技术",
]
type = "blog"
+++

### part_one: 基本结构介绍
![](/images/k8s_structure.png)
&emsp;&emsp;Node可以理解为物理资源，在K8s里面可操作的最小单元就是Pod，我们可以通过配置Pod配置文件的template字段来控制一个Pod里面有哪几种Container，但是不能控制一个Pod里面有几种container。Pod配置里面的template说明了要启动的容器是怎么样的，最重要的就是image的信息。  

&emsp;&emsp;后来由于需要部署的服务越来越多，每个Pod都是人工手动启动或者是删除检查的。所以后来产生了ReplicaController，有了这个东西以后就可以指定每个服务所要启动的Pods。这里的selector就是现在RC管理的下，还有哪些Pod是存活的。然后template就是指的是要启动的Pod是什么样子的。  

&emsp;&emsp;但是后来又遇到了滚动升级的问题(滚动升级：比如现在一个服务要从v1升级到v2，当时人工的处理方法就是将v1的Replica:3->Replica:2, 然后将v2的配置文件里面配上Replica:1，后续的动作依次类推)。这样也耗费了大量的人力。  

&emsp;&emsp;所以这时候Deployment就出现了，但是由于ReplicaController还有一些涉及的不好的地方，所以为了更好地与Deployment进行配合，于是就出现了ReplicaSet。  

&emsp;&emsp;**所以Deployment就是控制ReplicaSet的，Deployment里面可以看见RS和Pod两个东西的内容。**

&emsp;&emsp;Pod在K8s里面，这是一个不稳定的东西，每次死掉以后再挂起来，Pod的IP都会发生变化。那怎么去准确地找到哪一个Pod呢？(这个和endpoint也有一点关系)但是一个服务起了多个Pod，这时候如果有流量过来应该分配给哪个Pod呢？于是就出现了Service，这个Service起到了一个所谓的负载均衡的作用。每一个Service在这个集群里都有唯一确定的IP，这样就可以找到一群Pods，然后每个Pod都由endpoint进行标识。  

&emsp;&emsp;但是Service还是在这个虚拟网络里面的，外面用户来的流量应该怎么对应到指定的Service上面呢。之前说到外网想要连到百度云的服务器所在的私网的时候，使用的是VPN。但是我们不可能让用户都装上一个VPN Client。都进行验证。都建立一个虚拟专用网络。所以就出现了IngressController。这个东西可以简单地理解为是Nginx，可以实现反向代理。  

&emsp;&emsp;外部的流量会直接访问域名通过ip查询会命中Nginx中配置当中的public ip，这时候会把流量导向Service。但是这个对应的实现就是，Ingress实际上就是一种映射机制，定义了映射规则。在IngressController里面存在这一份vhost,这个里面存在service_ip和port的映射。这样就可以实现外部网络访问内部对应的pod了。