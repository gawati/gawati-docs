Server Setup
############

Installation script
*******************

The server setup will install all required components in the OS from `CentOS`_
repositories or respective project sources where no CentOS packages are available.
It will configure the OS and all services as a ready to run sytem.

If you want to try Gawati on a local virtual machine, please follow the related
instructions in :doc:`Gawati on a server / VM <../development/dev-env-vm>` to
install Virtualbox and download / import our preinstalled CentOS 7 image, then
continue here.

The installer is written for `CentOS`_ 7. CentOS "Minimal installation" type
is sufficient.

To download the `installation script`_, switch to user root and execute::

 cd
 curl http://gawati.org/setup -o setup
 chmod 755 setup

Quickstart
==========

The installer must be run as user root. It will switch accounts as needed.

To run the installer simply run::

 ./setup

The installer works in two steps. On first invocation it downloads a Gawati
configuration template with our default settings. On second run the
installation is executed according to the settings in this configuration file.

Running the installer twice will complete our default installation, assuming you
will access your server with the URL *https://my.gawati.local*. You will have to
make sure that *my.gawati.local* resolves to your server IP locally.

.. note::
   The Installer will automatically set the Admin passwords for the 2 eXist instances
   and display it to you. You will need to copy and paste this from the screen or note it down somewhere as it is
   the only point where the password is shown to the user.

If that's all you need, you may finish reading here. Below you find more
information for customising common configuration items and the key information
of what is going to be installed.


Common configurations
*********************

monitoring email address
========================

In the section [fail2ban], change the variables *mailsender* (default:
from@sender.domain) and *mailrecipient* (default: root@localhost).

Gawati server URL
=================

In the ini file that has been downloaded to your home folder after the first run
of the installer, find the section [gawati-portal] and change the
*GAWATI_URL_ROOT* variable (default: my.gawati.org).

SSL server certificate
=======================

Your SSL certificate name must match your Gawati server URL. The installer offers
two mutually exclusive options for creating a certificate.

- signed locally (use for internal server / testing)
- signed by letsencrypt (use for public servers)

Between both options there is a shared set of information identifying the owner
of the generated certificates. Please adapt the respective section in [options]
to your case::

  [options]
  ...
  organisation=ACME Installation Corp Ltd
  country=CH
  state=Zug
  city=Zug

locally signed certificate
--------------------------

Creating such a certificate can be done without any external dependencies. It's
meant for running internal or testing servers.
In section [acme] make sure to configure *type=disabled*. In section [localcerts]
set *type=install* and set variable *certs* identical to your *GAWATI_URL_ROOT*.

`letsencrypt`_ signed certificate
---------------------------------

For this, your Gawati server URL and certificate name must be resolvable via public
DNS and public HTTP requests for it must arrive at your Gawati server on port 80.
If those conditions are met and you intend to make your server publicly available,
this is the preferred option.

In section [localcerts] make sure to configure *type=disabled*. In section [acme]
set *type=install* and set variable *certs* identical to your *GAWATI_URL_ROOT*.

builduser
=========

After installing eXist application servers, the installer will retrieve code
from github, compile and deploy it into these eXist instances. To do this, the
installer creates a user dedicated for compiling Gawati components from source.
This avoids compiling as root and interfering with existing user environments.
The name of this user account is defined by the *builduser* user item in the
[gawati-portal] section.


Installation targets
********************

When you run the installer for the first time, it will download an additional
file "dev.ini" into your home folder. The ini file defines the details of the
installation. We call this an installation target.

With the second execution of the installer, installation commences according to
the configuration in the ini file.

To choose a different profile to install, provide it as a commandline parameter,
for example::

 ./setup prod

At this time, the default target "dev" is the only installation target provided by us.

You can change ours, or create your own ini files if you need to deviate from our defaults.

Components overview
*******************

The Gawati reference server is based on `CentOS`_ 7, Minimal Install.
For hosting the application, we use `eXistdb`_ as XML/document database and
`jetty`_ as Java web application server.

We use two (2) instances of `eXistdb`_

#. Backend - the main data repository / active data
#. Staging - data in transit / for syncronisation

All services except for a (1) frontend Apache instance will be listening on
127.0.0.1 only.

Jetty
=====

`jetty`_ binaries will be installed into /opt for shared use. It will be
configured with configuration files in "start.d" directory.

The Gawati jetty-base environment will be installed into a separate user account.
A JETTY_BASE folder will be created in that users ~/apps/ folder.
A link to its jetty installation in /opt will be created inside JETTY_BASE called
"jettyserver". JETTY_HOME will be configured as JETTY_BASE/jettyserver.

Jetty will be installed as a system service starting with the boot process.

eXistdb
=======

Two (2) instances of `eXistdb`_ will be created. Each instance under a dedicated
user account. eXistdb will be installed in folder ~/apps/existdb with data in
~/apps/existdata. A random generated password will be configured for user "admin"
and is displayed during installation.

The backend instance of eXistdb will be installed as a system service starting
with the boot process.

Downloads
=========

Installation Resources will be downloaded into "/opt/Download"

Uninstalling
============

There is no proper uninstaller yet, but if you installed the system with our
default installation paths and service names, you can use the script at
/opt/Download/installer/uninstall.sh to remove all files related to Gawati.

References
**********

- :doc:`setup-installationsystem`.


.. _CentOS: https://www.centos.org
.. _letsencrypt: https://letsencrypt.org
.. _eXistdb: http://www.exist-db.org
.. _installation script: https://raw.githubusercontent.com/gawati/setup-scripts/master/gawati/gawati_server_setup.sh
.. _jetty: http://www.eclipse.org/jetty/
