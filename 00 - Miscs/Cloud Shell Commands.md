<h2>Cloud Shell</h1>
1. Overview -
https://cloud.google.com/shell
2. Reset Cloud Shell -
https://cloud.google.com/shell/docs/resetting-cloud-shell

<h2>GCP Project</h2>
1. List current project information -
gcloud config list
2. Switch to a specific project -
gcloud config set project $PROJECT_1_ID

<h2>VM Creation</h2>
1. List all zones - 
gcloud compute zones list | grep us-central1
2. List all available regions -
gcloud compute regions list
3. List all VMs -
gcloud compute instances list --sort-by=ZONE
4. Set default zone for a new vm - 
gcloud config set compute/zone us-central1-c
5. Create a new vm -
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image-family "debian-10" \
--subnet "default"
6. Create a new VM with network details -
gcloud compute instances create privatenet-us-vm --zone=us-central1-c --machine-type=f1-micro --subnet=privatesubnet-us --image-family=debian-10 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=privatenet-us-vm
7. ssh to VM vm-internal -
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap

<h2>VM Utilities</h2>
1. Install a new web service - 
sudo apt-get intall nginx-light -y

<h2>VM Storage</h2>
1. Create a storage bucket - 
gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
2. Copy file from target - 
gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png \
my-excellent-blog.png
3. Upload file to storage -
gsutil cp my-excellent-blog.png \
gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
4. List files in storage - 
gsutil ls gs://$DEVSHELL_PROJECT_ID
5. Modify ACL to readable by everyone -
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

<h2>Kubernetes Engine</h2>
1. Create a Kubernetes cluster (k1) - 
gcloud container clusters create k1 --zone $MY_ZONE --num-nodes 2
2. Check Kubernetes version -
kubectl version
3. Create a deployment - 
kubectl create deploy nginx --imagine=nginx:1.15.7
4. List all running pods - 
kubectl get pods
5. Make pods and deployments publicly accessible -
kubectl expose deployment nginx --port=80 --type=LoadBalancer
6. Show services public IP addresses -
kubectl get services
7. Scale up a deployment - 
kubectl scale deployment nginx --replicas 3
8. Auto scale a deployment (when CPU usage is 80% of capacity) -
kubectl autoscale nginx --min=10 --max=15 --cpu=80
9. Apply deployment file -
kubectl apply -f nginx-deployment.yaml
10. List all replicas -
kubectl get replicasets
11. List all deployments -
kubectl get deployments

<h2>App Engine</h2>
1. Initialise App Engine app - 
gcloud app create --project=$DEVSHELL_PROJECT_ID
2. Clone source code from github - 
git clone https://github.com/xxxxxxxxxxxxx
3. Run application locally in Cloud Shell - 
dev_appserver.py ./app.yaml
4. Deploy application so App Engine - 
gcloud app deploy ./index.yaml ./app.yaml
5. Build a container image to run a Python virtual environment - 
docker build -t test-python . 
6. Run the application -
docker run --rm -p 8080:8080 test-python 

<h2>Deployment Manager</h2>
1. Create a deployment manager template:
````
resources:
- name: my-vm
  type: compute.v1.instance
  properties:
  zone: us-central1-a
  machineType: zones/us-central1-a/machineTypes/n1-standard-1
  metadata:
  items:
  - key: startup-script
  value: "apt-get update"
  disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
      sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-9-stretch-v20201216
      networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-01-09a7853a3186/global/networks/default
      accessConfigs:
        - name: External NAT
          type: ONE_TO_ONE_NAT
````
2. Build a deployment from template - 
gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml
3. Update a deployment from template -
gcloud deployment-manager deployments update my-first-delp --config mydeploy.yaml
4. List deployments - 
gcloud deployment-manager deployments list

<h2>Stackdriver</h2>
1. Install monitoring agents -
curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh
sudo bash install-monitoring-agent.sh
2. Install logging agents -
curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh
sudo bash install-logging-agent.sh

<h2>Big Query</h2>
1. Big query select - 
bq query "select xxxxx from xxxx"

<h2>VPC Networking</h2>
1. List all available VPC networks -
gcloud compute networks list
2. List all available PVC subnetworks -
gcloud compute networks subnets list --sort-by=NETWORK
3. List all firewall rules -
gcloud compute firewall-rules list --sort-by=NETWORK
4. Create VPC network in custome mode named management -
gcloud compute networks create management --subnet-mode=custom
5. Create VPC subnet named management-us -
gcloud compute networks subnets create management-us --network=management --region=us-central1 --range=172.16.0.0/24
6. Create firewall rule -
   gcloud compute --project=qwiklabs-gcp-00-865c19139b4e firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=management --action=ALLOW --rules=tcp:22,tcp:3389,icmp --source-ranges=0.0.0.0/0