Gawati Installation System
##########################

If you are looking for how to run a standard Gawati installation, please see
:doc:`../../setup/setup-essentials`.

Installation definiton file
***************************

The format is standard ini format. Section names and item names within a section
must be unique.

options
=======

The "options" section defines parameters that are valid for all installation components.

example content of [options] in "dev.ini"::

  [options]
  installPackages=java-1.8.0-openjdk
  downloadFolder=/opt/Download
  deploymentFolder=/opt
  debug=0


installPackages
 defines Packages to be installed from the Linux Distributions own repositories

downloadFolder
 common folder for downloads

deploymentFolder
 where applicable, common folder for deploying shared components

debug
 defines debuglevel, higher number means more noise during installation (0-3)

Installer
=========

Installers define what to install and parameters to be used for installation.
The content of demonstration installer instance in "dev.ini"::

  [demo]
  type=install
  installer=template
  version=1.0
  user=root
  instanceFolder=~
  options=demooption
  postinstall=postdemo

This installer is made for demonstration purposes. It does not execute any
installation task. The instance of this installation is called *demo*. If You
run multiple installations with the same installer, each one must have a
different, unique instance name as its header. This name will also be used as
the service / daemon name if installed as such.


type
 Installer sections are marked by the line::

  type=install

installer
 Installer sections reference an installer script. The name of the installer
 is denoted in the installer parameter::

  installer=template

This name references the folder name inside the *installers* folder containing
all components of this installer including the script itself

version
 Different versions may be available for installation. The chosen version must
 be denoted for the installation::

  version=1.0

The *installers* folder is to be found on the git repositry in the folder
*setup-scripts/gawati* and will be copied to the users *downloadFolder* as
defined in the [options] ini files, which is in */opt/Download* by default.

In total these together::

  [demo]
  type=install
  installer=template
  version=1.0

make the installer run the script *installer/${installer}/${version}*.
With *downloadFolder* set as */opt/Download*, in this example expand to::

  /opt/Download/installer/template/1.0

which is a link to the actual script inside the folder
*/opt/Download/installer/template/scripts*::

    /opt/Download
         └── installer
             └── template
                 ├── 1.0 -> scripts/template.sh
                 └── scripts
                     ├── postdemo.sh
                     └── template.sh

This allows to make a single script serve multiple versions of a product if
such is feasible for the given product.

user
 OS user name. The installation will typically execute as this user and/or run
 its service as this user::

   user=root

instanceFolder
 filesystem folder into which the installation will be deployed::

  instanceFolder=~

options
 options for the execution of the installation::

  options=daemon

 daemon
   make the installation a boot time system service / daemon

 which options are supported is defined by the installer in the given section


postinstall
 additional, optional installation steps where available with the given
 installer can be activated by listing in postinstall

There can be any number of addtional items added and defined by the installer
script called in the section.


resources
=========

A dedicated resources section is used in special cases only. Typically installers
define their requirements themselves.

Resources define additional files used for installation. They are identified by
the line::

  type=resource

The section header defines the name of the resource.
Resource names currently must match the name of the installer function that uses them.

download
 defines, separated by whitespace

 #. the filename as written to in local filesystem
 #. the URL from which the resource is to be retrieved

unpackfolder (optional)
 for installations deploying shared components into deploymentFolder,
 the name of the shared folder that will be created in deploymentFolder


Implementation considerations
*****************************

Applying eXistdb ports
======================

We deviate with our confguration method from recommendations by eXistdb for the
reasons below.

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

Compared to documentation at http://exist-db.org/exist/apps/doc/troubleshooting.xml
which wants you to...

change this for nonSSL (which doesnt exist)::

 <Set name="port"><SystemProperty name="jetty.port" default="8080"/></Set>

change both of these for SSL (which dont exist)::

 <Set name="confidentialPort">8443</Set>
 <Set name="Port">8443</Set>

Options considered
------------------

changing jetty.xml, but doesnt produce the expected result::

 sed -i "s%^\(.*\)name=\"jetty.port\" default=\"[[:digit:]]*\"/>\(.*\)$%\1name=\"jetty.port\" default=\"${EXIST_PORT}\"/>\2%" "${EXIST_HOME}/tools/jetty/etc/jetty.xml"

changing the default for an undefined property instead of defining the property
is not the right thing to do, but does work::

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
