---
layout: post
title:  "How to run homebridge in the background"
date:   2019-04-20 12:15:40 -0700
categories: side-projects
tags: []
---

A no BS guide to running homebridge in the background on a unix system (requires systemd). _Works 70% of the time every time._ 

Add user to run homebridge under  
`sudo useradd --system homebridge`

Create directory for it  
`sudo mkdir /var/lib/homebridge`

Own the directory permissions  
`sudo chown -R homebridge:homebridge /var/lib/homebridge` 
`sudo chmod 777 -R /var/lib/homebridge`

Copy your home directory's config (if you don't have one already, edit it instead)  
`sudo cp ~/.homebridge/config.json /var/lib/homebridge/config.json`


Copy your home directory's persist directory (if it exists)
`sudo cp -R ~/.homebridge/persist /var/lib/homebridge/persist`

Determine where homebridge is aliased  
`which homebridge`

Edit the systemd service  
`sudo nano /etc/systemd/system/homebridge.service`

Paste in the contents of `homebridge.service` from [here](https://gist.github.com/johannrichard/0ad0de1feb6adb9eb61a/). Make sure to replace `/usr/local/bin/homebridge` with where homebridge is actually installed.

Exit nano  
`control + x, Y`

`sudo nano /etc/default/homebridge`   
Paste in the contents of `homebridge` from [here](https://gist.github.com/johannrichard/0ad0de1feb6adb9eb61a/).

Reload systemd configs  
`sudo systemctl daemon-reload`

Enable and start the service  
`sudo systemctl enable homebridge`  
`sudo systemctl start homebridge`

Check on its status  
`sudo systemctl status homebridge`

If something is wrong, check the logs.  
`journalctl -u homebridge`

At this point if there are any issues, live vicariously via google and stackoverflow. 

🏝