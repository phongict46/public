---
Status: true
New field: 2025-06-22T21:54
Stage: Source
---

##### **How to configure send metrics postgresql to elasticsearch with metricbeat module (postgresql.yml)**

**Note:** case study and command for this project.

**Link reference:**
	-  [PostgreSQL module cannot connect](https://discuss.elastic.co/t/postgresql-module-cannot-connect/220916)
	-  [Postgresql module](https://www.elastic.co/docs/reference/beats/metricbeat/metricbeat-module-postgresql)
	- [Metricbeat PostgreSQL Dashboard](https://dbtut.com/index.php/2021/02/04/metricbeat-postgresql-dashboard/)

- enable postgre in metricbeat.
```bash
sudo metricbeat modules enable postgresql
sudo metricbeat modules disable system
```

- restart metricbeat.
```bash
sudo systemctl status metricbeat -l
sudo systemctl restart metricbeat
```

- login postgresql with port change.
```bash
psql -U postgres -p 51173 "login with port 51173 default port 5432"

\q "exit login postgresql"

```

- check permission on database postgresql **(alredy login superuser)**.
```bash
\dp pg_stat_activity "check on ACTIVITY "

\dp pg_stat_database "check on DATABASE "

\dp pg_stat_bgwriter "check on BGWRITER "
```

- check database already create in postgresql.
```bash
SELECT datname FROM pg_database WHERE datistemplate = false; "database"
```

- check port open.
```bash
netstat -tulnp
```

- test configure metricbeat after configure.
```bash
sudo metricbeat test config
```

- check version postgresql.
```bash
psql --version 'run in terminal orcale linux'
```

- allow port on firewall oracle linux server.
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
