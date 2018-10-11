#### chores: 
BUCKET-NAME = data-science-bookclub2018-ml


## Step I -  Create Hadoop Cluster

```
gcloud dataproc clusters create --num-workers=2 --scopes=cloud-platform --worker-machine-type=n1-standard-2 --master-machine-type=n1-standard-4 --zone=us-central1-a ch6cluster
```
## Step II

- go to Dataproc console in the browser
- ssh to master node
- to verify Hadoop exists by connecting to the master node of the cluster using SSH
```
hdfs dfsadmin -report
```
- to verify Spark exists on the cluster by connecting to the master node via SSH
```
pyspark
```

## Step II Delete Cluster

```
gcloud dataproc clusters delete ch6cluster
```
## Customize Hadoop cluster
!/bin/bash

if [ "$#" -ne 2 ]; then
    echo "Usage: ./create_cluster.sh  bucket-name  zone"
    exit
fi

PROJECT=$DEVSHELL_PROJECT_ID
BUCKET=$1
ZONE=$2
INSTALL=gs://$BUCKET/flights/dataproc/install_on_cluster.sh

##### upload install file
sed "s/CHANGE_TO_USER_NAME/$USER/g" install_on_cluster.sh > /tmp/install_on_cluster.sh
gsutil cp /tmp/install_on_cluster.sh $INSTALL

##### create cluster
gcloud dataproc clusters create \
   --num-workers=2 \
   --scopes=cloud-platform \
   --worker-machine-type=n1-standard-2 \
   --master-machine-type=n1-standard-4 \
   --zone=$ZONE \
   --initialization-actions=gs://dataproc-initialization-actions/datalab/datalab.sh,$INSTALL \
   ch6cluster
  
