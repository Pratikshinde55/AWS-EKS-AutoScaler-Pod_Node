# EKS-Autoscaler-Pod-Node
aws eks auto scaler for Node &amp; pod

- EKS master Node AutoScaling is EKS. 

- There are two Scaling in EKS: 
  1. Node AutoScaling (Cluster AutoScaler & karpenter)
  2. POD AutoScaling (Hpa)

 1. Node AutoScaling:
    It is "Cluster AutoScaler" means increases worker node, For this EKS use aws ASG service(Auto scaling group).

    There are two Autoscaling are available for Node that is "Cluster AutoScaler" & "karpenter".

    for Autoscaling metrics is important, EKS uses aws "Cloud Watch" for matrics the node.

    Matrics means the program keep on monitoring the CPU,RAM of Nodes. 

2. POD AutoScaling:

   For Pod AutoScaling Hpa(Horizontal Pod AutoScaling) is used.

   Kebernetes give metrics server for pod autoscaling, we need to download that metrics server which helps us for AutoScaling.
   


# AutoScaler for Node:

- This command is for create cluster with enable asg (--asg-access) : 

          eksctl create cluster  --name pscluster1  --region ap-south-1  --nodegroup-name ps-ng1  --node-type t2.micro  --nodes 3  --nodes-min 3 
         --nodes-max 7   --node-volume-size 8  --node-volume-type gp3  --ssh-access  --enable-ssm  --asg-access  --managed

![image](https://github.com/user-attachments/assets/f58bac64-f176-41fa-a615-163758d6362a)


- Now need to start asg using file :

           curl -O https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml


- open file "cluster-autoscaler-autodiscover.yaml" :

          notepad cluster-autoscaler-autodiscover.yaml

![image](https://github.com/user-attachments/assets/d4bf2eb5-3fa9-44b5-9d7e-01b36d0be2bb)

in this file we replace "<YOUR CLUSTER NAME >"  > pscluster1 my cluster name



- apply file command :

          kubectl apply -f cluster-autoscaler-autodiscover.yaml

- after aplly check cluster-autoscaler pod run:

           kubectl get pods -n kube-system
          

![image](https://github.com/user-attachments/assets/0c5bc16f-3902-47b1-8370-99ab1bce7de1)



