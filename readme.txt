# process to setup k8s with CentOS VM.
--------------------------------------------------------------
#1 copy kubernetes.repo to /etc/yum.repos.d/kubernetes.repo
#2 yum -y install epel-release
#3 yum clean all
#4 yum makecache
#5 yum -y install kubelet kubeadm kubectl kubernetes-cni
#6 systemctl enable kubelet

#7 download images by script k8s-images.sh
   before running it, chmod -R 777 ./k8s-images.sh
#8 sudo swapoff -a
   要永久禁掉swap分区，打开如下文件注释掉swap那一行 
   sudo vi /etc/fstab
#9 临时禁用selinux
   永久关闭 修改/etc/sysconfig/selinux文件设置
   sed -i 's/SELINUX=permissive/SELINUX=disabled/' /etc/sysconfig/selinux
   这里按回车，下面是第二条命令
   setenforce 0
#10 copy k8s.conf to /etc/sysctl.d/k8s.conf and run sysctl --system
---------------------------------------------------------------------

master:
kubeadm init --kubernetes-version=v1.11.0 --pod-network-cidr=10.244.0.0/16
mkdir -p /etc/cni/net.d/
create file /etc/cni/net.d/10-flannel.conf
mkdir /usr/share/oci-umount/oci-umount.d -p
mkdir /run/flannel/
create file /run/flannel/subnet.env
   FLANNEL_NETWORK=10.244.0.0/16
   FLANNEL_SUBNET=10.244.1.0/24
   FLANNEL_MTU=1450
   FLANNEL_IPMASQ=true
kubectl create -f ./flannel.yml
