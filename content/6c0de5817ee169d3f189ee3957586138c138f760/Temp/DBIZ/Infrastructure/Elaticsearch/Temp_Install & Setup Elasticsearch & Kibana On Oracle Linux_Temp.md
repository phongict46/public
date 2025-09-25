##### **Install Elasticsearch**
- Link web site: [elastic.io](https://www.elastic.co/docs/deploy-manage/deploy/self-managed/install-elasticsearch-with-rpm) 
- Link youtube: [Elastic Stack Tutorials](https://www.youtube.com/watch?v=kkrLanotz1I&list=PLe0UsEbF-5meLIZJaAOF5QDKey-v1PEvw) 

**Note:**
![[Pasted image 20250507055234.png]] Password auto create install elasticsearch
/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
QwB2DU+6o69ef79e8ZV0

- pass elasticsearch on server monitor dbiz
- The generated password for the elastic built-in superuser is :
```hash
 h8qdVHVbPSvf-ci632BF
```

- new password reset 
```bash
/usr/share/elasticsearch/bin/elasticsearch-reset-password -i -u elastic

Dbiz@#2025 "new password"
```

- Run test 
  ```bash
  curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic https://localhost:9200
```
- Create account **Monitor** on oracle linux server, download all file and run in directory **U1** (Mr.Minh share)

- ./kibana-encryption-keys generate
```bash
  Settings:

xpack.encryptedSavedObjects.encryptionKey: e31531107962081447778295524b38c7

xpack.reporting.encryptionKey: aea00b6154de5c6f3c2c4443e1c19e77

xpack.security.encryptionKey: d6cf3ccc8bdceadecf7c24d52d17573b
```
- ![[Screenshot 2025-05-07 at 23.07.31.png]]

![[Screenshot 2025-05-07 at 23.07.41.png]]

##### **Information on server monitornew**

- The generated password for the elastic built-in superuser is : ebqkq4gwWDGVBeQPH4SR
- Settings:
xpack.encryptedSavedObjects.encryptionKey: aea5f107ac1b3f58de8a8c027a7a8133

xpack.reporting.encryptionKey: ee36595a00c486ab646d37e825882246

xpack.security.encryptionKey: 8926f2024b4fdc8aad85defe4c8f046b


##### **Information file kibana.yml on dbiz cmc cloud**
```bash
[root@dbizoilapp /]# cat /u01/Kibana/kibana-8.1.3/config/kibana.yml

# For more configuration options see the configuration guide for Kibana in

# https://www.elastic.co/guide/index.html

  

# =================== System: Kibana Server ===================

# Kibana is served by a back end server. This setting specifies the port to use.

server.port: 5601

  

# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.

# The default is 'localhost', which usually means remote machines will not be able to connect.

# To allow connections from remote users, set this parameter to a non-loopback address.

server.host: "0.0.0.0"

  

# Enables you to specify a path to mount Kibana at if you are running behind a proxy.

# Use the `server.rewriteBasePath` setting to tell Kibana if it should remove the basePath

# from requests it receives, and to prevent a deprecation warning at startup.

# This setting cannot end in a slash.

#server.basePath: "/kibana"

  

# Specifies whether Kibana should rewrite requests that are prefixed with

# `server.basePath` or require that they are rewritten by your reverse proxy.

# Defaults to `false`.

server.rewriteBasePath: false

  

# Specifies the public URL at which Kibana is available for end users. If

# `server.basePath` is configured this URL should end with the same basePath.

#server.publicBaseUrl: "http://identity.digitalbiz.com.vn:5601/kibana"

  

# The maximum payload size in bytes for incoming server requests.

#server.maxPayload: 1048576

  

# The Kibana server's name. This is used for display purposes.

server.name: "identity.digitalbiz.com.vn"

  

# =================== System: Kibana Server (Optional) ===================

# Enables SSL and paths to the PEM-format SSL certificate and SSL key files, respectively.

# These settings enable SSL for outgoing requests from the Kibana server to the browser.

server.ssl.enabled: true

server.ssl.certificate: /u01/Elastic/elasticsearch-8.1.3/cert/ca/digitalbiz_com_vn.crt

server.ssl.key: /u01/Elastic/elasticsearch-8.1.3/cert/ca/digitalbiz_com_vn.key

  

# =================== System: Elasticsearch ===================

# The URLs of the Elasticsearch instances to use for all your queries.

elasticsearch.hosts: ["http://172.16.1.107:9200"]

  

# If your Elasticsearch is protected with basic authentication, these settings provide

# the username and password that the Kibana server uses to perform maintenance on the Kibana

# index at startup. Your Kibana users still need to authenticate with Elasticsearch, which

# is proxied through the Kibana server.

#elasticsearch.username: "kibana_system"

#elasticsearch.password: "_ZyEfMQ-QE7LTeu=0xuC"

  

# Kibana can also authenticate to Elasticsearch via "service account tokens".

# Service account tokens are Bearer style tokens that replace the traditional username/password based configuration.

# Use this token instead of a username/password.

elasticsearch.serviceAccountToken: "eyJ2ZXIiOiI4LjEuMyIsImFkciI6WyIxNzIuMTYuMS4xMDc6OTIwMCJdLCJmZ3IiOiIwYWM4MmI1ODMwYTkzYTNhMWUyNzQ4NzIyNjVkM2E4Nzc4ODlmNTMyNzlkMjgyY2JlNzZmMWFlNDZiYjFiODQxIiwia2V5IjoiVEZjcHFvd0I2cC1Yc3ZCbEZhRmI6SXBVaVZ6UzRSZDZheUVkb0J5aUNiQSJ9"

  

# Time in milliseconds to wait for Elasticsearch to respond to pings. Defaults to the value of

# the elasticsearch.requestTimeout setting.

#elasticsearch.pingTimeout: 1500

  

# Time in milliseconds to wait for responses from the back end or Elasticsearch. This value

# must be a positive integer.

#elasticsearch.requestTimeout: 30000

  

# Specifies whether Kibana should use compression for communications with elasticsearch

# Defaults to `false`.

#elasticsearch.compression: false

  

# List of Kibana client-side headers to send to Elasticsearch. To send *no* client-side

# headers, set this value to [] (an empty list).

#elasticsearch.requestHeadersWhitelist: [ authorization ]

  

# Header names and values that are sent to Elasticsearch. Any custom headers cannot be overwritten

# by client-side headers, regardless of the elasticsearch.requestHeadersWhitelist configuration.

#elasticsearch.customHeaders: {}

  

# Time in milliseconds for Elasticsearch to wait for responses from shards. Set to 0 to disable.

#elasticsearch.shardTimeout: 30000

  

# =================== System: Elasticsearch (Optional) ===================

# These files are used to verify the identity of Kibana to Elasticsearch and are required when

# xpack.security.http.ssl.client_authentication in Elasticsearch is set to required.

#elasticsearch.ssl.certificate: /path/to/your/client.crt

#elasticsearch.ssl.key: /path/to/your/client.key

  

# Enables you to specify a path to the PEM file for the certificate

# authority for your Elasticsearch instance.

#elasticsearch.ssl.certificateAuthorities: [ "/path/to/your/CA.pem" ]

  

# To disregard the validity of SSL certificates, change this setting's value to 'none'. defaul: full

elasticsearch.ssl.verificationMode: none

  

# =================== System: Logging ===================

# Set the value of this setting to off to suppress all logging output, or to debug to log everything. Defaults to 'info'

#logging.root.level: debug

  

# Enables you to specify a file where Kibana stores log output.

#logging.appenders.default:

#  type: file

#  fileName: /var/logs/kibana.log

#  layout:

#    type: json

  

# Logs queries sent to Elasticsearch.

#logging.loggers:

#  - name: elasticsearch.query

#    level: debug

  

# Logs http responses.

#logging.loggers:

#  - name: http.server.response

#    level: debug

  

# Logs system usage information.

#logging.loggers:

#  - name: metrics.ops

#    level: debug

  

# =================== System: Other ===================

# The path where Kibana stores persistent data not saved in Elasticsearch. Defaults to data

#path.data: data

  

# Specifies the path where Kibana creates the process ID file.

#pid.file: /run/kibana/kibana.pid

  

# Set the interval in milliseconds to sample system and process performance

# metrics. Minimum is 100ms. Defaults to 5000ms.

#ops.interval: 5000

  

# Specifies locale to be used for all localizable strings, dates and number formats.

# Supported languages are the following: English - en , by default , Chinese - zh-CN .

#i18n.locale: "en"

  

# =================== Frequently used (Optional)===================

  

# =================== Saved Objects: Migrations ===================

# Saved object migrations run at startup. If you run into migration-related issues, you might need to adjust these settings.

  

# The number of documents migrated at a time.

# If Kibana can't start up or upgrade due to an Elasticsearch `circuit_breaking_exception`,

# use a smaller batchSize value to reduce the memory pressure. Defaults to 1000 objects per batch.

#migrations.batchSize: 1000

  

# The maximum payload size for indexing batches of upgraded saved objects.

# To avoid migrations failing due to a 413 Request Entity Too Large response from Elasticsearch.

# This value should be lower than or equal to your Elasticsearch clusterâ€™s `http.max_content_length`

# configuration option. Default: 100mb

#migrations.maxBatchSizeBytes: 100mb

  

# The number of times to retry temporary migration failures. Increase the setting

# if migrations fail frequently with a message such as `Unable to complete the [...] step after

# 15 attempts, terminating`. Defaults to 15

#migrations.retryAttempts: 15

  

# =================== Search Autocomplete ===================

# Time in milliseconds to wait for autocomplete suggestions from Elasticsearch.

# This value must be a whole number greater than zero. Defaults to 1000ms

#data.autocomplete.valueSuggestions.timeout: 1000

  

# Maximum number of documents loaded by each shard to generate autocomplete suggestions.

# This value must be a whole number greater than zero. Defaults to 100_000

#data.autocomplete.valueSuggestions.terminateAfter: 100000
```

##### **Information file elasticsearch.yml on dbiz cloud cmc**
- /u01/Elastic/elasticsearch-8.1.3/config/elasticsearch.yml
```bash
[root@dbizoilapp /]# cat /u01/Elastic/elasticsearch-8.1.3/config/elasticsearch.yml

# ======================== Elasticsearch Configuration =========================

#

# NOTE: Elasticsearch comes with reasonable defaults for most settings.

#       Before you set out to tweak and tune the configuration, make sure you

#       understand what are you trying to accomplish and the consequences.

#

# The primary way of configuring a node is via this file. This template lists

# the most important settings you may want to configure for a production cluster.

#

# Please consult the documentation for further information on configuration options:

# https://www.elastic.co/guide/en/elasticsearch/reference/index.html

#

# ---------------------------------- Cluster -----------------------------------

#username: elastic

#password: SvxxBxehjfs$

#

# Use a descriptive name for your cluster:

#

#cluster.name: my-application

#

# ------------------------------------ Node ------------------------------------

#

# Use a descriptive name for the node:

#

#node.name: node-1

#

# Add custom attributes to the node:

#

#node.attr.rack: r1

#

# ----------------------------------- Paths ------------------------------------

#

# Path to directory where to store the data (separate multiple locations by comma):

#

path.data: /u01/Elastic/elasticsearch-8.1.3/data

#

# Path to log files:

#

path.logs: /u01/Elastic/elasticsearch-8.1.3/logs

#

# ----------------------------------- Memory -----------------------------------

#

# Lock the memory on startup:

#

#bootstrap.memory_lock: true

#

# Make sure that the heap size is set to about half the memory available

# on the system and that the owner of the process is allowed to use this

# limit.

#

# Elasticsearch performs poorly when the system is swapping the memory.

#

# ---------------------------------- Network -----------------------------------

#

# By default Elasticsearch is only accessible on localhost. Set a different

# address here to expose this node on the network:

#

#network.host: 172.16.1.107

#

# By default Elasticsearch listens for HTTP traffic on the first free port it

# finds starting at 9200. Set a specific HTTP port here:

#

#http.port: 9200

#

# For more information, consult the network module documentation.

#

# --------------------------------- Discovery ----------------------------------

#

# Pass an initial list of hosts to perform discovery when this node is started:

# The default list of hosts is ["127.0.0.1", "[::1]"]

#

#discovery.seed_hosts: ["host1", "host2"]

#

# Bootstrap the cluster using an initial set of master-eligible nodes:

#

#cluster.initial_master_nodes: ["node-1", "node-2"]

#

# For more information, consult the discovery and cluster formation module documentation.

#

# ---------------------------------- Various -----------------------------------

#

# Allow wildcard deletion of indices:

#

#action.destructive_requires_name: false

  

#----------------------- BEGIN SECURITY AUTO CONFIGURATION -----------------------

#

# The following settings, TLS certificates, and keys have been automatically

# generated to configure Elasticsearch security features on 12-10-2023 02:33:17

#

# --------------------------------------------------------------------------------

  

# Enable security features

xpack.security.enabled: no

  

xpack.security.enrollment.enabled: true

  

# Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents

xpack.security.http.ssl:

  enabled: true

  keystore.path: certs/http.p12

  

# Enable encryption and mutual authentication between cluster nodes

xpack.security.transport.ssl:

  enabled: true

  verification_mode: certificate

  keystore.path: certs/transport.p12

  truststore.path: certs/transport.p12

# Create a new cluster with the current node only

# Additional nodes can still join the cluster later

cluster.initial_master_nodes: ["dbizoilapp.ssg.vn"]

  

# Allow HTTP API connections from localhost and local networks

# Connections are encrypted and require user authentication

http.host: [_local_, _site_]

  

# Allow other nodes to join the cluster from localhost and local networks

# Connections are encrypted and mutually authenticated

#transport.host: [_local_, _site_]

  

action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*

  

#----------------------- END SECURITY AUTO CONFIGURATION ------------------------
```

- ./u01/Elastic/elasticsearch-7.16.3/config/elasticsearch.yml
```bash
[root@dbizoilapp /]# cat ./u01/Elastic/elasticsearch-7.16.3/config/elasticsearch.yml

# ======================== Elasticsearch Configuration =========================

#

# NOTE: Elasticsearch comes with reasonable defaults for most settings.

#       Before you set out to tweak and tune the configuration, make sure you

#       understand what are you trying to accomplish and the consequences.

#

# The primary way of configuring a node is via this file. This template lists

# the most important settings you may want to configure for a production cluster.

#

# Please consult the documentation for further information on configuration options:

# https://www.elastic.co/guide/en/elasticsearch/reference/index.html

#

# ---------------------------------- Cluster -----------------------------------

#

# Use a descriptive name for your cluster:

#

cluster.name: dbiz-prod-elastic

#

# ------------------------------------ Node ------------------------------------

#

# Use a descriptive name for the node:

#

node.name: node-1

#

# Add custom attributes to the node:

#

#node.attr.rack: r1

#

# ----------------------------------- Paths ------------------------------------

#

# Path to directory where to store the data (separate multiple locations by comma):

#

#path.data: /path/to/data

#

# Path to log files:

#

#path.logs: /path/to/logs

#

# ----------------------------------- Memory -----------------------------------

#

# Lock the memory on startup:

#

#bootstrap.memory_lock: true

#

# Make sure that the heap size is set to about half the memory available

# on the system and that the owner of the process is allowed to use this

# limit.

#

# Elasticsearch performs poorly when the system is swapping the memory.

#

# ---------------------------------- Network -----------------------------------

#

# By default Elasticsearch is only accessible on localhost. Set a different

# address here to expose this node on the network:

#

network.host: 0.0.0.0

#

# By default Elasticsearch listens for HTTP traffic on the first free port it

# finds starting at 9200. Set a specific HTTP port here:

#

http.port: 9200

transport.host: localhost

transport.tcp.port: 9300

#

# For more information, consult the network module documentation.

#

# --------------------------------- Discovery ----------------------------------

#

# Pass an initial list of hosts to perform discovery when this node is started:

# The default list of hosts is ["127.0.0.1", "[::1]"]

#

#discovery.seed_hosts: ["host1", "host2"]

#

# Bootstrap the cluster using an initial set of master-eligible nodes:

#

cluster.initial_master_nodes: ["node-1"]

#

# For more information, consult the discovery and cluster formation module documentation.

#

# ---------------------------------- Various -----------------------------------

#

# Require explicit names when deleting indices:

#

#action.destructive_requires_name: true

#

# ---------------------------------- Security ----------------------------------

#

#                                 *** WARNING ***

#

# Elasticsearch security features are not enabled by default.

# These features are free, but require configuration changes to enable them.

# This means that users don’t have to provide credentials and can get full access

# to the cluster. Network connections are also not encrypted.

#

# To protect your data, we strongly encourage you to enable the Elasticsearch security features.

# Refer to the following documentation for instructions.

#

# https://www.elastic.co/guide/en/elasticsearch/reference/7.16/configuring-stack-security.html
```

cd /usr/share/metricbeat/bin/

./metricbeat test config -c /etc/metricbeat/metricbeat.yml --path.home /usr/share/metricbeat --path.data /var/lib/metricbeat

./metricbeat test output -c /etc/metricbeat/metricbeat.yml --path.home /usr/share/metricbeat --path.data /var/lib/metricbeat
```
input {
beats {
port => 5044
}
}
filter {
if [type] == “syslog” {
grok {
match => { “message” => “%{SYSLOGLINE}” }
}
date {
match => [ “timestamp”, “MMM d HH:mm:ss”, “MMM dd HH:mm:ss” ]
}
}
}
output {
elasticsearch {
hosts => [“https://172.16.3.3:9200"]
index => “%{[@metadata][beat]}-%{+YYYY.MM.dd}”
user => “elastic”
password => “Dbiz@#2025”
ssl_certificate_verification => false
}
}
```

sudo useradd vncuserdbiz
sudo passwd Dbiz2025
[link youtube](https://www.youtube.com/watch?v=ws1J9nKpAKc)


openssl s_client -connect 172.16.3.3.:443 -showcerts
openssl x509 -fingerprint -sha256 -in /etc/elasticsearch/certs/http_ca. crt

ce:b7:dd:3d:80:a5:0d:5c:52:50:1f:e8:5d:d4:3d:a3:3c:b9:22:87:01:17:05:6f:af:6f:fd:b5:22:fa:72:9e

ssl:
	enabled: true
	ca_trusted_fingerprint: "ceb7dd3d80a50d5c52501fe85dd43da33cb922870117056faf6ffdb522fa729e"

#### - **Research function manage apps and code** ![[Pasted image 20250510222139.png]]
  

**[HOW TO INSTALL LOGSTASH V8.10.2](https://medium.com/@yago82/comprehensive-guide-to-installing-and-configuring-logstash-on-linux-servers-3a10a14b77e8)**
- API KEY FOR LOGSTASH: 
  ```bash
  Np0pzpYBV9ML_3tQ6U7o:hOl3kTYYYUVU0a9DM8Dm9A
	```
![[Pasted image 20250514164331.png]]

password email app "phongict46@gmail.com"
```bash
utqi splb vapl qhet
```