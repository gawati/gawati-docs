template
########

General
*******

The goal of the module is to create a local certificate authority and local
certificates self signed by this CA.

Software Packages
*****************

OS pacakge openssl will be installed

Configuration
*************

Several files will be created

/etc/pki/CA/newcerts/ca.conf
============================

default data for certificate creation

/etc/pki/CA/private/cacert.pem
==============================

CA keypair and certificate for self signing

/etc/pki/CA/certs/ca.crt
========================

public certificate of self signing CA

/etc/pki/CA/<servername>.conf
=============================

certificate data

/etc/pki/CA/private/<servername>.key
====================================

per server private keys

/etc/pki/CA/certs/<servername>.crt
==================================

per server public certificates


ini Variables
*************

certs
=====

comma separated list of servernames for which self signed certificates will be created::

  certs=my.gawati.org


Postinstallers
**************

No postinstaller available.
