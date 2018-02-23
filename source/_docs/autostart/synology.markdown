---
layout: page
title: "Autostart on Synology NAS boot"
description: "Instructions how to setup Home Assistant to launch on boot on Synology NAS."
date: 2015-9-1 22:57
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /getting-started/autostart-synology/
---

To get Home Assistant to automatically start when you boot your Synology NAS:

SSH into your synology & login as admin or root

```bash
$ cd /volume1/homeassistant
```

Create "homeassistant.conf" file using the following code

```bash
# only start this service after the httpd user process has started
start on started httpd-user

# stop the service gracefully if the runlevel changes to 'reboot'
stop on runlevel [06]

# run the scripts as the 'http' user. Running as root (the default) is a bad ide
#setuid admin

# exec the process. Use fully formed path names so that there is no reliance on
# the 'www' file is a node.js script which starts the foobar application.
exec /bin/sh /volume1/homeassistant/hass-daemon start
```

Register the autostart

```bash
$ ln -s homeassistant.conf /etc/init/homeassistant.conf
```

Make the relevant files executable:

```bash
$ chmod -r 777 /etc/init/homeassistant.conf
```

That's it - reboot your NAS and Home Assistant should automatically start

===
If the above doesn't work for you, try:

Open the DSM web interface
Open the Text Editor (if this package is not yet installed, install it) and open a new file
Paste the following: exec /bin/sh /volume1/homeassistant/hass-daemon start
Save the file in the homeassistant folder as StartAtBoot.sh or whatever you would like
Open the DSM task scheduler from the Control Panel
Create a new trigged task > user defined script and make sure the event is set to boot, ran as root, and point it to /volume1/homeassistant/StartAtBoot.sh
ssh into the NAS and cd to /volume1/homeassistant/ and run sudo chmod +x StartAtBoot.sh to make sure it is executable
You can now reboot the NAS and it should autostart just fine!
