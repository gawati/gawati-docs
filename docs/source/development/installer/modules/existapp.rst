existapp
########

General
*******

The goal of the module is to install or update exist applications from internet
sources.

Software Packages
*****************

No OS packages to install

Configuration
*************

No OS configurations applied

ini Variables
*************

appname
=======

The name of the app as it appears inside existdb namespace. Used to identify if
an instance of the application is already present::

  appname=http://gawati.org/portal

source_url
==========

The source off which the app is to be deployed as a URL::

  source_url=https://github.com/gawati/gawati-portal/releases/download/1.4/gawati-portal-1.4-all.xar

exist_instance
==============

The exist installer instance name in the same installer ini file to which the app
is to be deployed. This retrieves connection and if available password information
to connect to exist::

  exist_instance=eXist-st

Postinstallers
**************

No postinstaller available.
