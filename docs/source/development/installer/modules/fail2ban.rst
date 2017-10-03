fail2ban
########

General
*******

The goal of the module is to protect the system from some attacks that can be
detected by notifications in logfiles.

Software Packages
*****************

"iptables" and "fail2ban" will be installed.

Configuration
*************

The reactive measures are configured in /etc/fail2ban/jail.local
By default we monitor login failures on ssh.
Banning will be executed by temporarily blocking all incoming traffic from
originating IPs.
A new shell script "offenders" is added to the system listing the current status
of banning.

ini Variables
*************

For email notifications, you need to configure a senders email address in
"mailsender" and a recipient address in "mailrecipient" respectively.

Postinstaller
*************

None
