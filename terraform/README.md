# Terraform

## Init GKE cluster 

This procedure will provision a lightweight gke cluster for kubernetes esgischool.

### Setup tools
 
 !! Faites une sauvegarde du fichier ~/.kube/config !!
[Install kubctl](https://kubernetes.io/docs/tasks/tools/)

[Install gcloud CLI](https://cloud.google.com/sdk/docs/install?hl=fr)
[Install gke-gcloud-auth-plugin ](https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke)

Install Terraform

```bash
brew install terraform
```

### Setup configuration

Go in "terraform/gke" directory

```bash
cd terraform/gke
```

Setup environment variable : 
* "TF_VAR_esgischool_k8s_gcp_projectid" with projectId where create GKE cluster

```bash
export TF_VAR_esgischool_k8s_gcp_projectid=id de votre projet sur gcp
echo "export TF_VAR_esgischool_k8s_gcp_projectid=id de votre projet sur gcp" >> ~/.zshrc
```

Authenticate with gcloud command and setup project Id

```bash
gcloud auth login
gcloud config set project $TF_VAR_esgischool_k8s_gcp_projectid
```

Setup service account for terraform in GCP project with owner rights

```bash
gcloud iam service-accounts create k8s-terraform-sa
```

Copy key file of service account in according directory

```bash
mkdir ~/.gcp/
gcloud iam service-accounts keys create ~/.gcp/terraform-esgischool-k8s.json --iam-account=k8s-terraform-sa@${TF_VAR_esgischool_k8s_gcp_projectid}.iam.gserviceaccount.com
```

##### Adding the Terraform service account as project owner. 
* On the GCP console, go to IAM.
* Add your service account `k8s-terraform-sa@${TF_VAR_esgischool_k8s_gcp_projectid}.iam.gserviceaccount.com` as project owner



### Setup K8s cluster

Launch Terraform to setup cluster.

```bash
terraform init -get=true -upgrade=true
terraform apply
```

### Configure kubectl 

```bash
gcloud container clusters get-credentials esgischool-gke-cluster --project $TF_VAR_esgischool_k8s_gcp_projectid  --zone=europe-west3-a
```


### Detroy K8s cluster (at the end of the lab)

At the end of the lab don't forget to stop your GKE cluster

```bash
terraform destroy
```