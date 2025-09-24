---
Status: true
New field: 2025-06-22T21:54
Stage: Source
---

##### **Install Elasticsearch**
- Link web site: [elastic.io](https://www.elastic.co/docs/deploy-manage/deploy/self-managed/install-elasticsearch-with-rpm) 
- Link login Kibana : http://172.16.3.3:5601/
- Link login Elasticsearch: https://172.16.3.3:9200/
- Note: all file download & install via directory U01 on vm Monitor_Server (virtual machine running oracle linux 8 in [Clear Sky Cloud](https://console3.clearsky.vn/#provision-tab))

![[Pasted image 20250523112407.png]]

![[Pasted image 20250523113444.png]]
##### **Introduction**

Elasticsearch, Logstash, and Kibana, which 3 open-source applications developed, managed, and supported by Elastic, formed the foundation of the ELK Stack.

**Elasticsearch** is a the whole text search and evaluation engine which employs the Apache Lucene free search engine. It includes a distributed, multitenant full-text search engine, an HTTP web interface, and schema-free documents in JSON.  
**Logstash** is a logging aggregator that accepts data from a variety of input sources, applies various transformations and enhancements, and then sends the data to a variety of supported output destinations.  
**Kibana** is a visualization layer that sits on above Elasticsearch, which allowing users to examine and visualize data. Last but not least, Beats are simple agents which are put on edge hosts to collect various types of data before forwarding it into the stack.

**How to Set Up RHEL to Install ELK S**tack  

Let’s install the ELK stack on the Oracle Linux server. Rocky and RHEL, as well as other RHEL-based distributions, can be installed using the same steps. Let’s get a brief overview of what each component:

- Elasticsearch saves the logs sent by clients.

- These logs are processed by Logstash.

- Kibana provides a web interface for inspecting and analyzing logs.

We will need to install Java JDK version 21, the most recent version stable release, which is required by the ELK components.

You may want to first check the [Java downloads page](https://www.oracle.com/java/technologies/downloads/) to see if a newer version is available.

**Step 1:** You need to first make sure the your server is up to date the commands below

```bash
dnf update -y && dnf upgrade -y

dnf clean all

reboot
```
![[Pasted image 20250517053101.png]]

**Step 2:** Installation of Java Development Kit (JDK)
```bash
cd /opt

wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.rpm

rpm -Uvh jdk-21_linux-x64_bin.rpm

java -version

cd /

clear
```

Time to see if the installation was successful:
![[Pasted image 20250517054626.png]]

**Step 3:** Install Elasticsearch in Oracle Linux.

**I.** Import the Elasticsearch public GPG key into the RPM package manager.
```bash
rpm--import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```
![[Pasted image 20250517054942.png]]

**II.** Create the repository configuration file elasticsearch.repo, add the following lines:

nano /etc/yum.repos.d/elasticsearch.repo
```bash
[elasticsearch]
name=Elasticsearch repository for 9.x packages
baseurl=https://artifacts.elastic.co/packages/9.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabed=0
autorefresh=1
type=rpm-md
```
![[Pasted image 20250517055332.png]]

**III.** Update to the latest packages to the latest version. Get and install the Elasticsearch package.
```bash
dnf update -y && dnf makecache
dnf install --enablerepo=elasticsearch elasticsearch -y
```
![[Pasted image 20250517055945.png]]

**Note:** When the installation is completed, you will be need to copy and keep the credentials for the superuser under autoconfiguration information section of the installation output shown from the line below
```bash
The generated password for the elastic built-in superuser is : zZzct2F8E-7TrKNW_24I
```

**Option:** you can change password superuser if you want with command
```bash
/usr/share/elasticsearch/bin/elasticsearch-reset-password -i -u elastic "run command with elasticsear.service start already"
```
![[Pasted image 20250520174556.png]]

**IV.** Confirm the package installation.
```bash
rpm -qi elasticsearch
```
![[Pasted image 20250517060151.png]]

**V.** Allow remote access by specific the IP or IP Range as shown below
```bash
nano /etc/elasticsearch/elasticsearch.yml
```

Uncomment and edit the lines as shown below or leave the commented lines and make new entry as shown below
```bash
network.host: 0.0.0.0  
http.port: 9200  
node.name: elasticsearch-dbiz-monitor 
# Single Node Discovery  
discovery.seed_hosts: ["single-node"]
transport.host: 0.0.0.0
```
![[Pasted image 20250517060805.png]]
![[Pasted image 20250517060904.png]]
![[Pasted image 20250517060922.png]]

**VI.** Launch and enable the service.
```bash
systemctl daemon-reload  
systemctl enable elasticsearch  
systemctl start elasticsearch  
systemctl status elasticsearch
```
![[Pasted image 20250517062327.png]]

**VII.** Allow traffic over TCP port 9200 in your firewall.
```bash
firewall-cmd --add-port=9200/tcp

firewall-cmd --zone=public --permanent --add-port=9200/tcp

firewall-cmd --reload
```
![[Pasted image 20250517062518.png]]

**VIII.** Type on the browser, you will get a login page. Enter the username and password. The password to use here is the one which you copied and saved from the auto configuration information section.

```bash
https://172.16.3.16:9200
user: elastic
password: 4_oJFcmIIXHum+0uDvcU
```

![[Pasted image 20250520173128.png]]
###### **Step 4:** Install Logstash

![[Pasted image 20250521114410.png]]
**I.** Download and install the public signing key:
```bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

**II.** Add the following in your `/etc/yum.repos.d/` directory in a file with a `.repo` suffix, for example `logstash.repo`
```bash
[logstash-9.x]
name=Elastic repository for 9.x packages
baseurl=https://artifacts.elastic.co/packages/9.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

**III.** And your repository is ready for use. You can install it with command
```bash
sudo yum install logstash
```
![[Pasted image 20250521055548.png]]

**case study:**
	- Grant permission on directory logstash, this allows any member of the `logstash` group (including the `logstash` user) to execute files within `/share/logstash` and traverse the directory.
```bash
sudo usermod -a -G logstash logstash "Add the `logstash` user to the `logstash` group:"

sudo chgrp logstash /share/logstash "Change the group ownership of `/share/logstash` to the `logstash` group:"

sudo chmod g+x /share/logstash "Grant execute permissions to the group:"

```

After installing Logstash, you can move on to configuring it. Logstash’s configuration files reside in the `/etc/logstash/conf.d` directory. For more information on the configuration syntax, you can check out the [configuration reference](https://www.elastic.co/guide/en/logstash/current/configuration-file-structure.html) that Elastic provides. As you configure the file, it’s helpful to think of Logstash as a pipeline which takes in data at one end, processes it in one way or another, and sends it out to its destination (in this case, the destination being Elasticsearch). A Logstash pipeline has two required elements, `input` and `output`, and one optional element, `filter`. The input plugins consume data from a source, the filter plugins process the data, and the output plugins write the data to a destination.

Insert the following `input` configuration. This specifies a `beats` input that will listen on TCP port `5044`. Insert the following `output` configuration. Essentially, this output configures Logstash to store the Beats data in Elasticsearch, which is running at `localhost:9200`, in an index named after the Beat used. The Beat used in this tutorial is Filebeat:

```bash
input {
  beats {
    port => 5044
  }
}

output {
  if [@metadata][pipeline] {
	elasticsearch {
        hosts => ["localhost:9200"]
        manage_template => false
        index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
        pipeline => "%{[@metadata][pipeline]}"
	}
  } else {
	elasticsearch {
        hosts => ["localhost:9200"]
        manage_template => false
        index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
	}
  }
}
```
![[Pasted image 20250521115153.png]]

```bash
/usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
```
![[Pasted image 20250521114959.png]]

If there are no syntax errors, your output will display `Config Validation Result: OK. Exiting Logstash` after a few seconds. If you don’t see this in your output, check for any errors noted in your output and update your configuration to correct them. Note that you will receive warnings from OpenJDK, but they should not cause any problems and can be ignored.

If your configuration test is successful, start and enable Logstash to put the configuration changes into effect
```bash
sudo systemctl start logstash

sudo systemctl enable logstash
   
```

**Case study:**
- Test send email to gmail any information in kibana.log file

Create file test_logstash.conf with content following.
```bash
input {
  file {
    path => "/var/log/kibana/*.log"  # Adjust the path to your Kibana log files
    start_position => "beginning"
    sincedb_path => "/dev/null" # disable sincedb
  }
}

filter {
  # Optional: Add filters to parse and structure the log data
  # Example:
   grok {
     match => { "message" => "%{TIMESTAMP_ISO8601:timestamp} %{LOGLEVEL:level} %{GREEDYDATA:message}" }
   }
}

output {
  email {
    to => "phongict46@gmail.com"  # Replace with your Gmail address
    from => "phongict46@gmail.com"  # Replace with a Gmail address you control
    subject => "Kibana Log Alert"
    body => "Log message: %{message}"
    via => "smtp"
    address => "smtp.gmail.com"
    port => 587
    username => "phongict46@gmail.com"  # Replace with your Gmail address
    password => "yvms xwxi ykmw zaqk"  # Replace with your Gmail password or app password
    authentication => "plain"
    use_tls => true
    # enable_starttls_auto => true
  }
  stdout {
    codec => rubydebug
  }
}
```

Run test logstash with command, and check mail box sure any log from kibana.log send to email account already setup in file test_logstash.conf. Press Ctr+C for stop.
```bash
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash_test.conf
```
![[Pasted image 20250522154358.png]]
![[Pasted image 20250522154306.png]]




###### **Step 5: Install & configure kibana**
**I.** Create the repo file and enter the following lines into the repository configuration file
```bash
nano /etc/yum.repos.d/kibana.repo
```

```bash
[kibana-9.X]
name=Kibana repository for 9.x packages
baseurl=https://artifacts.elastic.co/packages/9.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```
![[Pasted image 20250521135903.png]]

**II.** To install the Kibana package:
```bash
dnf install kibana -y
```
![[Pasted image 20250521140125.png]]

**III.** Generate the kibana encryption Key using the command below, then copy them and save them cause you will need to set in the kibana.yml file.
```bash
cd /usr/share/kibana/bin

./kibana-encryption-keys generate
```
![[Pasted image 20250521140502.png]]

**IV.** Modify the lines below in the configuration file for kibana
```bash
nano /etc/kibana/kibana.yml
```
uncomment the lines below and edit accordingly
```bash
server.host: “0.0.0.0”

server.port: 5601

server.publicBaseUrl: “http://172.16.3.16:5601/"

xpack.encryptedSavedObjects.encryptionKey: 47b185340292ac6a50110ace1055bb71
xpack.reporting.encryptionKey: 85d1e580fcc68192d743250449e24000
xpack.security.encryptionKey: fa4a33d3d67740a07887a373d32c55c0
```
![[Pasted image 20250521141450.png]]
**V.** Launch and enable Kibana
```bash
systemctl daemon-reload  
systemctl start kibana  
systemctl enable kibana  
systemctl status kibana
```
![[Pasted image 20250521141932.png]]

**VI.** Confirm that you can access Kibana’s web interface from another computer (allow traffic on TCP port 5601):
```bash
firewall-cmd --add-port=5601/tcp

firewall-cmd --zone=public --permanent --add-port=5601/tcp

firewall-cmd --reload
```
![[Pasted image 20250521142353.png]]

**VII**. You need to generate the enrollment token inorder to configure the elastic server.
```bash
cd /usr/share/elasticsearch/bin

./elasticsearch-create-enrollment-token --scope kibana
```
![[Pasted image 20250521142619.png]]

**VIII.** Generate the verification code
```bash
/usr/share/kibana/bin/kibana-verification-code
```

**IX.** Open Kibana to ensure that you can access the web interface:
```bash
http://172.16.3.16:5601
```

**X.** You will then need to enter the generated enrolment token in the steps above. Then click **Configure Elastic**
![[Pasted image 20250521142938.png]]

![[Pasted image 20250521143305.png]]

**XI.** The next page will require the verification code so I will get this code from the script initialized above. Then Click **Verify**
![[Pasted image 20250521143355.png]]

**XII.** The initialization of Kibana with Elasticsearch server will start as shown in the screenshot below
![[Pasted image 20250521143431.png]]

**XIII.** When the setup completes, you will get the login page. From the login page enter the need credentials (username and password) and click **Login**
![[Pasted image 20250521143510.png]]

**XIV.** Click **Explore on my own** go to welcome page
![[Pasted image 20250521144111.png]]
![[Pasted image 20250521144247.png]]
##### **How to configure send metrics postgresql to elasticsearch with metricbeat module (postgresql.yml)**

**Note:** case study and command for this project

**Link reference:**
	-  [PostgreSQL module cannot connect](https://discuss.elastic.co/t/postgresql-module-cannot-connect/220916)
	-  [Postgresql module](https://www.elastic.co/docs/reference/beats/metricbeat/metricbeat-module-postgresql)
	- [Metricbeat PostgreSQL Dashboard](https://dbtut.com/index.php/2021/02/04/metricbeat-postgresql-dashboard/)

- enable postgre in metricbeat 
```bash
sudo metricbeat modules enable postgresql
sudo metricbeat modules disable system
```

- restart metricbeat
```bash
sudo systemctl status metricbeat -l
sudo systemctl restart metricbeat
```

- login postgresql with port change
```bash
psql -U postgres -p 51173 "login with port 51173 default port 5432"

\q "exit login postgresql"

```

- check permission on database postgresql **(alredy login superuser)**
```bash
\dp pg_stat_activity "check on ACTIVITY "

\dp pg_stat_database "check on DATABASE "

\dp pg_stat_bgwriter "check on BGWRITER "
```

- check database already create in postgresql
```bash
SELECT datname FROM pg_database WHERE datistemplate = false; "database"
```

- check port open 
```bash
netstat -tulnp
```

- test configure
```bash
sudo metricbeat test config
```

- check version postgresql
```bash
postgres --version
```

- allow port on firewall oracle linux server
```bash
sudo firewall-cmd --add-port=5432/tcp --permanent

sudo firewall-cmd --add-port=51173/tcp --permanent

sudo firewall-cmd --reload

sudo firewall-cmd --list-all

sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="172.16.3.12/24" port port="5432" protocol="tcp" accept'

sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="172.16.3.12/24" port port="51173" protocol="tcp" accept'
```

###### **Step by step configure**

**I.**  Enable metricbeat postgresql module on vm running postgresql.

```bash
sudo metricbeat modules enable postgresql
```
![[Pasted image 20250519045735.png]]

**II.**  Configure information necessary for connect among metricbeat->postgresql->elasticsearch.

```bash
- module: postgresql
  metricsets:
	 - database
	 - bgwriter
	 - activity
	 - statement
  period: 10s
  hosts: ["postgres://172.16.3.12:51173?sslmode=disable"]
  username: postgres
  password: HrWL(f#vXnrIWNkL
```
![[Pasted image 20250519045459.png]]

**III.**  Create statements extension on postgresql serve for information collection send to elasticsearch and show on dashboard in kibana. [link](https://stackoverflow.com/questions/57834690/how-to-retrieve-statement-metrics-from-postgres-using-metricbeat)

```bash
create extension pg_stat_statements; "create extension, alredy login success postgresql"
```
![[Pasted image 20250519111648.png]]

- Add information to file nano /data/pg/postgresql.conf (restart postgresql require)
```bash
shared_preload_libraries = 'pg_stat_statements'
pg_stat_statements.max = 10000
pg_stat_statements.track = all 
```

```bash
systemctl restart postgresql-14 "restart postgresql server after configure done"
```
![[Pasted image 20250519165757.png]]


###### - **Install postgresql on oracle linux 8 (only serve for test collection metrics via postgresql module with extension pg_stat_statements)**: [link](https://www.atlantic.net/dedicated-server-hosting/how-to-install-and-secure-postgresql-server-on-oracle-linux/)
```bash
CREATE USER postgresql WITH CREATEDB CREATEROLE PASSWORD 'Dbiz@#2025';
```

```bash
CREATE DATABASE testdb OWNER postgresql;
```

**case study:**
- fix error uncreate extension "pg_stat_statements" 
```bash
sudo yum install postgresql14-contrib ""
```
![[Pasted image 20250519163559.png]]
![[Pasted image 20250519163825.png]]

- fix error 
```bash
QueryStats: failed to obtain a connection with the database: pq: no pg_hba.conf entry for host "172.16.3.16", user "postgres", database "postgres", no encryption
```
![[Pasted image 20250519170415.png]]

- fix error connection refused
![[Pasted image 20250519172211.png]]
![[Pasted image 20250519171940.png]]

##### **How to show metric database of postgresql running on database testdb in vm monitornew**

- show database testdb ?
![[Pasted image 20250520113123.png]]
- show user postgresql ?
- why don't see any thing metrics bgwriter of module postgresql 
  => result: already start and show name in fields metricset.name
![[Screenshot 2025-05-20 at 11.26.06.png]]

##### **How to change kibana logo to other logo**

Refer: [link](https://stackoverflow.com/questions/67831737/how-can-i-change-all-the-kibana-logos-to-other-logos)

##### **How to Backup elasticsearch & kibanna**
	- Snapshot ? [link](https://www.elastic.co/docs/deploy-manage/tools/snapshot-and-restore)
	

##### **Send email alerts via google & ms exchange 365**
	- send email via google email with logstash ?

##### **How to downgrade logstash 9 to 8 version on vm monitor ?**
