# SIEM System Setup Using ELK Stack and Filebeat

## Introduction

This document outlines the steps taken to set up a Security Information and Event Management (SIEM) system using the ELK stack (Elasticsearch, Logstash, and Kibana) and Filebeat for log shipping on a (MacBook Air M1). The setup ensures that logs are collected from various sources, processed, and visualized effectively.

## Prerequisites

- Docker and Docker Compose installed.
- Basic understanding of Docker and container orchestration.
- Access to a terminal with administrative privileges.

## Setup Steps

### Step 1: Install Docker and Docker Compose

Ensure Docker and Docker Compose are installed and running on your MacBook Air M1.
```sh
brew install --cask docker
open /Applications/Docker.app
```

### Step 2: Create Docker Network
Create a Docker network for the ELK stack components
```sh
docker network create elk
```

### Step 3: Create Docker Network
Run the Elasticsearch container.
```sh
docker run -d --name elasticsearch --net elk -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.13.2
```

### Step 4: Create Docker Network
Run the Logstash container with a configuration to process logs from Filebeat and send them to Elasticsearch.
```sh
docker run -d --name logstash --net elk -p 5044:5044 -p 9600:9600 -v $(pwd)/logstash.conf:/usr/share/logstash/pipeline/logstash.conf docker.elastic.co/logstash/logstash:7.13.2
```

### Step 5: Run Kibana
Run the Kibana container to visualize the logs.
```sh
docker run -d --name kibana --net elk -p 5601:5601 docker.elastic.co/kibana/kibana:7.13.2
```

### Step 6: Configure Filebeat
Configure Filebeat to send logs to Logstash.
Run the Filebeat container:
```sh
docker run -d --name filebeat --net elk -v $(pwd)/filebeat.yml:/usr/share/filebeat/filebeat.yml docker.elastic.co/beats/filebeat:7.13.2
```


### Step 7: Verify Log Ingestion
#### 1. Send a Test Log Message:
```sh
docker exec -it filebeat /bin/bash
echo "Test log message" >> /var/log/test.log
```
#### 2. Check Elasticsearch Indices:

```sh
docker exec -it elasticsearch /bin/bash
curl -X GET "localhost:9200/_cat/indices?v"
```

### Step 8: Configure Kibana
#### 1. Open Kibana: Navigate to http://localhost:5601 in your browser.
#### 2. Create Index Pattern:
- Go to "Stack Management" > "Index Patterns".
- Click "Create index pattern".
- Enter logstash-* and select @timestamp.
- Click "Create index pattern".

### Step 9: Verify Data in Kibana
#### 1. Navigate to "Discover" in Kibana.
#### 2. Select the logstash-* index pattern.
#### 3. View and analyze the logs.



