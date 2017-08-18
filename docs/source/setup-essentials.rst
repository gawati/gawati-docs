Gawati Essentials Setup
#######################

Installer
*********

The installer is written for Centos 7.


Quickstart
==========

To download the `installation script`_ execute:

 curl "https://raw.githubusercontent.com/gawati/setup-scripts/master/gawati/gawati_server_setup.sh" >gawati_server_setup.sh

The installer must be run from root account. It will switch accounts as needed.

Running the installer twice will complete our default installation.
If that's all you need, you may finish reading here.


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

Jetty will be configured with  configuration files in "start.d" directory.
Each JETTY_BASE folder will contain a link called "jettyserver" pointing to its JETTY_HOME.


eXistdb
=======

A random generated password will be configured for user "admin" which is displayed during installation.


Downloads
=========

Installation Resources will be downloaded into "/opt/Download"


System preparation
******************

The setup will install the following `CentOS`_ packages, if not already installed:

 - java-1.8.0-openjdk
 - httpd
 - wget
 - xmlstarlet
 - git

Install Jetty
*************

`jetty`_ binaries will be installed into /opt for shared use.

The Gawati jetty-base environment will be installed into a separate user account.
A JETTY_BASE folder will be created in that users ~/apps/ folder.
A link to its jetty installation in /opt will be created inside JETTY_BASE called "jettyserver".
JETTY_HOME will be configured as JETTY_BASE/jettyserver.

Jetty will be installed as a system service starting with the boot process.


Install eXistdb
***************

Two (2) instances of `eXistdb`_ will be created. Each instance under a dedicated user account.
eXistdb will be installed in folder ~/apps/existdb with data in ~/apps/existdata.

The backend instance of eXistdb will be installed as a system service starting with the boot process.


Installation definiton
**********************

the default content for "dev.ini"::

  [options]
  installPackages=java-1.8.0-openjdk httpd wget crudini xmlstarlet git
  downloadFolder=/opt/Download
  deploymentFolder=/opt
  debug=0

  [existdb]
  type=resource
  download=eXist-db-setup-3.3.0.jar https://bintray.com/existdb/releases/download_file?file_path=eXist-db-setup-3.3.0.jar

  [jetty]
  type=resource
  download=jetty-distribution-9.4.6.v20170531.tar.gz http://central.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.4.6.v20170531/jetty-distribution-9.4.6.v20170531.tar.gz
  unpackfolder=jetty-distribution-9.4.6.v20170531

  [jetty-dev01]
  type=install
  resources=jetty
  user=dev01
  instanceFolder=~/apps/jetty-apps
  port=9084
  sslport=9444
  modules=server,http,console-capture,deploy,ext,jsp,resources,jstl,websocket,webapp
  options=daemon

  [eXist-be]
  type=install
  resources=existdb
  user=xstbe
  instanceFolder=~/apps/existdb
  dataFolder=~/apps/existdata
  port=10084
  sslport=10444
  options=daemon


  [eXist-st]
  type=install
  resources=existdb
  user=xstst
  instanceFolder=~/apps/existdb
  dataFolder=~/apps/existdata
  port=10083
  sslport=10443
  options=daemon

The format is standard ini format.

options
=======

The "options" section defines parameters that are valid for all installation components.

installPackages
 defines Packages to be installed from the Linux Distributions own repositories

downloadFolder
 common folder for downloads

deploymentFolder
 where applicable, common folder for deploying shared components

debug
 defines debuglevel, higher number means more noise during installation

resources
=========

Resources define files used for installation. They are identified by the line::

  type=resource

The section header defines the name of the resource.
Resource names currently must match the name of the installer function that uses them.

download
 defines, sparated by whitespace

 #. the filename as written to in local filesystem
 #. the URL from which the resource is to be retrieved

unpackfolder (optional)
 for installations deploying shared components into deploymentFolder,
 the name of the shared folder that will be created in deploymentFolder

installs
========

Installs define what to install and parameters used for installation.
They are identified by the line::

  type=install

The section header defines a name for the installation instance.
The name is freely chooseable, but must be unique.
The name will be used as the service / daemon name if installed as such.

resources
 a list of whitespace separated resources required for this installation.

user
 OS user name. installation will commence into this users environment.

instanceFolder
 filesystem folder into which the installation will be deployed

options
 options for the execution of the installation

 daemon
   make the installation a boot time system service / daemon

additional valid items can be defined by the software specific installer function


Implementation considerations
*****************************

Applying eXistdb ports
======================

We deviate with our confguration method from recommendations by eXistdb for the reasons below.

mismatch between online documentation and installation content
--------------------------------------------------------------

Delivered in the package we have...

jetty.xml::

 <Configure id="Server" class="org.eclipse.jetty.server.Server">
    <New id="httpConfig" class="org.eclipse.jetty.server.HttpConfiguration">
      <Set name="securePort">
        <Property name="jetty.httpConfig.securePort" deprecated="jetty.secure.port">
          <Default>
            <SystemProperty name="jetty.secure.port" default="8443"/>

jetty-http.xml::

 <Configure id="Server" class="org.eclipse.jetty.server.Server">
   <Call name="addConnector">
     <Arg>
       <New id="httpConnector" class="org.eclipse.jetty.server.ServerConnector">
         <Set name="port">
           <Property name="jetty.http.port" deprecated="jetty.port">
             <Default>
               <SystemProperty name="jetty.port" default="8080"/>

jetty-ssl.xml::

  <Configure id="Server" class="org.eclipse.jetty.server.Server">
    <Call  name="addConnector">
      <Arg>
        <New id="sslConnector" class="org.eclipse.jetty.server.ServerConnector">
          <Set name="port">
            <Property name="jetty.ssl.port" deprecated="ssl.port">
              <Default>
                <SystemProperty name="jetty.ssl.port" deprecated="ssl.port" default="8443"/>

Compared to documentation at http://exist-db.org/exist/apps/doc/troubleshooting.xml which wants you to...

change this for nonSSL (which doesnt exist)::

 <Set name="port"><SystemProperty name="jetty.port" default="8080"/></Set>

change both of these for SSL (which dont exist)::

 <Set name="confidentialPort">8443</Set>
 <Set name="Port">8443</Set>

Options considered
------------------

changing jetty.xml, but doesnt produce the expected result::

 sed -i "s%^\(.*\)name=\"jetty.port\" default=\"[[:digit:]]*\"/>\(.*\)$%\1name=\"jetty.port\" default=\"${EXIST_PORT}\"/>\2%" "${EXIST_HOME}/tools/jetty/etc/jetty.xml"

changing the default for an undefined property instead of defining the property is not the right thing to do, but does work::

 xmlstarlet ed -u '/Configure[@id="Server"]/New[@id="httpConfig"]/Set[@name="securePort"]/Property[@name="jetty.httpConfig.securePort"]/Default/SystemProperty[@name="jetty.secure.port"]/@default' -v "8444" jetty.xml

Best candidate, defining probed system properties in jetty.xml::

  <Call class="java.lang.System" name="setProperty">
      <Arg>jetty.port</Arg>
      <Arg>10083</Arg>
  </Call>

  <Call class="java.lang.System" name="setProperty">
      <Arg>jetty.ssl.port</Arg>
      <Arg>10443</Arg>
  </Call>


References
**********

- http://exist-db.org/exist/apps/doc/advanced-installation.xml
- http://exist-db.org/exist/apps/doc/production_good_practice.xml
- http://exist-db.org/exist/apps/doc/configuration.xml
- http://exist-db.org/exist/apps/doc/java-admin-client.xml
- http://exist-db.org/exist/apps/doc/troubleshooting.xml


.. _CentOS: https://www.centos.org
.. _eXistdb: http://www.exist-db.org
.. _installation script: https://raw.githubusercontent.com/gawati/setup-scripts/master/gawati/gawati_server_setup.sh
.. _jetty: http://www.eclipse.org/jetty/
