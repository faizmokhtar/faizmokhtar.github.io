---
id: 0130b156-5d8b-41ce-aeda-cef1ac586691
title: Fail2ban
desc: ''
updated: 1621319925384
created: 1621318193311
---

# What it is?
- Intrusion prevention framework to prevent brute-force attack
- Use to ban suscpicious inbound IP

# To install on macOS
```bash
brew install fail2ban
# To have launchd start fail2ban now and restart at startup:
sudo brew services start fail2ban
```

# Commands

```bash
# start fail2ban
sudo fail2ban-client start
# stop fail2ban
sudo fail2ban-client stop
# check status
sudo fail2ban-client status
```