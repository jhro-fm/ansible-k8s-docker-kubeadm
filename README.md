# ansible-k8s-docker-kubeadm

#### 介绍
一键部署k8s，kubeadm和docker版本的k8s集群，换rancher或者containerd其实也大同小异

#### 环境准备
1.三台虚拟机或者云服务器，centos7，ip设置为静态   
比如我这边三台主机的ip:  
(每台主机都写入下面三行数据)  
    master1 192.168.23.221  
    node1 192.168.23.222  
    node2 192.168.23.223  
我这边的提供这三个ip的网卡名称是ens33，如果你不是这个网卡名，建议永久方式更改网卡名称，或者你也可以去修改flannel中的ens33为你的网卡名称，建议用ens33  

2.你需要设置好主机名，分别是master1，node1，node2。   注意master1我在配置文件中也是写这个，如果你用别的主机名要修改kubeadm.yaml中的主机名，建议你还是用master1  
master1主机：hostnamectl --static set-hostname master1  
node1主机：hostnamectl --static set-hostname node1  
node2主机：hostnamectl --static set-hostname node2  


#### ansible安装教程即资源清单准备(这在master1上操作即可)  

可以使用pip3或者yum安装，这里用yum  
yum install epel-release && yum repolist && yum install ansible  

vim /etc/ansible/hosts:  
(写下这些内容，ip换成你自己的)  
[k8sallnode]  
192.168.23.221  
192.168.23.222  
192.168.23.223  
[k8sworknode]  
192.168.23.222  
192.168.23.223  
[k8smaster]  
192.168.23.221  


#### 使用说明  
1.将整个项目克隆下来 git clone git@github.com:20gu00/ansible-k8s-docker-kubeadm.git  
注意，放在你的/root目录下面，即效果是/root/ansible-k8s-docker-kubeadm/ansible-k8s-docker-kubeadm.yml

2.将kubeadm.yaml,join-token两个文件中的master的ip地址改成自己的

3.修改kube-flannel.yml中网卡名改成自己的，默认是ens33

4.ansible-playbook ansible-k8s-docker-kubeadm.yml  


附加：集群搭建起来了，但如果你的网速不好的话，集群可能还不能短时间内就是用，比如kube-system的coredns和flannel容器还在下载镜像中，需要等待一会，或者你可以describe查看下载中的镜像，手动docker pull下来，还有就是如果running了但较长时间还是0/1，你可以删除该pod，它会自动重建，一般都可以解决  
