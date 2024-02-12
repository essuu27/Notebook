# Transfering large datasets into BigQuery without uploading
## Introduction

**[Note]** BigQuery can only import datafiles that are written in CSV, Avro, JSON, ORC, Parquet, or Cloud Datastore formats.

## Preparation
You'll need a few things ready before you can use this procedure. Those things are:
- a Google cloud account with access to the cloud console
- a cloud project already set up that will hold the data in a BigQuery table after it has been transferred
- the URL(s) for the datafile(s)

## The process
- Go to your Google cloud console at https://console.cloud.google.com/

- Open the Cloud Shell by clicking on the icon in the top toolbar.

- Change into the project you want to use. Replace [PROJECTNAME] with the name of the project:
` gcloud config set project [PROJECTNAME] `

- Create a storage bucket that will hold the downloaded data. Replace [BUCKET] with the name you want the bucket to have:
` gcloud storage buckets create gs://[BUCKET] `

- Check that the bucket has been created. Output should show the chosen bucket name
` gcloud storage buckets list | grep name `

- **optional** - make a temporary directory to hold the downloaded data, change into that directory: 
``` 
mkdir data
cd data
```

- download the datafile from the remote web resource. The bang symbol('!') at the start of the command is required:
` !wget https://sometest-site.xyz/bigdatafile.zip `

- unzip/unpack the datafile is it is compressed
` unzip *.zip `

- copy the uncompressed datafiles into the storage bucket. Replace [BUCKET] with the name of the bucket you created earlier:
` gcloud storage cp *.csv gs://[BUCKET]/ `

- check that the datafiles have been successfully copied into the bucket
` gcloud storage ls -l gs://[BUCKET] `

You should see a list of the datafiles that are currently held in the storage bucket. You will need the filenames for the next step of th process:

- load the datafile from the storage bucket [BUCKET] into the BigQuery table [BIGTABLE]
`` bq load --autodetect [BIGTABLE] gs://[BUCKET]/[DATAFILE] ``



```
gcloud storage rm gs://mydata_cycles
gcloud storage ls gs://mydata_cycles/
```

` gcloud storage buckets delete gs://mydata_cycles/ `
