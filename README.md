# Dockerized ELK Stack with Elasticsearch, Kibana and Filebeat

This project is build to read a CSV file and a JSON file and send the data to Elasticsearch. Everything needed to run the project is present in this directory. After initial setup, follow the last section, Customize filebeat to read your data, to integrate FB in your environment.

## Pre-setup

    sysctl -w vm.max_map_count=262144


## Steps to run this project

#### Generate the certificates (only needed once):

        docker-compose -f create-certs.yml run --rm create_certs

#### Start Elasticsearch cluster onfigured for SSL/TLS:

        docker-compose up -d

#### Create Passwords for ELK stack components:

        docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords auto --batch --url https://es01:9200"

#### Copy generated password from step 4

#### Replace passwords in .env file as copied in Step 4

#### Stop docker containers and restart
        docker-compose down
        docker-composer up -d


## Cutomize Filebeat to read your data

Currently, logstash reads a json file dropped in the data folder of this project and deletes the files after a successful read. You can configure any directory on your machine to be used with FB. Steps are as follows:


1. Open docker-compose file and go to volumes section of logstash service. You will find entries given below:
    volumes:
        - certs:$CERTS_DIR
        - ./data:/home/logstash/data
        - ./conf.d/:/etc/logstash/conf.d/
        - ./logstash_config/pipelines.yml:/usr/share/logstash/config/pipelines.yml
        - ./logstash_config/logstash.yml:/usr/share/logstash/config/logstash.yml


2. Replace the following line with the source folder that you want to use with logstash. Do Not change '/home/logasth/data', only ./data needs to be updated
        - ./data:/home/logstash/data 


## How to modify data pipeline
Current config reads the files with .json extension and send the data to elastic. However, logstash can do much more than that. 

If you want to enhance your data before sending it to elastic, then update the following file:
    - Go to conf.d directory. Here you will find json.conf file. This is logstash data pipeline configuration file.
    




