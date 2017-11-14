gawatidemodata
##############

General
*******

This module feeds demodata into a Gawati instance. We provide a set of 200 documents
of the ALL document dataset and matching XML data for trying and developing the
Gawati system.

Software Packages
*****************

unzip will be installed.

Configuration
*************

No OS configurations applied

ini Variables
*************

existst
=======

Which exist instance will be used for uploading the data. The configuration of
this instance must be available uin the same installer ini::

  existst=eXist-st

importFolder
============

Defines the folder into which the data for uploading will be unpacked::

  importFolder=/tmp/import

Postinstallers
**************

No postinstaller available.
