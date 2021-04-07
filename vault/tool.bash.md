---
id: 170dd2d7-f902-4e34-9881-55b55d05358a
title: Bash
desc: ''
updated: 1617409496828
created: 1617408892783
---

# Bash Cookbook

## To redirect output and error messages to different files

```
myprogram 1> message.out 2> message.err
```

## To throw output away

Use `/dev/null`. Useful when you don't wat to save the output. Eg;

```
noisy > /dev/null
```