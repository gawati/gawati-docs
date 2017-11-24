Gawati as a local installation
##############################

The below instructions are not specific to an operating system, the components will run on different operating systems.

.. note::
  Installing the JDK 8 is a pre-requisite. On Linux operating systems you can install `OpenJDK8 <http://openjdk.java.net/install/>`_; For `Windows <https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html#CHDEBCCJ>`_ and for `Mac OSX <https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#CHDBADCG>`_ and `Using OS X Homebrew <https://stackoverflow.com/questions/24342886/how-to-install-java-8-on-mac/28635465#28635465>`_

The installation items are listed below:

  1. eXist-db: Download and install eXist-db, see `eXist-db <https://bintray.com/existdb/releases/exist/3.4.1/view>`_ Remember to note down the admin password of the eXist-db installation, you will need that later.
  2. Ant: Download and `install Ant <http://ant.apache.org/manual/install.html#installing>`_
  3. Apache: Install Apache, on Cent OS, Ubuntu and OS X this will likely be installed by default, on Windows you will have to download and install, see `Apache for Windows <https://www.apachehaus.com/cgi-bin/download.plx>`_; enable `mod_alias`, `mod_rewrite`, `mod_proxy`, `mod_proxy_http`
  4. Visual Studio Code: This is if you want play around with gawati code. Download, and setup Visual Studio Code (there are versions for Windows, OS X and Linux) for development, see :doc:`VS Code Setup <./using-vscode>`

You can either build the source from github for each component, or you can install a released version of a component. For getting familiar with the system we recommend starting by installing a released version.

*********************
Installing components
*********************
.. note::
  .. include:: version-info.rst
  .. include:: download-links.rst

Place #1,#2 & #3 files in the `autodeploy` folder within the eXist installation, and restart the eXist database server. They will be automatically installed.

Now extract #4 which are the front-end templates into a folder where Apache can serve them.


****************
Load Sample Data
****************

.. note::
  The sample data is currently at version 1.2

To understand better how gawati works, we provide you with sample data, which can be loaded into the system and tested. Sample data is provided in two specific parts:

 * Xml Documents - which get loaded into the XML database
 * PDF and other binary Documents - which are refered to by the XML documents, but served from the *file system*

We do this to ensure optimal performance.

Download the data sets
======================

Download the `XML Data set`_ and the corresponding `PDF Data set`_

Setup the PDF data set
======================

To setup the PDF data-set, you just need to extract the files into a folder, e.g if you extract the PDF files into `/home/data/akn_pdf`, and add a Apache configuration to serve the folder contents (See line 7 below `Add the Apache configuration`_)

Setup the XML data set
======================

To setup the XML data-set, extract the archive into a separate folder. On Linux and MacOS you can run the following command to get the data input password:

.. code-block:: bash
  :linenos:

  <path_to_exist>/bin/client.sh -ouri=xmldb:exist://localhost:8080/exist/xmlrpc -u admin -P <exist_admin_password> -x "data(doc('/db/apps/gw-data/_auth/_pw.xml')/users/user[@name = 'gwdata']/@pw)"

Where `<path_to_exist>` is the path to the eXist-db installation, and `<exist_admin_password>` is the eXist-db admin password. If you installed eXist on a different port change that in the `-ouri` setting.

On Windows do the following; Start the eXist-db Client(`<path_to_exist>/bin/client.bat`). In the command window of the eXist-db client run the following commands:

.. code-block:: none
  :linenos:

  find data(doc('/db/apps/gw-data/_auth/_pw.xml')/users/user[@name = 'gwdata']/@pw)
  show 1

Copy the output password hash as shown below.

.. figure:: ./_images/client-get-data-password.png
  :alt: Get data entry password
  :align: center
  :figclass: align-center

Now upload the data using the following command run from the eXist-db folder:

.. code-block:: bash
  :linenos:

  ./bin/client.sh -u gwdata -P <copied_password_hash> -d -m /db/apps/gw-data/akn -p /home/data/akn_xml/akn

On Windows you will run it as :samp:`.\\bin\\client.bat` instead:

.. code-block:: bash
  :linenos:

  .\bin\client.bat -u gwdata -P <copied_password_hash> -d -m /db/apps/gw-data/akn -p d:\data\akn_xml\akn


****************************
Add the Apache configuration
****************************

The Apache configuration will allow accessing gawati over a web-browser using the URL:

.. code-block:: none

  http://localhost/gwportal/

To do this, open the `httpd.conf` (or equivalent) file of your apache installation and add the following:

.. code-block:: apacheconf
  :linenos:

    Alias /gwtemplates "/home/apps/path/to/gawati-templates"
    <Directory "/home/apps/path/to/gawati-templates">
      Require all granted
      AllowOverride All
      Order allow,deny
      Allow from all
    </Directory>

    Alias /akn "/home/data/akn_pdf"
    <Directory "/home/data/akn_pdf">
      Require all granted
      Options Includes FollowSymLinks
      AllowOverride All
      Order allow,deny
      Allow from all
    </Directory>

    <Location "/gwportal/">
      AddType text/cache-manifest .appcache
      DirectoryIndex "index.html"
      ProxyPass  "http://localhost:8080/exist/apps/gawati-portal/"
      ProxyPassReverse "http://localhost:8080/exist/apps/gawati-portal/"
      SetEnv force-proxy-request-1.0 1
      ProxyPassReverseCookiePath /exist /
      SetEnv proxy-nokeepalive 1
    </Location>

The above assumes:
  * eXist-db is running on port 8080 (if that is not the case in your installation change it appropriately in line 16 and 17)
  * Change the path in line 1 and line 2 to the folder into which you extracted `Gawati Templates`
  * Change the path in line 7 and 8 to the folder into which you extracted the Gawati Sample data.

.. note::
  On Windows the Apache Alias directory path need to use the back slash instead of the standard windows forward slash. For e.g. if the templates are in: `d:\\code\\gawati-templates` then the path in the Apache configuration should be: `d:/code/gawati-templates`


****************
Portal Version 2
****************

The Portal Version 2 is a new front-end to the Gawati Data Server and will replace the current Portal frontend, what we call *gawati-portal*.

This can be found at the following URL: `Portal v2 <https://github.com/gawati/gawati-portal-v2>`_. It has been written on the `node js <https://nodejs.org/en/>`_ platform.

Pre-requisities for Installing Version 2:

  * NVM - Node Version Manager
  * Node JS

First install NVM:

.. code-block:: bash
  :linenos:

  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash

Then install node using NVM:

.. code-block:: bash
  :linenos:

  nvm install node --lts

Then build the Portal V2 app:

.. code-block:: bash
  :linenos:

  npm run build

The `build` folder has the full app compiled as static html and JS. Put this within Apache HTTP or deploy using ExpressJS server.


.. _XML Data set: https://github.com/gawati/gawati-data-xml/releases/download/1.2/akn_xml_sample-1.2.zip
.. _PDF Data set: https://github.com/gawati/gawati-data-xml/releases/download/1.2/akn_pdf_sample-1.2.zip
