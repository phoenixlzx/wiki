title: MySQL
category: Server Configuration
time: 1476133839
---

### InnoDB log flush once per second to increase speed

Add to `my.cnf`

```
innodb_flush_log_at_trx_commit = 0
```

**Note** This will cause MySQL to flush log once per second, and IF MySQL crashed, transactions of last second will be erased.

Reference:

* [StackExchange DBA](http://dba.stackexchange.com/questions/12611/is-it-safe-to-use-innodb-flush-log-at-trx-commit-2)

