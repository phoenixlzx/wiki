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

Reference:

1. [InfluxDB: Authentication and Authorization](https://docs.influxdata.com/influxdb/v1.3/query_language/authentication_and_authorization/)

