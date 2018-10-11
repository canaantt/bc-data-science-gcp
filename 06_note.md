#### chores: 
BUCKET-NAME = data-science-bookclub2018-ml


## Step I -  create hadoop cluster

```
gcloud dataproc clusters create --num-workers=2 --scopes=cloud-platform --worker-machine-type=n1-standard-2 --master-machine-type=n1-standard-4 --zone=us-central1-a ch6cluster
```

