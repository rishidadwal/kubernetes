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



