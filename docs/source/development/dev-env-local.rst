Gawati as a local installation
##############################

If you haven't yet, please review the Gawati Architecture page: :doc:`../system/gawati-architecture`.

The below instructions are not specific to an operating system, the components will run on different operating systems.

.. _inst-prerequisites:

.. note::
  .. include:: prerequisites.rst

.. contents:: Table of Contents 
  :local:

You can either build the source from github for each component, or you can install a released version of a component. For getting familiar with the system we recommend starting by installing a released version.

.. note::
  .. include:: version-info.rst

.. _inst-gawati-portal:

************************
Installing Gawati Portal
************************

Gawati Portal provides access to all legal and legislative data in the system.
See :ref:`gawati-portal` for an architecture overview. 

**IMPORTANT**: In Gawati Component interaction is purely via REST services, we use Apache HTTP as a reverse proxy to bind all the services together under one domain and user interface for the user. You may want to read up and get an idea of the Apache configuration before starting the installation. :doc:`./dev-and-prod-testing`

Installing Gawati Data
======================

.. note::
  .. include:: note-gawati-data.rst

For development environments, you should clone the project from Git, and build the package:

.. code-block:: bash
  :linenos:

  git clone https://github.com/gawati/gawati-data.git
  ant xar

.. raw:: html

  <iframe width="560" height="315" src="http://www.youtube.com/embed/35ua4-Ctov8?rel=0" frameborder="0" allowfullscreen></iframe>
  

Install the ``gawati-data``  XAR file into eXist using the eXist package manager in the eXist-db admin dashboard to manually select and install the package (see `Installing Packages in eXist-db`_).

.. raw:: html

  <iframe width="560" height="315" src="http://www.youtube.com/embed/9AYSWREk24I?rel=0" frameborder="0" allowfullscreen></iframe>


Load Sample Data
----------------
.. note::
  The sample data is currently at version 1.14

To understand better how gawati works, we provide you with sample data, which can be loaded into the system and tested. Sample data is provided in two specific parts:

 * Xml Documents - which get loaded into the XML database 
 * PDF and other binary Documents - which are refered to by the XML documents, but served from the *file system*

We serve PDF and other binary documents from the filesystem to ensure optimal performance.

Download the data sets
----------------------

Download the XML data set, which is in 2 parts: `XML Data set`_  +  `Full Text Data set`_ (the full text data set is the full text extraction of the PDFs) and the corresponding `PDF Data set`_

Setup the PDF data set
----------------------

To setup the PDF data-set, you just need to extract the files into a folder, e.g if you extract the PDF files into ``/home/data/akn_pdf``, and add a Apache configuration to serve the folder contents (See :ref:`conf-binary`)

Setup the XML data set
----------------------

To setup the XML data-set, extract the archives into separate folders (e.g. ``/home/data/akn_xml/akn`` and ``/home/data/akn_xml/akn_ft``). On Linux and MacOS you can run the following command to get the data input password:

.. code-block:: bash
  :linenos:

  <path_to_exist>/bin/client.sh -ouri=xmldb:exist://localhost:8080/exist/xmlrpc -u admin -P <exist_admin_password> -x "data(doc('/db/apps/gawati-data/_auth/_pw.xml')/users/user[@name = 'gawatidata']/@pw)"

Where ``<path_to_exist>`` is the path to the eXist-db installation, and ``<exist_admin_password>`` is the eXist-db admin password. If you installed eXist on a different port change that in the ``-ouri`` setting.

On Windows do the following; Start the eXist-db Client(``<path_to_exist>/bin/client.bat``). In the command window of the eXist-db client run the following commands:

.. code-block:: none
  :linenos:

  find data(doc('/db/apps/gawati-data/_auth/_pw.xml')/users/user[@name = 'gawatidata']/@pw)
  show 1

Copy the output password hash as shown below.

.. figure:: ./_images/client-get-data-password.png
  :alt: Get data entry password
  :align: center
  :figclass: align-center

Now upload the data using the following command run from the eXist-db folder:

.. code-block:: bash
  :linenos:

  ./bin/client.sh -u gawatidata -P <copied_password_hash> -d -m /db/docs/gawati-data -p /home/data/akn_xml_docs_sample
  ./bin/client.sh -u gawatidata -P <copied_password_hash> -d -m /db/docs/gawati-data -p /home/data/akn_xml_ft_sample
  

On Windows you will run it as :samp:``.\\bin\\client.bat`` instead:

.. code-block:: bash
  :linenos:

  .\bin\client.bat -u gawatidata -P <copied_password_hash> -d -m /db/docs/gawati-data -p d:\data\akn_xml_docs_sample
  .\bin\client.bat -u gawatidata -P <copied_password_hash> -d -m /db/docs/gawati-data -p d:\data\akn_xml_ft_sample

.. note::
  
  1. the current folder structure expected within ``/db/docs/gawati-data`` is ``/db/docs/gawati-data/akn`` for the XML files and ``/db/docs/gawati-data/akn_ft`` for the full text files.
  2. if you get a password failure, log in to eXist-db as admin, and reset the password for gwdata user manually, and then use that password.


Finally Rebuild the Database Index
-----------------------------------

.. code-block:: bash
  :linenos:

  $curl http://localhost:8080/exist/apps/gawati-data/post-data-load.xql
  <success>Build Sort index</success>


Apache Config
-------------

There are Apache HTTP configs required for both serving XML and PDF documents. See :ref:`conf-gawati-data` and :ref:`conf-binary`

Development
-----------

We recommend using :doc:`Oxygen XML for developing on eXist-db <./using-oxygen>`. VSCode can also be used (see :ref:`using-vscode-existdb`).


Accessing eXist-db via SSH tunnelling
-------------------------------------

If eXist-db is installed in a remote server, by default the server starts on port 8080 and listens only to localhost.
To access the web-based dashboard from a remote computer, you need to use ssh tunneling. For example, if your remote server  is on the I.P. Address `101.102.103.104`, and eXist-db is on port `8080`, running the following command, will give you access to the eXist-db dashboard on `http://localhost:9999` :

.. code-block:: bash
  :linenos:

   ssh -vv -i <path to private key> -p 22 -L 9999:127.0.0.1:8080 server_user@101.102.103.104


.. _inst-gawati-portal-ui:

Installing Gawati Portal UI
===========================

Extract the contents of the zip file onto a directory served by Apache. 

But, if installing for development, clone from git and build:

.. code-block:: bash
  :linenos:

  git clone https://github.com/gawati/gawati-portal-ui.git
  npm install 

And add the corresponding Apache Server configuration entry (See :ref:`conf-portal-ui`). 

The React app in the https://gitlab.com/bungenicom/gawati/gawati-portal-ui repository can be used to run different clients.  The configuration and set up scripts for the presently available clients: `ecowas` and `gawati` are available in separate git repositories.

.. code-block:: bash
  :linenos:

    git clone https://gitlab.com/bungenicom/gawati/portal-ui-config
    git clone https://gitlab.com/bungenicom/ecowas/portal-ui-config
    

**Switching clients**

In order to switch clients during development:

* Checkout to the `<client>/portal-ui-config` repository and pull latest.
* Check proxy setting in 
   - `package.json`
   - `portal-ui-config/proxy.js`.

* Change following values in `.env` and `.env.development` files accordingly:

  .. code-block:: bash

    REACT_APP_THEME=default
    REACT_APP_CLIENT=gawati
    REACT_APP_CONFIG_PATH=../../gawati-portal-config

* Restart portal

.. note::
  `REACT_APP_CONFIG_PATH` points to the client config repository.  The path is relative to the `scripts` folder in gawati-portal-ui repository.

.. note::
  A theme can only be associated with a specific client. A client may have a choice of multiple themes.


Development and Production mode
-------------------------------

See our detailed guide on setting up your environment for production and development mode testing :doc:`./dev-and-prod-testing`.

For setting up Authentication, click here:  :doc:`Authentication <./authentication>`

.. _inst-gawati-portal-fe:

Installing Gawati Portal FE
===========================

Extract the contents of the zip file into any directory. 

For development environments, clone from git and install it:

.. code-block:: bash
  :linenos:

  git clone https://github.com/gawati/gawati-portal-fe.git
  npm install 


The Gawati Portal has two runnable components, the portal http server which provides access to REST services, and a cron component that runs scheduled tasks periodically. 


Apache Config
-------------

See :ref:`conf-portal-server`.

Environment Variables
---------------------

The server can be customized with various envirobment variables which can be specified as prefixes to the service startup. 

  * WITH_CRON - setting `WITH_CRON=1` starts the server with the cron, so there is no separate process for the cron. *This is not recommended for production use*.
  * WITH_CLIENT - setting `WITH_CLIENT=1`, the server provides the portal-ui client on the `/v2` virual directory (instead of Apache doing it). The client is expected to be in the `client/build` sub-directory.
  * HOST - allows setting the host name or address which the server binds to, default is `127.0.0.1`. 
  * PORT - allows setting the port on which the server listens to, default is `9001`.
  * API_HOST - allows setting the host address to the `gawati-data` server, default is `localhost`
  * API_PORT - allows setting the port number to the `gawati-data` server, default is `8080`

Configuring KeyCloak Auth
=========================
#. Follow the installation steps from `Installing Keycloak`_.

#. Within the ``auth.gawati.local`` realm, navigate to the ``Clients`` tab. Click on ``gawati-portal-ui``. Set the other parameters as shown below. In this case we have set the root url, valid url etc to http://localhost:3000 which is the dev mode host and port for Gawati Editor UI. If you are deploying on a domain e.g. http://www.domain.org you can set it to that domain. Note the ``Redirect URLs`` can be set to multiple urls, this is because, the profiles services typically runs on a different url base .e.g. a different domain than the portal, so the authentication headers need to support such redirects, and unless each of the redirect domains ( for the portal-ui and for the profiles system) are set here, then the authenticated redirect will fail. You can set ``Web Origins`` to ``+`` which tells KeyCloak to set valids CORS headers ``Redirect Urls``.

    .. figure:: ./_images/kc-edit-dev-portal.png
        :alt: Edit Portal
        :align: center
        :figclass: align-center

    .. figure:: ./_images/kc-edit-dev-portal-2.png
        :alt: Edit Client
        :align: center
        :figclass: align-center

#. Within the client config, switch to the ``Credentials`` tab and regenerate the secret.

    .. figure:: ./_images/kc-edit-dev-portal-secret.png
        :alt: Edit Client
        :align: center
        :figclass: align-center

#. Switch to the ``Installation`` tab in the client section, and choose the format as ``KeyCloak OIDC JSON``. Download the json file.
#. Open the dowloaded json file using your preferred text editor. Copy the variables ``auth-server-url`` to ``url`` and ``resource`` to ``clientId``. It should look similar to the json shown below.

    .. code-block:: JSON
        :linenos:

        {
          "realm": "auth.gawati.local",
          "auth-server-url": "http://localhost:11080/auth",
          "url": "http://localhost:11080/auth",
          "ssl-required": "external",
          "resource": "gawati-portal-ui",
          "clientId": "gawati-portal-ui",
          "credentials": {
            "secret": "b344caaa-7341-479f-81b7-9d47aa3128dc"
          },
          "use-resource-role-mappings": true,
          "confidential-port": 0,
          "policy-enforcer": {}
        }

#. Copy the downloaded ``keycloak.json`` contents into the   ``gawati-portal-fe/configs/auth.json`` file on the portal-fe installation (see :ref:`inst-gawati-portal-fe`.
#. Finally, login as admin into KeyCloak and create some users. You can create a test users like `portaladmin`, `portaleditor`, `portaluser` and associate them with the groups `portalui.Admins`, `portalui.Editors` and `portalui.Editors` .
    
    .. figure:: ./_images/kc-added-user-portal.png
      :alt: Submitter Username
      :align: center
      :figclass: align-center

      Above: a user called ``portaladmin`` has been added.

    .. figure:: ./_images/kc-added-user-group-portal.png
      :alt: Adding user to group
      :align: center
      :figclass: align-center

      Above: the user has been added to the ``portalui.Admins`` group to give it the ``portalui.Admin`` role.


Starting up Services
====================

All the primary components and services need to be started in a specific order because of interconnected dependencies.

The recommended order is as follows:

  * Base Services

    #. KeyCloak
    #. eXist-db
    #. MongoDB
    #. RabbitMQ
  * Component Services
  
    #. :ref:`inst-gawati-portal-fe`
    #. :ref:`inst-gawati-profiles-fe`
    #. :ref:`inst-gawati-profiles-ui`
    #. :ref:`inst-gawati-viewer-service`
    #. :ref:`inst-gawati-portal-ui`

.. _inst-gawati-viewer-service:

Installing Gawati Viewer Service
================================

Extract the contents of the zip file into any directory. 

For development environments, clone from git and install it:

.. code-block:: bash
  :linenos:

  git clone https://github.com/gawati/gawati-viewer-service.git
  npm install 


The Gawati Viewer services are invoked by the gawati-viewer npm package.  The service can convert XML/DOCX to HTML


Running the REST service
---------------------------

Run the following in the extracted folder to setup the server:

Assuming you extracted the portal server into : `/home/web/gawati-viewer-service`, from that folder, run :

.. code-block:: bash
  :linenos:

  node ./bin/www

.. _inst-gawati-profiles:

**************************
Installing Gawati Profiles
**************************

Gawati Profiles allows authenticated users in system to have a profile with their personal information. 
The Profiles system supports other functionality in the system, like allowing logged in users to save their searches.
The profiles system is made up of 3 different components:
  * MongoDB (as mentioned earlier as a pre-requisite)
  * profiles-ui - front-end component
  * profiles-fe  - back-end component

.. _inst-gawati-profiles-fe:

Setting up profiles-fe
======================

Extract the contents of the zip file into any directory. 

For development environments, clone from git and install it:

.. code-block:: bash
  :linenos:

  git clone https://github.com/gawati/gawati-profiles-fe.git
  npm install 

Run the following in the app folder to setup the server:

.. code-block:: bash
  :linenos:

  npm install 

From that folder, run... :

.. code-block:: bash
  :linenos:

  npm start

...to start up the web-service. By default it starts on PORT 9003. You can change that by running it as: 

.. code-block:: bash
  :linenos:

  PORT=11003 npm start


.. _inst-gawati-profiles-ui:

Installing Gawati Profiles UI
=============================

Extract the contents of the zip file onto a directory served by Apache. 

But, if installing for development, clone from git and build:

.. code-block:: bash
  :linenos:

  git clone https://github.com/gawati/gawati-profiles-ui.git
  npm install 

And add the corresponding Apache Server configuration entry (See :ref:`conf-profiles-ui`). 





.. _inst-gawati-editor:

*****************************
Installing Gawati Data Editor
*****************************

Gawati Data Editor (or Gawati Editor  in short), is a tool that allows inputting managing documents in the portal. Gawati is a suite of distributed applications, and the same model applies here for data entry. The :ref:`gawati-portal` has been conceived has a public facing system to access and search for data. The Gawati Editor is a back-office systme that allows managing the process of entering data and publishing it online.

Gawati Editor can be used independent of the :ref:`gawati-portal`, as it has its own working-data store and workflow, and information is published onto the :ref:`gawati-portal` via a asynchronous message queue. 

The Gawati Editor is composed of different components: Editor UI, Editor Server component, Editor Data services(an eXist-db component), and authentication integration component.

See :ref:`gawati-editor` for an architecture overview. 

.. _inst-gawati-editor-ui:

Setting up the Editor UI
========================

To install the Editor UI Component in development environments: 

#. Clone https://github.com/gawati/gawati-editor-ui.git
#. Install packages

    .. code-block:: bash
          :linenos:

          npm install

In case of error during the above step, please refer to https://stackoverflow.com/a/39648550 on instructions on how to resolve the issue

.. _inst-gawati-editor-fe:

Setting Up the Editor FE (Server Component)
===========================================

To install the Editor Server Component in development environments: 

#. Clone https://github.com/gawati/gawati-editor-fe.git
#. Install packages

    .. code-block:: bash
          :linenos:

          npm install


.. _inst-gawati-editor-be:

Installing Gawati Editor BE (Data services Component)
=====================================================

#. Download sample data from here: `Client Sample data (XML)`_, `Client Sample data (PDF)`_

#. Clone https://github.com/gawati/gawati-client-data.git

#. Build to get the package. 

    .. code-block:: bash
      :linenos:

      cd gawati-client-data
      ant xar

    The above generates `gawati-client-data-1.x.xar` package in the ``build`` folder. Install it using the Package Manager in the eXist-db admin dashboard to manually select and install the package (see `Installing Packages in eXist-db`_).
    Alternatively, here is a video that shows how to install a package in eXist-db:

    .. raw:: html

      <iframe width="560" height="315" src="http://www.youtube.com/embed/9AYSWREk24I?rel=0" frameborder="0" allowfullscreen></iframe>


#. Extract and load the `Client Sample data (XML)`_.
   In eXist's dashboard -> Collections, create the path ``/db/docs/gawati-client-data``.

   Now upload the data using the following command run from the eXist-db folder:

    .. code-block:: bash
      :linenos:

      ./bin/client.sh -u gawati-client-data -P <gawati-client-data_password> -d -m /db/docs/gawati-client-data -p <path_to_extracted_data>/gawati-client-data


    or on windows: 

    .. code-block:: bash
      :linenos:

      .\bin\client.bat -u gawati-client-data -P <gawati-client-data_password>  -d -m /db/docs/gawati-client-data -p  <path_to_extracted_data>\gawati-client-data

  the user here is ``gawati-client-data`` which is the user with permissions over the ``/db/docs/gawati-client-data`` collection where we are storing the xml documents. The password for this user is generated during installation and stored in the ``/db/apps/gawati-client-data/_auth/_pw.xml`` file. The same instructions are shown in the video below.

    .. raw:: html

      <iframe width="560" height="315" src="http://www.youtube.com/embed/QOd8tv4Cs_k?rel=0" frameborder="0" allowfullscreen></iframe>

#. Make the necessary Apache conf entries. See :ref:`conf-client`.


Configuring KeyCloak Auth
=========================
#. Follow the installation steps from `Installing Keycloak`_.

#. Within the ``auth.gawati.local`` realm, navigate to the ``Clients`` tab. Click on ``gawati-client``. Set the other parameters as shown below. In this case we have set the root url, valid url etc to http://localhost:3000 which is the dev mode host and port for Gawati Editor UI. If you are deploying on a domain e.g. http://www.domain.org you can set it to that domain.

    .. figure:: ./_images/kc-edit-dev-client.png
        :alt: Edit Client
        :align: center
        :figclass: align-center

    .. figure:: ./_images/kc-edit-dev-client-2.png
        :alt: Edit Client
        :align: center
        :figclass: align-center

#. Within the client, switch to the ``Credentials`` tab and regenerate the secret.

    .. figure:: ./_images/kc-dev-secret.png
        :alt: Edit Client
        :align: center
        :figclass: align-center

#. Switch to the ``Installation`` tab in the client section, and choose the format as ``KeyCloak OIDC JSON``. Download the json file.
#. Open the dowloaded json file using your preferred text editor. Copy the variables ``auth-server-url`` to ``url`` and ``resource`` to ``clientId``. It should look similar to the json shown below.

    .. code-block:: JSON
        :linenos:

        {
          "realm": "auth.gawati.local",
          "auth-server-url": "http://localhost:11080/auth",
          "url": "http://localhost:11080/auth",
          "ssl-required": "external",
          "resource": "gawati-client",
          "clientId": "gawati-client",
          "credentials": {
            "secret": "b344caaa-7341-479f-81b7-9d47aa3128dc"
          },
          "use-resource-role-mappings": true,
          "confidential-port": 0,
          "policy-enforcer": {}
        }

#. Copy the downloaded ``keycloak.json`` contents into the   ``gawati-editor-fe/auth.json`` file on the editor-fe installation (see :ref:`inst-gawati-editor-fe`.
#. Finally, login as admin into KeyCloak and create some users. You can create test users like `submitter`, `editor`, `admin` and associate them with the groups `client.Submitters`, `client.Editors` and `client.Admins` .
    
    .. figure:: ./_images/kc-added-user.png
      :alt: Submitter Username
      :align: center
      :figclass: align-center

      Above: a user called ``submitter`` has been added.


    .. figure:: ./_images/kc-added-user-group.png
      :alt: Adding user to group
      :align: center
      :figclass: align-center

      Above: the user has been added to the ``client.Submitters`` group to give it the ``client.Submitter`` role.


Run Gawati Editor
=================
#. Start eXist
#. Start keycloak

    .. code-block:: bash
      :linenos:

      cd keycloak-3.4.3.Final
      ./bin/standalone.sh

#. Start gawati-editor-fe service. Use the ``dev_npm_start`` scripts to start the service in development node. 

    .. code-block:: bash
      :linenos:

      cd gawati-editor-fe
      ./dev_npm_start.sh # .\dev_npm_start.bat on windows

#. Start gawati-editor-ui

    .. code-block:: bash
      :linenos:

      cd gawati-editor-ui
      npm start 

#. Load http://localhost:3000 in the browser. You should see a login screen. Login with any of the users you created.

    .. figure:: ./_images/gawati-client-login.png
      :alt: Login
      :align: center
      :figclass: align-center

      Above: Login screen for gawati-editor

#. After logging in, you should be able to see the dashboard with some sample documents.

    .. figure:: ./_images/gawati-client-dashboard.png
      :alt: Dashboard
      :align: center
      :figclass: align-center


.. _gawati-portal-ui: https://github.com/gawati/gawati-portal-ui
.. _gawati-portal-server: https://github.com/gawati/gawati-portal-server
.. _Full Text Data set: https://github.com/gawati/gawati-data/releases/download/1.14/akn_xml_ft_sample_1.14.zip
.. _XML Data set: https://github.com/gawati/gawati-data/releases/download/1.14/akn_xml_docs_sample_1.14.zip
.. _PDF Data set: https://github.com/gawati/gawati-data/releases/download/1.14/akn_xml_pdf_sample-1.14.zip
.. _Client Sample data (XML): https://github.com/gawati/gawati-client-data/releases/download/1.4/akn_xml_docs_sample-1.4.zip
.. _Client Sample data (PDF): https://github.com/gawati/gawati-client-data/releases/download/1.4/akn_pdf_docs_sample-1.4.zip
.. _Installing Keycloak: http://docs.gawati.org/en/latest/development/authentication.html#installing-configuring-keycloak-for-development
.. _Installing Packages in eXist-db: https://exist-db.org/exist/apps/doc/dashboard.xml#D2.4.8
.. _Model Realm: https://github.com/gawati/gawati-keycloak-scripts/blob/dev/model_realm/model-realm.json
