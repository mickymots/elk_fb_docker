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

Currently, FB reads a sample file stored in data folder of this project. You can configure any directory on your machine to be used with FB. Steps are as follows:

    1. Open docker-compose file and go to volumes section of filebeat service
    2. Replace the following line with the source folder that you want to use with filebeat. Do Not change '/home/filebeat/data', only ./data needs to be updated
        - ./data:/home/filebeat/data 

    *   Go to inputs.d directory. Here you will find 2 yml files. CSV_Config.yml is configured to read a CSV file and json_config.yml is configured to read a json file
    *   if your file is a json document like the test.json in data folder then update only the name of the file as highlighted below:
        paths:
         - /home/filebeat/data/{test.json --- update this much only}

    * If your file is a CSV file like the test.csv in data folder then do the following:
        *   update the name of the file 
            paths:
            - /home/filebeat/data/{test.csv --- update this much only}

        *   update the extract array processor: 

            here mapping represents the columns of csv file. 
            - extract_array:
                field: decoded
                mappings:
                    name: 0
                    age: 1