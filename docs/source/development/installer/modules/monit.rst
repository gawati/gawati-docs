monit
#####

General
*******

The goal of the module is to install and configure monit monitoring service for
Gawati and the server its running on.

Software Packages
*****************

monit will be installed

Configuration
*************

Monit will be configured to start on boot, monitoring items will be configured
in /etc/monit.d , logfiles created and a monitoring target file created in Apache
webroot to provide a test URL

ini Variables
*************

options
=======

Define the monitoring targets to activate::

  options=apache,base,chrony,email,eXist-st,fail2ban,sshd,startup,system,webinterface

Define a notification email address to receive alerts::

  mailrecipient=root@my.gawati.local

Postinstallers
**************

No postinstaller available.
