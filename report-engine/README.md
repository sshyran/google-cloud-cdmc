# CDMC REPORT ENGINE

This folder contains the CDMC Report Engine, the python application which based on configuration files, configuration tables and BigQuery jobs metadata inspects that BigQuery sensitive data assets follow the CDMC controls.

The repository contains 12 dedicated controls implementations classes which refers to the 14 CDMC controls, being controls 1 and 5 implicitly implemented and not having dedicated classes.

GCP CDMC Report Engine is a Python flask application, with execution based on rest requests.


## DEPLOYMENT

### REQUIREMENTS

Python3 environment;
- Metadata generated by [TAG Engine] (https://github.com/GoogleCloudPlatform/datacatalog-tag-engine);
- Pub/Sub topics and subscriber based on terraform folder in this repo;
- According to the deployment service (Cloud Run or Compute Engine) a service account with the following privileges:
-- Data Catalog Viewer in the inspected project;
-- BigQuery Metadata Viewer in the inspected project;
-- Data Lineage Editor in the inspected project;
-- BigQuery Data Editor in the deployment project;
-- Pub/Sub Publisher in the deployment project;


### Cloud Run (Recommended)

The CDMC Report Engine contains a Dockerfile compatible with Cloud Run deployment.


### Compute Engine

The following steps relate to the Compute Engine deployment:
Clone this repository;
Install all required Python libraries in requirements.txt
Execute the main.py file;


## EXECUTION

CDMC Report Engine is a Flask web application. The following request parameters must be considered for the POST method in /generate:

- (Mandatory) orgId=<INTEGER> the GCP Organization ID of the inspection;
- (Mandatory) projectId=<STRING> the GCP Project ID to be inspected;
- (Mandatory) projectNumber= <INTEGER> the inspected GCP Project number
- (Mandatory) topicProjectId=<STRING> the GCP Project ID that has the Pub/Sub topic that receive the alerts;
- (Mandatory) topic=<STRING> the Pub/Sub topic name
- (Optional) controls= <String> the list of all CDMC controls to be inspected, the format of list are two digit per control. Ex.: 020312 inspects the controls 02, 03 and 12. The default value is “all”.
- (Optional) assetsScope=<BOOLEAN> informs if the log of inspected assets must be sent to a Pub/Sub topic configured in config.ini ASSETS_SCOPE section. Default value is TRUE

Sample request format:
```
curl -X POST 'https://<endpoint>/generate?orgId=<INTEGER>&projectId=<STRING>&topicProjectId=<STRING>&topic=<STRING>&projectNumber=<INTEGER>&controls=020608'
```


## CONFIGURATION CHANGES

It is recommended that Dataplex Catalog templates, Pub/Sub topics and BigQuery configuration tables keep the original values, in case these values must be adjusted, the requirements/config.ini must be considered.