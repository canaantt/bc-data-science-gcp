
# Set up environment

```
git clone  https://github.com/GoogleCloudPlatform/data-science-on-gcp/
cd data-science-on-gcp
export PROJECT_ID=$(gcloud info --format='value(config.project)')
export BUCKET=${PROJECT_ID}-ml
gsutil mb gs://$BUCKET
```

```
cd 02_ingest/monthlyupdate
./ingest_flight.py --bucket $BUCKET --year 2015 --month 01
``` 

Create a Flask web app to ingest data. Flask is a framework for creating lightweight web application with Python. The final objective is to create a web application, written in Python, that can be invoked using Cron so Flask is ideal.

The Python file ingestapp.py uses Flask to host a simple welcome page for the root of the web app that just points to a second URL, /ingest , that calls the ingest_next_month function defined in the ingest_flights.py application you tested in the last task.

To load and run an application on Google App Engine you need to create a yaml file that contains the details that specify the type of application, the application itself, and its parameters.

The repository contains a pre-prepared sample yaml file called app.yaml.

Initialize Google App Engine with creating a new app:
```
gcloud app create --region us-central

git clone https://github.com/GoogleCloudPlatform/python-docs-samples
cd python-docs-samples/appengine/standard/hello_world
```

Stop the demo and change with the real app:
```
gcloud app deploy --quiet --stop-previous-version
cd  ~/data-science-on-gcp/02_ingest/monthlyupdate
sed -i -e 's/cloud-training-demos-ml/'"$BUCKET"'/g' app.yaml
gcloud app deploy
```
Google App Engine [Deployment YAML](https://cloud.google.com/appengine/docs/standard/python/config/appref)