gawatiportal
############

General
*******

The goal of the module is to apply Gawati specific configurations in the
operating system and webserver.

Software Packages
*****************

"unzip" will be installed.

Configuration
*************

A Gawati website configuration "10-gawati.conf" file will be added to
/etc/httpd/conf.d


ini Variables
*************

The DNS name for the public gawati portal root URL needs to be specified in
"GAWATI_URL_ROOT". The internal URL for the backend eXist DB must be specified
as "EXIST_ST_URL". The eXist DB installer instance names for Gawati backend and
frontend must be provided as "exstbe" and "existst" respectively.

Postinstallers
**************

None

Details
*******

In local apache instance, a virtual host directory will be created in
/var/www/html and a matching log firectory in /var/log/httpd. These locations
will be references in /etc/httpd/conf.d/10-gawati.conf.
SSL key and certificate matching the Gawati URL with compliant naming must be
present in /etc/pki/tls/private and /etc/pki/tls/certs respectively. By default
this is prepared by the <localcert> or <letsencrypt> module.
The Gawati default website template / theme will be deployed into its web root
folder below /var/www/html
