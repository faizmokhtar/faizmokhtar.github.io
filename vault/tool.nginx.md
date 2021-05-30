---
id: 33d0f188-ab41-4acd-959b-d2fdbb31f546
title: Nginx
desc: ''
updated: 1621697878931
created: 1621582650559
---

# What it is?
- basically a web server that's now famously used for reverse proxy, HTTP cache, load balancing and among other else

## Basic commands

```bash
nginx -s $COMMAND 
```
- Replace `$COMMAND` with either of the following
```bash
stop # fast shutdown
quit # graceful shutdown
reload # reloading the configuration file
reopen # reopening the log files
```