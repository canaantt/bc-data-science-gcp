
# Set up environment
```
    gcloud auth list
```

[gcloud](https://cloud.google.com/sdk/gcloud/) is the powerful and unified command-line tool for Google Cloud Platform. 

```
    gcloud config list project
```

```
git clone \
   https://github.com/GoogleCloudPlatform/data-science-on-gcp/
cd data-science-on-gcp
mkdir data
cd data
```

##

##### Get Data using curl 
```
curl https://www.bts.dot.gov/sites/bts.dot.gov/files/docs/legacy/additional-attachment-files/ONTIME.TD.201501.REL02.04APR2015.zip --output data.zip
```

Explore the data
```
unzip data.zip
head ontime.td.201501.asc
```

The file doesnt have header information. 

##### Get custom data from a web form using curl

```
bash ../02_ingest/download.sh
```
It will take about five minutes to fetch all twelve source data files. 

Rename the files

```
for month in `seq -w 1 12`; do
   unzip 2015$month.zip
   mv *ONTIME.csv 2015$month.csv
   rm 2015$month.zip
done
```

Note that the data lines have a trailing comma at the end and all string fields are delimited with quotation marks.

Count the number of columns: 
```
head -2 201503.csv  | tail -1 | sed 's/,/ /g' | wc -w
```

Count the number of rows:
```
wc -l *.csv
```

Clean up the trailing commo and quotation marks using ```sed```:

```
for month in `seq -w 1 12`; do
    sed 's/,$//g' 2015$month.csv | sed 's/"//g' > tmp
    mv tmp 2015$month.csv
done
```

make a storage bucket using the project-id as a suffix and copy all the data files to the storage bucket:
```
export PROJECT_ID=$(gcloud info --format='value(config.project)')
gsutil mb gs://${PROJECT_ID}-ml
gsutil -m cp *.csv gs://${PROJECT_ID}-ml/flights/raw/
```