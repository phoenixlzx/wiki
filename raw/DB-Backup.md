title: Database backup
category: Database
---
#### MySQL

##### All databases
```
mysqldump --user=$USER --password=$PASSWORD -A > /path/to/dumpfile.sql
```

##### Individual database
```
mysqldump --user=$USER --password=$PASSWORD --databases DB_NAME1 DB_NAME2 DB_NAME3 > /path/to/dumpfile.sql
```

##### Restore MySQL
```
mysql --verbose --user=$USER --password=$PASSWORD DB_NAME < /path/to/dumpfile.sql
```

##### Restore ALL MySQL databases
```
mysql --verbose --user=$USER --password=$PASSWORD < /path/to/dumpfile.sql
```

#### MongoDB

##### Backup MongoDB
```
mongodump --dbpath /data/db/ --db test --out /data/backup/
```

##### Restore MongoDB
```
mongorestore --dbpath <database path> <path to the backup>
```

##### More with MongoDB
```
http://docs.mongodb.org/manual/tutorial/backup-databases-with-binary-database-dumps/
```

Reference:

* [FelixWiki](https://felixc.at)
