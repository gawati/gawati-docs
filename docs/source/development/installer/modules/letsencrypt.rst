httpd
######

General
*******

The goal of the module is to prepare the OS and install the tools for using
letsencrypt certificate services. Using a postinstaller, the module provides
SSL keys officially signed by letsencrypt ready for use with a local apache
websever. To maintain the validity of the certificate the postinstaller creates
a cronjob to renew the certificate when needed.

Software Packages
*****************

"epel-release" will be installed to enable use of the EPEL repository.
"acme-tiny" will be installed for accessing the letsencrypt service.

Configuration
*************

No configurations applied

ini Variables
*************

The main module for installation of the acme tools, do not use varibles.
postinstall=setupcerts
certs=my.gawati.org


Postinstallers
**************

1 postinstaller available called "setupcerts" to create key pairs and retrieve
certificates from letsencrypt. "certs" specifies a comma separated list of
DNSnames for which certificates shall be retrieved. These names must reach the
local apache webserver on port 80 through public DNS to be successful.

Example::

  postinstall=setupcerts
  certs=my.gawati.org


Details
*******

The letsencrypt verification folder structure will be created at
"/var/www/challenges/.well-known/acme-challenge"
Keys will be stored at "/etc/pki/tls/letsencrypt"
Certificates will be stored at "/etc/pki/tls/letsencrypt"
For compatibility and organisation links will be created at "/etc/ssl/letsencrypt"
