# EKS-Autoscaler-Pod-Node
aws eks auto scaler for Node &amp; pod

- EKS master Node AutoScaling is fully managed by EKS. 

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

   Kubernetes give metrics server for pod autoscaling, we need to download that metrics server which helps us for AutoScaling.
   


# AutoScaler of Node:

- This command is for create cluster with enable asg (--asg-access) (AutoScaler group) : 

          eksctl create cluster  --name pscluster2  --region ap-south-1  --version 1.30  --nodegroup-name psnodegp --instance-types t2.micro --nodes 3
          --nodes-min 3  --nodes-max 6  --node-volume-size 8  --node-volume-type gp3  --ssh-access   --enable-ssm --instance-name psworkernode --asg-access
          --managed

![image](https://github.com/user-attachments/assets/f58bac64-f176-41fa-a615-163758d6362a)


- Now need to start asg using file : (This command download GitHub file named as "cluster-autoscaler-autodiscover.yaml")

           curl -O https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml


- open file "cluster-autoscaler-autodiscover.yaml" :

  The YAML file sets up permissions and deploys the Cluster Autoscaler pod, which automatically scales our Kubernetes cluster based on workload demands.

          notepad cluster-autoscaler-autodiscover.yaml


- Check right version of image as compare to kubernetes version while creating cluster:

![image](https://github.com/user-attachments/assets/7ae1c097-845d-49f1-bf24-3a5ae79afad7)

- Set Auto scale down time:

![image](https://github.com/user-attachments/assets/9996df76-19a1-4ca7-a30e-529712833a0e)

In this file we replace "<YOUR CLUSTER NAME >"  > "pscluster2" my cluster name



- apply file command :(Create auto scaler pod)

          kubectl apply -f cluster-autoscaler-autodiscover.yaml

- after aplly check cluster-autoscaler pod run:

           kubectl get pods -n kube-system
          

![image](https://github.com/user-attachments/assets/0c5bc16f-3902-47b1-8370-99ab1bce7de1)

Check lastest version or on which version our kubernetes work and use that image for autoscaler pod: (https://github.com/kubernetes/autoscaler/releases)

In my case i use version -->> --version 1.30  so i use image version ":v1.30.1"

command for deployment.apps/cluster-autoscaler image updated: (Optional commad/not needed)

           kubectl set image deployment cluster-autoscaler -n kube-system cluster-autoscaler=registry.k8s.io/autoscaling/cluster:v1.30.1
           
 - we can check nodes details info using command: 

           kubectl describe nodes
    
 - we can check specific nodes details info using command:

           kubectl describe nodes <nodeName>
       
# AutoScaling of POD:

The manual Horizontal scaling of pods we can done using command:

          kubectl scale deployment myps1  --replicas=5

- Note:

   There is no program running which give metrics of pods, without metrics server we can't use AutoScaling because of this Kubernetes provide "Metrics server".

   Metrics server keep on monitoring on all pods(CPU, RAM), If load come then auto horizontal scaling done by using Hpa.

  Metrics server is one kind of program running as pod.
  
  Hpa: Horizontal pod AutoScaling

- Install metrics server from YAML file -->> Search on browser 'kubernetes metric server' (Link: https://github.com/kubernetes-sigs/metrics-server)

           kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

  
