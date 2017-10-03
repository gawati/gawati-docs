centos
######

General
*******

The goal of the module is to configure a newly minimal CentOS installation for
production use as an internet connected server platform.

Software Packages
*****************

The module will perform a system update if it has never been performed or last
update is older than 1 week. It will add essential administration tools.

Configuration
*************

The system wil be configured for minimum swapping and disable IPv6.
Correct hostname and DNS domain name configuration will be ensured.

ini Variables
*************

hostname
========

The "physical" (interface and IP independent) single hostname of the machine
without domain name (no dot ".").

DNSdomain
=========

The DNS domain name to be used with the hostname variable.

Postinstallers
**************

None
