***Reference:***

- **[Link youtube](https://www.youtube.com/watch?v=RAqMP_NnGec&t=23s)**
- [Link web](https://www.virtualizationhowto.com/2023/01/prometheus-node-exporter-cadvisor-grafana-install-and-configure/) 

***Command use:***

- *Check container running in docker desktop* .
```bash
  docker ps 
  ```
- *Install docker compose*.
```bash
sudo apt-get install docker-compose -y
```
- Check id user use in configuration file docker compose yml.
```bash
id -u
```
- *Create directory for project*  ![[Pasted image 20241011151520.png]]
```bash
mkdir -p promgrafnode/prometheus
mkdir -p promgrafnode/grafana/provisioning
touch promgrafnode/docker-compose.yml
touch promgrafnode/prometheus/prometheus.yml
```
- *Install services whith docker conpose file yml*, copy all command below to <mark style="background: #FFF3A3A6;">docker-compose.yml</mark> file.
```bash
cd promgrafnode\
nano docker-compose.yml
```
  
  **note:** remember change <mark style="background: #FFF3A3A6;">ID</mark> user. if you want setup <mark style="background: #FFF3A3A6;">static ip</mark> for containers can refernce [link ](https://www.howtogeek.com/devops/how-to-assign-a-static-ip-to-a-docker-container/)
```bash
version: '3.8' 
networks: 
  monitoring: 
    driver: bridge 
volumes: 
  prometheus_data: {} 
services: 
  node-exporter: 
    image: prom/node-exporter:latest 
    container_name: node-exporter 
    restart: unless-stopped 
    volumes: 
      - /proc:/host/proc:ro 
      - /sys:/host/sys:ro 
      - /:/rootfs:ro 
    command: 
      - '--path.procfs=/host/proc' 
      - '--path.rootfs=/rootfs' 
      - '--path.sysfs=/host/sys' 
      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)' 
    ports: 
      - 9100:9100 
    networks: 
      - monitoring 
  prometheus: 
    image: prom/prometheus:latest 
    user: "501" 
    environment: 
      - PUID=501 
      - PGID=501 
    container_name: prometheus 
    restart: unless-stopped 
    volumes: 
      - ~/promgrafnode/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml 
      - ~/promgrafnode/prometheus:/prometheus 
    command: 
      - '--config.file=/etc/prometheus/prometheus.yml' 
      - '--storage.tsdb.path=/prometheus' 
      - '--web.console.libraries=/etc/prometheus/console_libraries' 
      - '--web.console.templates=/etc/prometheus/consoles' 
      - '--web.enable-lifecycle' 
    ports: 
      - 9090:9090 
    networks: 
      - monitoring 
  grafana: 
    image: grafana/grafana:latest 
    user: "501" 
    container_name: grafana 
    ports: 
      - 3000:3000 
    restart: unless-stopped 
    volumes: 
      - ~/promgrafnode/grafana/provisioning/datasources:/etc/grafana/provisioning/datasources 
      - ~/promgrafnode/grafana:/var/lib/grafana 
    networks: 
      - monitoring 
  cadvisor: 
    image: gcr.io/cadvisor/cadvisor:latest 
    container_name: cadvisor 
    ports: 
      - 8081:8080 
    networks: 
      - monitoring 
    volumes: 
      - /:/rootfs:ro 
      - /var/run:/var/run:rw 
      - /sys:/sys:ro 
      - /var/lib/docker/:/var/lib/docker:ro 
    depends_on: 
      - redis 
  redis: 
    image: redis:latest 
    container_name: redis 
    ports: 
      - 6379:6379 
    networks: 
      - monitoring
```
- *Run docker compose file create all services in docker-compose file.* 
```bash
docker-compose up -d
```
- *Check container already up after finish download and run*.
```bash
docker-compose ps
```
- *Setup Metrics forwarding form prometheus to grafana with infomation insert to <mark style="background: #FFF3A3A6;">grafana.yml file</mark>*.
```bash
cd promgrafnode\grafana\
nano grafana.yml
```

**note:** copy and paste all command below and remember change information <mark style="background: #FFB86CA6;">ip address matching with ip services setup before.</mark>

```bash
global: 

  scrape_interval: 1m

scrape_configs: 

  - job_name: "prometheus" 

    scrape_interval: 1m 

    static_configs: 

    - targets: ["localhost:9090"]

  - job_name: "node" 

    static_configs: 

    - targets: ["node-exporter:9100"]

  - job_name: "cadvisor" 

    scrape_interval: 5s 

    static_configs: 

    - targets: ["10.1.149.19:8081"]  

  - job_name: "hvhost01" 

    scrape_interval: 5s 

    static_configs: 

    - targets: ["10.1.149.191:9182"]
```

- *Stop and Start prometheus container for apply configure file prometheus.yml*.
```bash
docker stop prometheus
docker start prometheus
```

- *Configure*