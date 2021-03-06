#################
Developing Gawati
#################

.. contents:: Table of Contents 
  :local:

************
Introduction
************

Gawati is made of different components, you don't need to install the full set to begin developing on the Gawati platform.  This section provides an outline of how you can use and individual components.


.. note::
  If you only wish to install and test the system, See :doc:`Setup <../setup/index>`.

****************
Getting Started
****************

You will need to setup the basics first.

Prepare your development environment installing and running Gawati as local services on your development system (this document provides you with additional insight).
Alternatively, use a prebuilt VM and Gawati installer to run Gawati as a virtual machine (this document gets you a standardised environment fast).

.. toctree::
   :maxdepth: 1

   dev-env-local
   dev-env-vm


********************
Development Workflow
********************

The standard development cycle is as follows:
  1. clone the projects from github
  2. build the projects where necessary (`gawati portal`_, `gawati data`_, `gawati data xml`_)
  3. deploy onto eXist-db (`gawati portal`_, `gawati data`_, `gawati data xml`_)


Code for eXist-db packages requires an additional step. You will need to export the database onto the file-system and then merge it into your github clone folder:

  .. figure:: ./_images/exist-backup.png
   :alt: eXist backup
   :align: center
   :figclass: align-center

The database contents get exported to the file system:

  .. figure:: ./_images/exist-backup-export.png
   :alt: eXist backup exported to file system
   :align: center
   :figclass: align-center

In the image the exported `gawati portal`_ folder is selected. You will need to compare this folder with the git cloned folder on your file system using a tool like `WinMerge`_(on Windows) or `Meld`_(on Linux) or `Meld OS X`_, and merge the changed files. After which you can commit your changes.

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

Click the *+* icon, and select the package you just built in the `build` folder and install it into eXist-db. You will find the package accessible via the URL: `eXist gawati data <http://localhost:8080/exist/apps/gawati-data>`


********************
Customizing Gawati
********************

See :doc:`Theming Guide <./theming-guide>` . 