Gawati Essentials Setup
#######################

Installer
*********

The installer is written for `CentOS`_ 7.


Quickstart
==========

To download the `installation script`_ execute:

 curl https://raw.githubusercontent.com/gawati/setup-scripts/master/gawati/gawati_server_setup.sh -o gawati_server_setup.sh

The installer must be run from root account. It will switch accounts as needed.

Running the installer twice will complete our default installation, assuming you
will access your server with the URL *https://my.gawati.org*. You will have to
make sure that *my.gawati.org* resolves to your server IP locally.

If that's all you need, you may finish reading here.

Common customisations
=====================

montoring email address
-----------------------

In the section [fail2ban], change the variables *mailsender* (default:
from@sender.domain) and *mailrecipient* (default: root@localhost).

Gawati server URL
-----------------

In the ini file that has been downloaded to your home folder after the first run
of the installer, find the section [gawati-portal] and change the
*GAWATI_URL_ROOT* variable (default: my.gawati.org).

SSL server certificate
----------------------

Your SSL certificate name must match your Gawati server URL. You have two
mutually exclusive options for creating a certificate.

 - signed locally (use for internal server / testing)
 - signed by letsencrypt (use for public servers)

locally signed certificate
''''''''''''''''''''''''''

Creating such a certificate can be done without any external dependiencies. It's
meant  for running internal or testing servers.
In section [acme] make sure to configure *type=disabled*. In section [localcerts]
set *type=install* and set variaböe *certs* identical to your *GAWATI_URL_ROOT*.

`letsencrypt`_ signed certificate
'''''''''''''''''''''''''''''''''

For this, your Gawati server URL and certificate name must be resolvable via public
DNS and public HTTP requests must arrive at your Gawati server on port 80.
If those conditions are met, and you intend to make your server publicly available,
this is the preferred option.

In section [localcerts] make sure to configure *type=disabled*. In section [acme]
set *type=install* and set variaböe *certs* identical to your *GAWATI_URL_ROOT*.


Installation targets
====================

When you run the installer for the first time, it will download an additional file "dev.ini" into your home folder.
The ini file defines the details of the installation. We call this an installation target.

With the second execution of the installer, installation commences according to the configuration in the ini file.

To choose a different profile to install, provide it as a commandline parameter, for example::

 ./gawati_server_setup.sh prod

At this time, the default target "dev" is the only installation target provided by us.

You can change ours, or create your own ini files if you need to deviate from our defaults.


Components overview
*******************

The Gawati reference server is based on `CentOS`_ 7, Minimal Install.
For hosting the application, we use `eXistdb`_ as XML/document database and `jetty`_ as Java web application server.

We use two (2) instances of `eXistdb`_

 #. Backend - the main data repository / active data
 #. Staging - data in transit / for syncronisation

All services except for a (1) frontend Apache instance will be listening on 127.0.0.1 only.
These installation instructions will be converged into an installer tool at a later stage.


Jetty
=====

`jetty`_ binaries will be installed into /opt for shared use. Iz will be
configured with configuration files in "start.d" directory.

The Gawati jetty-base environment will be installed into a separate user account.
A JETTY_BASE folder will be created in that users ~/apps/ folder.
A link to its jetty installation in /opt will be created inside JETTY_BASE called "jettyserver".
JETTY_HOME will be configured as JETTY_BASE/jettyserver.

Jetty will be installed as a system service starting with the boot process.


eXistdb
=======

Two (2) instances of `eXistdb`_ will be created. Each instance under a dedicated user account.
eXistdb will be installed in folder ~/apps/existdb with data in ~/apps/existdata.
A random generated password will be configured for user "admin" which is displayed during installation.

The backend instance of eXistdb will be installed as a system service starting with the boot process.


Downloads
=========

Installation Resources will be downloaded into "/opt/Download"

References
**********

 - :doc:`setup-installationsystem`.


.. _CentOS: https://www.centos.org
.. _letsencrypt: https://letsencrypt.org
.. _eXistdb: http://www.exist-db.org
.. _installation script: https://raw.githubusercontent.com/gawati/setup-scripts/master/gawati/gawati_server_setup.sh
.. _jetty: http://www.eclipse.org/jetty/
