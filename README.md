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

This command is for create cluster with enable asg (--asg-access) : 

          eksctl create cluster  --name pscluster1  --region ap-south-1  --nodegroup-name ps-ng1  --node-type t2.micro  --nodes 3  --nodes-min 3 
         --nodes-max 7   --node-volume-size 8  --node-volume-type gp3  --ssh-access  --enable-ssm  --asg-access  --managed







