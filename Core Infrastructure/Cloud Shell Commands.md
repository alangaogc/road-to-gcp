<h2>VM Creation</h2>
1. List all zones - 
gcloud compute zones list | grep us-central1
2. Set default zone for a new vm - 
gcloud config set compute/zone us-central1-c
3. Create a new vm -
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image-family "debian-10" \
--subnet "default"

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
kubectl run nginx --imagine=nginx:1.15.7
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