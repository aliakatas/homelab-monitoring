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
