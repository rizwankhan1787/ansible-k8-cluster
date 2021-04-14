# k8s-cluster-ansible
Configuring Kubernetes Cluster with the Help of Redhat Ansible Automation tool inside AWS Cloud


first execute below command to create 3 ec2 instances one master and 2 worker nodes 

ansible-playbook -i hosts ec2.yml

once instance gets created copy the public ip's and add it in hosts file with master and slave group and execute the below command to configure self-manage kubernetes cluster

ansible-playbook -i hosts cluster.yml -u root

To configure scheduling in aws create a new role as below and execute the playbook

---
  - name: Configure the schedule host.
    hosts: schedule
    gather_facts: yes
    sudo: yes

    roles:
      - role: aws-schedule
        schedule_name: us-east-2
        schedule_aws_region: us-east-2
        schedule_enforce: yes
        schedule_start_stop_schedules:
          - schedule_name: India-business-hours
            start_cron_expression: "0 8 * * 1,2,3,4,5"
            stop_cron_expression: "0 20 * * 1,2,3,4,5"
            timezone: Asia/Kolkata
          

once its configured execute below command to check if kubernetes master and worker nodes are configured properly or not

kubectl get nodes

install helm and add the nginx repo in kubernetes master and perform the deployment

git clone https://github.com/nginxinc/kubernetes-ingress/

$ cd kubernetes-ingress/deployments/helm-chart
$ git checkout v1.11.1

$ helm repo add nginx-stable https://helm.nginx.com/stable
$ helm repo update

helm install my-release nginx-stable/nginx-ingress

run below command to check if deployment is created

kubectl get deployments

