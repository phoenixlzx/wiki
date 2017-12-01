title: InfluxDB.md
category: Server Configuration
time: 1512110004389
---

## Enable HTTP Auth

File `/etc/influxdb/influxdb.conf`

```
[http]  
  enabled = true  
  bind-address = ":8086"  
  auth-enabled = true  # change to true
  log-enabled = true  
  write-tracing = false  
  pprof-enabled = false  
  https-enabled = false  
  https-certificate = "/etc/ssl/influxdb.pem"  
```

Create an Admin user before restart the process.

## Create Users

#### Admin user

```
CREATE USER <username> WITH PASSWORD '<password>' WITH ALL PRIVILEGES
```

#### Normal user

```
CREATE USER <username> WITH PASSWORD '<password>'
```

## Privilege Management

```
GRANT ALL PRIVILEGES TO "<user>"
```

```
REVOKE ALL PRIVILEGES FROM "<user>"
```

```
GRANT [READ,WRITE,ALL] ON <database_name> TO <username>
```

```
REVOKE [READ,WRITE,ALL] ON <database_name> FROM <username>
```

## User Management

```
SHOW GRANTS FOR "<user>"
```

```
SET PASSWORD FOR <username> = '<password>'
```

```
DROP USER <username>
```

## Data Retention

Create retention policy

```
CREATE RETENTION POLICY <retention_policy_name> ON <database_name> DURATION <duration> REPLICATION <n> [DEFAULT]
```

Modify retention policy

```
ALTER RETENTION POLICY <retention_policy_name> ON <database_name> DURATION <duration> REPLICATION <n> [DEFAULT]
```

`DURATION` determines how long InfluxDB keeps the data - the options for specifying the duration of the retention policy are listed below. Note that the minimum retention period is one hour.

* `m` minutes
* `h` hours
* `d` days
* `w` weeks
* `INF` infinite

`REPLICATION` determines how many independent copies of each point are stored in the cluster, where n is the number of data nodes.

`DEFAULT` sets the new retention policy as the default retention policy for the database.

Reference:

1. [InfluxDB: Authentication and Authorization](https://docs.influxdata.com/influxdb/v1.3/query_language/authentication_and_authorization/)

