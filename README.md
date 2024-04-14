# Monitoring Dashboard Mini-Project

This mini-project outlines the creation of a monitoring dashboard for an application. The architecture leverages Prometheus, a monitoring tool, to scrape metrics from a target system, and Grafana, a data visualization tool, to display those metrics in a user-friendly dashboard.The target system, in this case, is a Node.js application. The Prometheus client library is used to collect metrics from the application, which are then exposed on a specific port. Prometheus scrapes these metrics periodically and stores them in its time series database.
Grafana can then query the Prometheus database to retrieve the collected metrics. These metrics can be visualized in a variety of ways, such as graphs, charts, and tables. The dashboard can be customized to display the most important metrics for the application, allowing users to monitor its health and performance at a glance.
All of these components are wrapped inside Docker containers, including the Node.js application, which is a containerized application inside Docker. This approach offers benefits such as isolation, portability, and simplified deployment, allowing for easier management and scaling of the application.This mini-project provides a basic framework for creating a monitoring dashboard for an application. The specific tools and technologies used can be adapted to fit the needs of a particular project.

## Table of Contents

- [Overview](#overview)
- [Prerequisite](#prerequisite)
- [Getting Started](#getting-started)
- [System Architecture](#system-architecture)
- [Running the Project](#running-the-project)
- [Dashboard Customization](#dashboard-customization)
- [Metrics Involved](#metrics-involved)
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

## Metrics Involved

The monitoring dashboard gathers and visualizes several key metrics from the Node.js application:

- **Process CPU Usage**: This metric measures the CPU usage of the Node.js application. Monitoring this helps you identify high CPU usage scenarios that could affect the performance of your application.
- **Event Loop Lag**: This metric indicates the time it takes for the Node.js event loop to handle a set of events. High event loop lag can lead to slower application response times, so keeping track of this metric can help you optimize performance.
- **Process Memory Usage**: This metric tracks the memory usage of the Node.js application. It is important to monitor this metric to ensure the application does not consume excessive memory, which could lead to crashes or other performance issues.
- **Active Handlers**: This metric counts the number of active event loop handlers in the Node.js application. Monitoring this helps you understand the concurrency level of your application and detect potential performance bottlenecks.
- **Heap Used Detail**: This metric measures the amount of memory currently used by the application heap. Keeping an eye on this metric helps you manage memory usage efficiently and prevent potential memory leaks.
- **Heap Available Detail**: This metric measures the total amount of heap memory available to the application. Tracking this metric helps you ensure your application has enough memory to function efficiently.

## Stopping and Cleaning Up

To stop the containers, use the following command:

```sh
docker-compose down
```

## License

This README and the associated code are provided under the [MIT License](LICENSE), allowing you to use and modify them for your blogging needs. Please review the license for more details.
