# grafana-docker-telegram

For deploying Grafana, Prometheus and Node Exporter, make these steps:
1. Clone repo
```bash
git clone https://github.com/kajihan/prometheus-grafana-telegram.git
```
2. Change configs if necessary (at least `configs/alertmanager.yml` to configure Telegram notification)
3. Deploy containers using docker-compose or the Swarm stack
```bash
docker-compose -f docker-compose up -d
```
###### or for the Swarm stack:
```
docker stack deploy -c grafana-docker-stack/docker-compose.yml monitoring
```

That's it!

If you want to add more servers to prometheus, make these steps:
1. Install node-exporter to each of these servers via these commands:
```bash
git clone https://github.com/kajihan/prometheus-grafana-telegram.git
docker-compose up -f node-exporter.yml -d
```
###### or for the Swarm stack:
```bash
docker stack deploy -c grafana-docker-stack/node-exporter.yml node-exporter
```
2. Add these servers to `configs/prometheus/prometheus.yml` file to `- targets: ['node-exporter:9100']` list of `- job_name: 'node-exporter'` section, like `- targets: ['node-exporter:9100', 'server1:9100', 'server2:9100', '...']`
3. Reload prometheus config via this command:
```bash
docker ps | grep prometheus | awk '{print $1}' | xargs docker kill -s SIGHUP
```