+++
title = "Avoids ssh timeouts"
date = "2012-11-21T16:17:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["linux"]
description = ""
+++


```sh
# to set it globally
echo 'ServerAliveInterval 60' >> /etc/ssh/ssh_config

# For per-user configuration
echo 'ServerAliveInterval 60' >> ~/.ssh/config
```
<!--more-->