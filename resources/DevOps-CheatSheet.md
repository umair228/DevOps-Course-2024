# DevOps Cheat Sheet

## Introduction
This cheat sheet provides quick references and essential commands for **DevOps tools** and practices, helping you streamline development, deployment, and infrastructure management workflows.

---

## Git Commands


  git clone <repository-url>

## Check Status:  
git status
Add Changes:  
git add <file-name>
## Commit Changes:  
git commit -m "commit message"
## Push Changes:  
git push origin <branch-name>
## Pull Changes:  
git pull origin <branch-name>


## Docker Commands
### Build an Image:  
docker build -t <image-name> .
### Run a Container:  
docker run -d --name <container-name> <image-name>
### List Running Containers:  
docker ps
### Stop a Container:  
docker stop <container-name>
### Remove a Container:  
docker rm <container-name>


## Kubernetes Commands
### Get Cluster Info:  
kubectl cluster-info
### Get Nodes:  
kubectl get nodes
### Get Pods:  
kubectl get pods
### Describe Pod:  
kubectl describe pod <pod-name>
### Apply Configuration:  
kubectl apply -f <config-file>.yaml
### Delete Resource:  
kubectl delete -f <config-file>.yaml

## CI/CD Commands (Jenkins)
### Start Jenkins:  
sudo systemctl start jenkins
### Stop Jenkins:  
sudo systemctl stop jenkins
### Check Jenkins Status:  
sudo systemctl status jenkins
### Restart Jenkins:  
sudo systemctl restart jenkins


## Ansible Commands
### Run Playbook:  
ansible-playbook <playbook-name>.yaml
### Check Syntax:  
ansible-playbook <playbook-name>.yaml --syntax-check
### List Hosts:  
ansible all --list-hosts
### Ping Hosts:  
ansible all -m ping


## Terraform Commands
### Initialize Terraform:  
terraform init
### Plan Infrastructure:  
terraform plan
### Apply Configuration:  
terraform apply
### Destroy Infrastructure:  
terraform destroy


## Monitoring Tools
### Prometheus:  
#### Start Prometheus:
prometheus --config.file=<config-file>.yml
### Grafana:  
#### Start Grafana:  
sudo systemctl start grafana-server
#### Stop Grafana:  
sudo systemctl stop grafana-server
#### Restart Grafana:  
sudo systemctl restart grafana-server