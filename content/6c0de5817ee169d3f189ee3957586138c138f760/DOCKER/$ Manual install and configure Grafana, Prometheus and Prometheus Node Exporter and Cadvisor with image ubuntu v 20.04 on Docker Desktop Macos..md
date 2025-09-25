##### [ Install Prometheus Server on Ubuntu 20.04](https://docs.vultr.com/install-prometheus-server-on-ubuntu-20-04):
 ##### **Install linux ubuntu v20.04.**
 [[$ Create and manage volumes]] represent with container (you can run many container in one volume if you like).
 ```bash
docker run -it -v ubuntu:/shared-volume --name promethe -p 9090:9090 ubuntu:20.04 "remember pull image ubuntu:20.04 in docker desktop befor run this command for create new container run ubuntu os "
```
- Login container with terminal on macos.
```bash
docker exec -it prometheus /bin/sh 
```

##### 1. Update the System

Update the apt package list to prepare the system for further installations.

```bash
 sudo apt update
```

##### 2. Download and Install Prometheus

Prometheus installation files are packaged as precompiled binaries. To download your preferred binaries, you can visit the official Prometheus [download page](https://prometheus.io/download/).

If you decide to install a different version of Prometheus, please note the version numbers in the following examples when downloading and extracting the archives.

Download the Prometheus release package.

```bash
 wget https://github.com/prometheus/prometheus/releases/download/v2.27.1/prometheus-2.27.1.linux-amd64.tar.gz
```

Extract the downloaded archive.

```bash
 tar xvf prometheus-2.27.1.linux-amd64.tar.gz
```

Change directory to the extracted archive.

```bash
 cd prometheus-2.27.1.linux-amd64
```

Create the configuration file directory.

```bash
 sudo mkdir -p /etc/prometheus
```

Create the data directory.

```bash
 sudo mkdir -p /var/lib/prometheus
```

Move the binary files `prometheus` and `promtool` to `/usr/local/bin/`.

```bash
 sudo mv prometheus promtool /usr/local/bin/
```

Move console files in `console` directory and library files in `console_libraries` directory to `/etc/prometheus/` directory.

```bash
 sudo mv consoles/ console_libraries/ /etc/prometheus/
```

Move the template configuration file `prometheus.yml` to `/etc/prometheus/` directory

```bash
 sudo mv prometheus.yml /etc/prometheus/prometheus.yml
```

Verify the installed version of Prometheus.

```bash
 prometheus --version
```

Verify the installed version of promtool.

```bash
 promtool --version
```

##### 3. Configure System Group and User

Create a `prometheus` group.

```bash
 sudo groupadd --system prometheus
```

Create a user `prometheus` and assign it to the created `prometheus` group.

```bash
 sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```

Set the ownership of Prometheus files and data directories to the `prometheus` group and user.

```bash
 sudo chown -R prometheus:prometheus /etc/prometheus/  /var/lib/prometheus/

 sudo chmod -R 775 /etc/prometheus/ /var/lib/prometheus/
```

##### 4. Configure Systemd Service

Create a systemd service file for Prometheus to start at boot time.

```bash
 sudo nano /etc/systemd/system/prometheus.service
```

Add the following lines to the file and save it:

```bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Restart=always
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090

[Install]
WantedBy=multi-user.target
```

Start the Prometheus service.

```bash
 sudo systemctl start prometheus
```

Enable the Prometheus service to run at system startup.

```bash
 sudo systemctl enable prometheus
```

Check the status of the Prometheus service.

```bash
 sudo systemctl status prometheus
```

##### Access Your Server

Access the Prometheus interface through your browser at port 9090. For example:

```bash
http://localhost:9090
```

#### [How to install Prometheus Node Exporter on Ubuntu 20.04](https://ourcodeworld.com/articles/read/1686/how-to-install-prometheus-node-exporter-on-ubuntu-2004)

##### **Install linux ubuntu v20.04.**
 
 ```bash
 docker run -it --name node-exporter -p 9100:9100 ubuntu:20.04 "remember pull image ubuntu:20.04 in docker desktop befor run this command for create new container run ubuntu os "
```
- Login container with terminal on macos.
```bash
docker exec -it node_exporter /bin/sh 
```

##### 1. Download Node Exporter

As first step, you need to download the Node Exporter binary which is [available for Linux in the official Prometheus website here](https://prometheus.io/download/#node_exporter). In the website, you will find a table with the list of available builds. Of our interest in this case, is the `node_exporter` build for Linux AMD64:

![Node Exporter Ubuntu Linux](https://cdn.ourcodeworld.com/public-media/gallery/gallery-623f7f70cb966.png)

In this case the latest available version is the 1.3.1. Copy the `.tar.gz` URL and download it somewhere in your server using wget or cURL:

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```

Copy snippet

##### 2. Extract Node Exporter and move binary 

After downloading the latest version of Node Exporter, proceed to extract the content of the downloaded tar using the following command:

```bash
tar xvf node_exporter-1.3.1.linux-amd64.tar.gz
```

Copy snippet

The content of the zip will be extracted in the current directory, the extracted directory will contain 3 files:

- LICENSE (license text file)
- node_exporter (binary)
- NOTICE (license text file)

You only need to move the binary file `node_exporter` to the `/usr/local/bin` directory of your system. Switch to the node_exporter directory:

```bash
cd node_exporter-1.3.1.linux-amd64
```

Copy snippet

And then copy the binary file with the following command:

```bash
sudo cp node_exporter /usr/local/bin
```

Copy snippet

Then you can remove the directory that we created after extracting the zip file content:

```bash
# Exit current directory
cd ..

# Remove the extracted directory
rm -rf ./node_exporter-1.3.1.linux-amd64
```

Copy snippet

##### 3. Create Node Exporter User

As a good practice, create an user in the system for Node Exporter:

```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
```

Copy snippet

And set the owner of the binary `node_exporter` to the recently created user:

```bash
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

Copy snippet

##### 4. Create and start the Node Exporter service

The Node Exporter service should always start when the server boots so it will always be available to be scrapped for information. Create the `node_exporter.service` file with nano:

```bash
sudo nano /etc/systemd/system/node_exporter.service
```

Copy snippet

And paste the following content in the file:

```bash
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

Copy snippet

Close nano and save the changes to the file. Proceed to reload the daemon with:

```bash
sudo systemctl daemon-reload
```

Copy snippet

And finally enable the `node_exporter` service with the following command:

```bash
sudo systemctl enable node_exporter
```

Copy snippet

And then start the service:

```bash
sudo systemctl start node_exporter
```

Copy snippet

##### 5. Test the Node Exporter service

As last step, access your server through the web browser at port 9100 and browse the metrics (**http://your_server_ip:9100/metrics**). You should get an output in the browser similar to:

```bash
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 2.5948e-05
go_gc_duration_seconds{quantile="0.25"} 2.9566e-05
go_gc_duration_seconds{quantile="0.5"} 3.0488e-05
go_gc_duration_seconds{quantile="0.75"} 3.2111e-05
go_gc_duration_seconds{quantile="1"} 0.000232387
go_gc_duration_seconds_sum 1.454063444
go_gc_duration_seconds_count 23486
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 9
# HELP go_info Information about the Go environment.
# TYPE go_info gauge
go_info{version="go1.17.3"} 1
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 2.365264e+06
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 5.0367212352e+10
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 1.897095e+06
```

Copy snippet

If you get some information at the mentioned URL, then your service has been properly configured and it's ready to be scrapped by Prometheus.

##### If port 9100 is unreachable

As we mentioned previously, by default the node exporter service will run on the port 9100 of your server. If after starting the service, the service is unreachable, do not forget to open the port 9100 in your Ubuntu server. If you are using UFW (Uncomplicated Firewall), you can easily open this port using the following instruction:

```bash
sudo ufw allow 9100
```

Copy snippet

Alternatively if you are using IPTABLES, use the following command to allow incoming traffic on that port instead:

```bash
sudo iptables -I INPUT -p tcp -m tcp --dport 9100 -j ACCEPT
```

Copy snippet

Happy monitoring.

#### [Implement cAdvisor as a container](https://www.kubecost.com/kubernetes-devops-tools/cadvisor/)

Run the following command to create a cAdvisor container in Docker:
```bash
sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  gcr.io/cadvisor/cadvisor:v0.39.3
```