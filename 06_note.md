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

## Step III  Delete Cluster

```
gcloud dataproc clusters delete ch6cluster
```
## Customize Hadoop cluster
https://github.com/canaantt/data-science-on-gcp/blob/master/06_dataproc/create_cluster.sh

```
./create_cluster.sh data-science-bookclub2018-ml us-central1-a
```

## Step IV Spark
```
gcloud compute ssh  --zone=us-central1-a  --ssh-flag="-D 1080" --ssh-flag="-N" --ssh-flag="-n" ch6cluster-m
```

```
rm -rf /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome 
/usr/bin/chrome \
  --proxy-server="socks5://localhost:1080" \
  --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" \
  --user-data-dir=/tmp/junk
```
