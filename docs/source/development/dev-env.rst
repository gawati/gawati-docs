#######################
Development Environment
#######################

************
Introduction
************

Gawati is composed of different components, you don't need to install the full set to begin developing on the Gawati platform.
This section provides an outline of how you can contribute and use individual components. If you wish to install the complete stack, you can still do that. See :doc:`Setup <../setup/index>`.

****************
Getting Started
****************

You will need to setup the basics first, the below instructions are specific to Windows 10 and Cent OS.

 - Download and install eXist-db, see `eXist-db <https://bintray.com/existdb/releases/exist/3.4.1/view>`_
 - Download, and setup Visual Studio Code for development, see :doc:`VS Code Setup <./using-vscode>`
 - Download and `install Ant <http://ant.apache.org/manual/install.html#installing>`_
 - Install Apache, on Cent OS this will likely be installed by default, on Windows you will have to download and install, see `Apache for Windows <https://www.apachehaus.com/cgi-bin/download.plx>`_; enable `mod_alias`, `mod_rewrite`, `mod_proxy`

*************************
Building code from Github
*************************

We are going to look at 2 components of Gawati:
 - the Gawati-Portal component: Provides a web portal interface to Legal data on Gawati
 - the Gawati-Data component: Provides a REST API to access Gawati documents from the XML database
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
