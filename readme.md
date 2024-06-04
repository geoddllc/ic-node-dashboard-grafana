# Prometheus and Grafana for IC Node Metrics Visualization

This repository facilitates the visualization of metrics from IC nodes using Prometheus Node Exporter and Grafana, utilizing Docker for containerization.

## Prerequisites

Before you start, make sure you have:
- Docker installed on your host system.
- IPv6 configured on your host OS, allowing Docker containers to communicate via IPv6.

## Configuration

Ensure the `prometheus.yml` configuration file is in your repository's root directory. This file is pre-configured to scrape metrics from IC nodes.

## Docker Network

First, create a Docker network to facilitate communication between the Prometheus and Grafana containers:

```bash
docker network create graf_net
```

## Running Prometheus

Launch the Prometheus container, using the configuration file from your repository and ensuring data persistence:

```bash
docker run -d --name=prometheus \
  -p 9090:9090 \
  -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml \
  -v prometheus_data:/prometheus \
  --network=graf_net \
  prom/prometheus
```

## Running Grafana
Start the Grafana container, ensuring data persistence:

```bash
docker run -d --name=grafana \
  -p 3000:3000 \
  -v grafana_data:/var/lib/grafana \
  --network=graf_net \
  grafana/grafana
```

## Access and Configuration

- Prometheus: Access the Prometheus UI by navigating to http://localhost:9090. Verify that Prometheus is correctly scraping data by checking http://localhost:9090/targets.

- Grafana: Access Grafana at http://localhost:3000. Default login credentials are typically admin for both username and password. Configure Grafana by adding Prometheus as the data source and importing your Grafana dashboard configurations.

## Importing Dashboard Configuration into Grafana

To visualize the metrics collected by Prometheus, you need to import a pre-configured dashboard into Grafana. Follow these steps to import the `dashboard.json` file into your Grafana setup:

1. **Access Grafana**: 
   Open your web browser and navigate to `http://localhost:3000` to access the Grafana user interface.

2. **Login**: 
   Log in with the default or your configured credentials. The default is typically `admin` for both the username and password.

3. **Navigate to Dashboard Import**:
   - Click on the "+" icon on the left sidebar.
   - Select "Import".

4. **Import Dashboard**:
   - Click on 'Upload JSON file'.
   - Choose the `dashboard.json` file from your local system or copy and paste the JSON directly into the text box provided.

5. **Select Prometheus as the Data Source**:
   - Ensure that you select the correct Prometheus data source from the dropdown menu that you have previously set up to gather metrics.

6. **Save and Load Dashboard**:
   - Click 'Import' to add the dashboard to your Grafana.

7. **Enter Node IP**:
   - Enter the NODE IP with Port in the host input ```[IPv6:42372]```


## Future Enhancements
- Implement a dropdown list for targets in the Grafana dashboard.
- Setup alerts within Grafana to monitor critical metrics and node health.

## Contributions
Contributions to this project are welcome! Please feel free to fork this repository, submit pull requests, or create issues for any improvements or additional features you wish to discuss.