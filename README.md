# SIEM

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


Step 2: Create Docker Network
Create a Docker network for the ELK stack components
