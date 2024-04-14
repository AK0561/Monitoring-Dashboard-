# Monitoring Dashboard Mini-Project

This mini-project demonstrates how to create a monitoring dashboard for a Node.js application using Prometheus and Grafana. The project uses Docker to facilitate easy deployment and management of the application, Prometheus, and Grafana.

## Table of Contents

- [Overview](#overview)
- [Prerequisite](#prerequisite)
- [Getting Started](#getting-started)
- [System Architecture](#system-architecture)
- [Running the Project](#running-the-project)
- [Dashboard Customization](#dashboard-customization)
- [Stopping and Cleaning Up](#stopping-and-cleaning-up)
- [License](#license)

## Overview

The architecture of this project includes the following components:
- **Node.js application**: A sample application exposing metrics on a specified port.
- **Prometheus**: A monitoring tool that scrapes metrics from the Node.js application and stores them in its time series database.
- **Grafana**: A data visualization tool that queries Prometheus to retrieve metrics and display them in a customizable dashboard.

The goal of this project is to provide a basic framework for creating a monitoring dashboard for the Node.js application.


## Prerequisite

- Basic knowledge of Node.js and Express Framework.
- Basic to intermediate knowledge in Docker and containerization.

## Getting Started

1. **Clone the repository**: Clone the repository to your local machine.

    ```sh
    git clone <repository-url>
    cd <repository-name>
    ```

2. **Configuration**: You may customize the configuration files located in the `config` folder as needed.

3. **Install Dependencies**: Make sure you have the necessary dependencies installed (e.g., Node.js, Docker, and Docker Compose).

## System Architecture 

![image](https://github.com/AK0561/Monitoring-Dashboard-/assets/97022114/e894592f-297d-48d1-bdbf-be5a96ed15bd)

## Running the Project

1. **Create the Prometheus config file**: Create a file named `prometheus-config.yml` and add the following configuration. Replace `<NODEJS_SERVER_ADDRESS>` with the actual address of your Node.js application.

    ```yaml
    global:
      scrape_interval: 4s
    
    scrape_configs:
      - job_name: prometheus
        static_configs:
          - targets: ["<NODEJS_SERVER_ADDRESS>"]
    ```

2. **Start the Prometheus server**: Use Docker Compose to start the Prometheus server.

    ```yaml
    version: "3"

    services:
      prom-server:
        image: prom/prometheus
        ports:
          - 9090:9090
        volumes:
          - ./prometheus-config.yml:/etc/prometheus/prometheus.yml
    ```

    The Prometheus server will now be up and running at port `9090`.

3. **Setup Grafana**: Start Grafana using Docker.

    ```sh
    docker run -d -p 3000:3000 --name=grafana grafana/grafana-oss
    ```

    Grafana will be available at `http://localhost:3000`.

4. **Logging in to Grafana**: By default, the Grafana login credentials are:
    - **Username**: `admin`
    - **Password**: `admin`

5. **Adding a data source**:
    - In Grafana, navigate to the "Configuration" tab and select "Data Sources".
    - Click on "Add data source" and choose "Prometheus".
    - Configure the data source with the following URL: `http://prometheus:9090`.

## Dashboard Customization

![image](https://github.com/AK0561/Monitoring-Dashboard-/assets/97022114/829a63fb-203d-410b-b97e-c59ac818833c)

Once you have added the Prometheus data source in Grafana, you can create your own dashboards or import existing ones to visualize the metrics. Add graphs, charts, and tables to display metrics from your Node.js application.

## Stopping and Cleaning Up

To stop the containers, use the following command:

```sh
docker-compose down
```
