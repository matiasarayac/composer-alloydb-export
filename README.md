# Composer AlloyDB Export Database


This is repository is intended to create an AlloyDB database export using Google Cloud Composer

1. Setting up your project ID and enable AlloyDB API on your project
```
gcloud config set project $PROJECT_ID
gcloud services enable alloydb.googleapis.com
gcloud services enable servicenetworking.googleapis.com
```

2. Create a VPC for your AlloyDB instance (skip this step if you already have a VPC)

```
gcloud compute networks create alloydb-vpc \
    --subnet-mode=custom

gcloud compute networks subnets create alloydb-subnet \
    --network=alloydb-vpc \
    --range=10.0.0.0/24 \
    --region=us-central1

gcloud compute firewall-rules create allow-internal \
--network alloydb-vpc --allow tcp,udp,icmp --source-ranges 10.0.0.0/24

gcloud compute firewall-rules create allow-ssh-rdp-icmp \
--network alloydb-vpc --allow tcp:22,tcp:3389,icmp
```

3. Create the AlloyDB cluster and instance
```
gcloud alloydb clusters create alloydb-cluster-demo \
    --password=Helloworld \
    --network=alloydb-vpc \
    --region=us-central1 \
    --project=$PROJECT_ID

gcloud alloydb instances create alloydb-instance-demo \
    --instance-type=PRIMARY \
    --cpu-count=2 \
    --region=us-central1 \
    --cluster=alloydb-cluster-demo \
    --project=$PROJECT_ID
```


