#######################
Development Environment
#######################

************
Introduction
************

Gawati is composed of different components, you don't need to install the full set to begin developing on the Gawati platform.  This section provides an outline of how you can use and individual components. 


.. note::
  If you only wish to install and test the system, See :doc:`Setup <../setup/index>`.

****************
Getting Started
****************

You will need to setup the basics first, the below instructions are not specific to an operating system, the components will run on different operating systems.

.. note::
  Installing the JDK 8 is a pre-requisite. On Linux operating systems you can install  `OpenJDK8 <http://openjdk.java.net/install/>`_; For `Windows <https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html#CHDEBCCJ>`_ and for `Mac OSX <https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#CHDBADCG>`_ and `Using OS X Homebrew <https://stackoverflow.com/questions/24342886/how-to-install-java-8-on-mac/28635465#28635465>`_

The installation items are listed below:

  1. eXist-db: Download and install eXist-db, see `eXist-db <https://bintray.com/existdb/releases/exist/3.4.1/view>`_ 
  2. Ant: Download and `install Ant <http://ant.apache.org/manual/install.html#installing>`_
  3. Apache: Install Apache, on Cent OS, Ubuntu and OS X this will likely be installed by default, on Windows you will have to download and install, see `Apache for Windows <https://www.apachehaus.com/cgi-bin/download.plx>`_; enable `mod_alias`, `mod_rewrite`, `mod_proxy`
  4. Visual Studio Code: This is if you want play around with gawati code. Download, and setup Visual Studio Code (there are versions for Windows, OS X and Linux) for development, see :doc:`VS Code Setup <./using-vscode>`

You can either build the source from github for each component, or you can install a released version of a component. 

For getting familiar with the system we recommend starting by installing a released version. 

*********************
Installing components
*********************

First download the individual components into a folder:

 1. `Gawati Portal <https://github.com/gawati/gawati-portal/releases/download/1.3/gawati-portal-1.3-dev.xar>_`
 2. `Gawati Data <https://github.com/gawati/gawati-data/releases/download/1.2/gawati-data-1.2.xar>_`
 3. `Gawati XML Data <https://github.com/gawati/gawati-data-xml/releases/download/1.2/gw-data-1.2.xar>_`

Place these files in the `autodeploy` folder within the eXist installation, and restart the eXist database server. They will be automatically installed. 

Now download the front-end templates, and place them in a folder where Apache can server them: 

  * `Gawati Templates <https://github.com/gawati/gawati-templates/releases/download/1.3/gawati-templates-1.3.zip>_`

Add the Apache configuration:

.. code-block:: apacheconf
   :linenos:

    Alias /gwtemplates "/home/apps/path/to/gawati-templates"
    <Directory "/home/apps/path/to/gawati-templates">
    Require all granted
      Options Indexes
      AllowOverride All
      Order allow,deny
      Allow from all
    </Directory>

    Alias /akn "/home/data/akn"
    <Directory "/home/data/akn">
        Require all granted
        Options Indexes Includes FollowSymLinks
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>

    <Location "/gwportal/">
      AddType text/cache-manifest .appcache
      DirectoryIndex "index.html"
      ProxyPass  "http://localhost:8080/exist/apps/gawati-portal/"
      ProxyPassReverse "http://localhost:8080/exist/apps/gawati-portal/"
      ProxyPassReverseCookiePath /exist /
      SetEnv force-proxy-request-1.0 1
      SetEnv proxy-nokeepalive 1
    </Location>


The above assumes:
  * eXist-db is running on port 8080 (if that is not the case in your installation change it appropriately)


*******************
Loading Sample Data
*******************







 


*************************
Building code from Github
*************************

We are going to look at 2 components of Gawati:
 - the Gawati-Portal component: Provides a web portal interface to Legal data on Gawati
 - the Gawati-Data component: Provides a REST API to access Gawati documents from the XML database.
 
The Portal accesses data and documents from the XML database via the Gawati-Data server's REST APIs.

The build process for these components is a trivial one. It merely packages the files into a format expected by eXist-db, and then the packages are deployed on eXist-db.

For example, to deploy Gawati-Data on the eXist-db server, do the following::

  https://github.com/gawati/gawati-data.git
  cd gawati-data

The source code for the Gawati-Data server is in the `gawati-data` folder, you can make code changes there.
Finally package your code::

  ant xar

This will generate a file `gawati-data-X.X.X.xar` in the `./build` folder, which you will install into eXist-db via the Dashboard.

If you have a stock installation of eXist-db, it will be running on port 8080. Access eXist-db on that port via the web-browser. Login as admin, and that should bring you to the page `http://localhost:8080/exist/apps/dashboard/index.html`. In the dashboard click on *Package Manager*:

.. figure:: ./_images/dashboard.jpg
   :alt: eXist-db dashboard
   :align: center
   :figclass: align-center

Click the *+* icon, and select the package you just built in the `build` folder and install it into eXist-db. You will find the package accessible via the URL: `eXist gawati data <http://localhost:8080/exist/apps/gawati-data>`_
