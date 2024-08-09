# EKS-Autoscaler-Pod-Node
aws eks auto scaler for Node &amp; pod


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
   


