# kubernetes


 BUCKER NAME : dev.k8s.airtel.in

 export KOPS_STATE_STORE=s3://dev.k8s.airtel.in
 
 export KOPS_CLUSTER_NAME=airtel.in
 
 
 kops create cluster --state=${KOPS_STATE_STORE} --node-count=2 --master-size=t2.micro --node-size=t2.micro --zones=us-east-1d --name=${KOPS_CLUSTER_NAME}  --dns-zone=airtel.in --dns private --master-count 1

kops create cluster --cloud=aws --zones=ap-southeast-1b --name=dev.k8s.valaxy.in --dns-zone=valaxy.in --dns private
	

kops edit ig nodes ; kops update cluster --yes ;kops rolling-update cluster 


kops delete cluster  --yes

###########commands############
[root@ip-172-31-37-129 ~]# kubectl get nodes
NAME                            STATUS   ROLES    AGE     VERSION
ip-172-20-44-33.ec2.internal    Ready    node     8m21s   v1.14.6
ip-172-20-52-245.ec2.internal   Ready    master   10m     v1.14.6
##################################
[root@ip-172-31-37-129 kubernetes]# kubectl get nodes
NAME                            STATUS   ROLES    AGE   VERSION
ip-172-20-44-33.ec2.internal    Ready    node     48m   v1.14.6
ip-172-20-52-245.ec2.internal   Ready    master   50m   v1.14.6
[root@ip-172-31-37-129 kubernetes]#
[root@ip-172-31-37-129 kubernetes]#
[root@ip-172-31-37-129 kubernetes]# kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
01-sample   1/1     Running   0          104s
[root@ip-172-31-37-129 kubernetes]#
[root@ip-172-31-37-129 kubernetes]#
[root@ip-172-31-37-129 kubernetes]# svc
-bash: svc: command not found
[root@ip-172-31-37-129 kubernetes]# kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   100.64.0.1   <none>        443/TCP   50m
[root@ip-172-31-37-129 kubernetes]#
[root@ip-172-31-37-129 kubernetes]# kubectl get svc -o wide
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE   SELECTOR
kubernetes   ClusterIP   100.64.0.1   <none>        443/TCP   50m   <none>
[root@ip-172-31-37-129 kubernetes]#
[root@ip-172-31-37-129 kubernetes]#
[root@ip-172-31-37-129 kubernetes]# kubectl exec -it 01-sample /bin/bash


#######################################################
[root@ip-172-31-37-129 kubernetes]# kubectl describe pod  01-sample
Name:               01-sample
Namespace:          default
Priority:           0
PriorityClassName:  <none>
Node:               <none>
Labels:             app=samtos
Annotations:        kubectl.kubernetes.io/last-applied-configuration:
                      {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"samtos"},"name":"01-sample","namespace":"default"},"spec":{"...
Status:             Pending
IP:
Containers:
  sample:
    Image:      centos
    Port:       <none>
    Host Port:  <none>
    Command:
      sh
      -c
      echo Hello Kubernetes! && sleep 3600
    Limits:
      cpu:     300m
      memory:  128Mi
    Requests:
      cpu:        250m
      memory:     64Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-9b9dl (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  default-token-9b9dl:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-9b9dl
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age                 From               Message
  ----     ------            ----                ----               -------
  Warning  FailedScheduling  7s (x7 over 7m32s)  default-scheduler  0/2 nodes are available: 2 Insufficient cpu.
[root@ip-172-31-37-129 kubernetes]#

#################################################################################################################

kubectl get rs

kubectl edit rs

kubectl scale --replicas=2 rs/httpd-rs

###############################START AND STOP KOPS#######################
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# export KOPS_STATE_STORE=s3://dev.k8s.airtel.in
[root@ip-172-31-37-129 ~]# export KOPS_CLUSTER_NAME=airtel.in
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# kops edit ig nodes
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# kops edit ig master

error reading InstanceGroup "master": InstanceGroup.kops "master" not found
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# kops update cluster --yes
I1013 15:02:42.844164    1135 dns.go:94] Private DNS: skipping DNS validation
I1013 15:02:43.195125    1135 executor.go:103] Tasks: 0 done / 85 total; 42 can run
I1013 15:02:43.609269    1135 executor.go:103] Tasks: 42 done / 85 total; 23 can run
I1013 15:02:43.782726    1135 executor.go:103] Tasks: 65 done / 85 total; 18 can run
I1013 15:02:44.166694    1135 executor.go:103] Tasks: 83 done / 85 total; 2 can run
I1013 15:02:44.352686    1135 executor.go:103] Tasks: 85 done / 85 total; 0 can run
I1013 15:02:44.352723    1135 dns.go:155] Pre-creating DNS records

I1013 15:02:44.613986    1135 update_cluster.go:294] Exporting kubecfg for cluster
W1013 15:02:44.670245    1135 create_kubecfg.go:76] Did not find API endpoint for gossip hostname; may not be able to reach cluster
kops has set your kubectl context to airtel.in

Cluster changes have been applied to the cloud.


Changes may require instances to restart: kops rolling-update cluster

[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# kops rolling-update cluster
NAME                    STATUS  NEEDUPDATE      READY   MIN     MAX     NODES
master-us-east-1d       Ready   0               1       1       1       1
nodes                   Ready   0               1       0       0       1

No rolling-update required.
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# kops edit ig master-us-east-1d
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# kops update cluster --yes
I1013 15:03:24.764388    1159 dns.go:94] Private DNS: skipping DNS validation
I1013 15:03:25.116254    1159 executor.go:103] Tasks: 0 done / 85 total; 42 can run
I1013 15:03:25.513662    1159 executor.go:103] Tasks: 42 done / 85 total; 23 can run
I1013 15:03:25.690566    1159 executor.go:103] Tasks: 65 done / 85 total; 18 can run
I1013 15:03:26.072358    1159 executor.go:103] Tasks: 83 done / 85 total; 2 can run
I1013 15:03:26.265983    1159 executor.go:103] Tasks: 85 done / 85 total; 0 can run
I1013 15:03:26.266018    1159 dns.go:155] Pre-creating DNS records
I1013 15:03:26.381794    1159 update_cluster.go:294] Exporting kubecfg for cluster
W1013 15:03:26.442406    1159 create_kubecfg.go:76] Did not find API endpoint for gossip hostname; may not be able to reach cluster
kops has set your kubectl context to airtel.in

Cluster changes have been applied to the cloud.


Changes may require instances to restart: kops rolling-update cluster

[root@ip-172-31-37-129 ~]#
[root@ip-172-31-37-129 ~]# kops rolling-update cluster
W1013 15:03:31.808035    1169 aws_cloud.go:666] ignoring instance  as it is terminating: i-0aa237abe37ca338b in autoscaling group: nodes.airtel.in
NAME                    STATUS  NEEDUPDATE      READY   MIN     MAX     NODES
master-us-east-1d       Ready   0               1       0       0       1
nodes                   Ready   0               0       0       0       0

No rolling-update required.


