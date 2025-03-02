# Homelab resource monitoring 
Instructions to set up some monitoring on system resources for your homelab and other machines on your network.

## Stack
We are using InfluxDB for storing the data and Grafana for visualisation.

These two are running from a Docker stack on the home server. 

Data collection is facilitated by Telegraf which can be spun up on any machine that you wish to monitor.

## Implementation
The stack(s) can be deployed using the corresponding docker-compose files:
- InfluxDB + Grafana: [`docker-compose.yml`](./visualisation/docker-compose.yml)
- Telegraf: [`docker-compose.yml`](./data-collection/docker-compose.yml)

### InfluxDB
To avoid messing with access tokens, we are sticking with InfluxDB v1.8.

For both InfluxDB and Grafana, you need to define the variables in a `.env` file. Otherwise the default values will be used.

There is an optional specification for a Docker network. 
Please remove if not needed. 

### Telegraf
For data collection, you can spin up the container using the relevant docker-compose file. 
It does require you to edit the telegraf configuration file [`telegraf.conf`](./data-collection/telegraf.conf) to your specific needs.
Mind that the machine tag needs to be unique for each machine to get meaningful data.

## Autostart
To configure the service to start on reboot, execute the following:

### InfluxDB+Grafana
```sh
chmod +x configure-service.sh
./configure-service.sh ./visualisation/homelab-monitoring-visualise.service "WorkingDirectory" $(pwd)/visualisation
./configure-service.sh ./visualisation/homelab-monitoring-visualise.service "EnvironmentFile" $(pwd)/visualisation/.env
sudo ln -sf "$(pwd)/visualisation/homelab-monitoring-visualise.service" /etc/systemd/system/homelab-monitoring-visualise.service
sudo systemctl daemon-reload
sudo systemctl start homelab-monitoring-visualise.service
sudo systemctl enable homelab-monitoring-visualise.service
```
### Telegraf
```sh
chmod +x configure-service.sh
./configure-service.sh ./data-collection/homelab-monitoring-data-collection.service "WorkingDirectory" $(pwd)/data-collection
./configure-service.sh ./data-collection/homelab-monitoring-data-collection.service "EnvironmentFile" $(pwd)/data-collection/.env
sudo ln -sf "$(pwd)/visualisation/homelab-monitoring-data-collection.service" /etc/systemd/system/homelab-monitoring-data-collection.service
sudo systemctl daemon-reload
sudo systemctl start homelab-monitoring-data-collection.service
sudo systemctl enable homelab-monitoring-data-collection.service
```