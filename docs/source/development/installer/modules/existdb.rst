httpd
######

General
*******

The goal of the module is to install existdb and apply essential configuration
for production use.

Software Packages
*****************

The module installs "xmlstarlet" to edit XML based configuration files of existdb.

Configuration
*************

No configurations applied

ini Variables
*************

instanceFolder
==============

The filesystem folder where existdb binaries will be installed.

dataFolder
=========

The filesystem folder where existdb data files will be stored.

port
====

The non SSL TCP port where existdb will answer http requests.

sslport
=======

The SSL TCP port where existdb will answer https requests.

options
=======

If "options" contains "daemon" existdb will be configured and enabled for start
during boot.

Postinstallers
**************

1 postinstaller available called "selinux" to apply selinux context configuration
for existdb.
