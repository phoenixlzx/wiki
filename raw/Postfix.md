title: Postfix
category: Server Configuration
---
#### Postfix rate limiting

Add the following lines to `/etc/postfix/main.cf`

```
smtp_destination_concurrency_limit = 2
smtp_destination_rate_delay = 1s
smtp_extra_recipient_limit = 10
```

Reference:

1. [Steam.io](http://steam.io/2013/04/01/postfix-rate-limiting/)