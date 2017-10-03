gawatiportal
############

General
*******

The goal of the module is to apply Gawati specific configurations in the
operating system and webserver, set up a user and start compilation and
deployment of Gawati by the 2nd stage installer.

Software Packages
*****************

"fabric" will be installed.

Configuration
*************

A Gawati website configuration "10-gawati.conf" file will be added to
/etc/httpd/conf.d


ini Variables
*************

The DNS name for the public gawati portal root URL needs to be specified in
"GAWATI_URL_ROOT". The internal URL for the backend eXist DB must be specified
as "EXIST_BE_URL". The eXist DB installer instances for Gawati backend and
frontend must be provided as "exstbe" and "existst" respectively. A name for an
OS user account must be specified as "builduser". This account will be created
if not existing and used by the 2nd stage installer to build and deploy Gawati
from source.

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
