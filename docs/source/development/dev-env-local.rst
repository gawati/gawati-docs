Gawati as a local installation
##############################

If you haven't yet, please review the Gawati Architecture page: :doc:`../system/gawati-architecture`.

The below instructions are not specific to an operating system, the components will run on different operating systems.

.. note::
  **Pre-requisites:**
    1. **JDK 1.8 LTS** - On Linux operating systems you can install `OpenJDK8 <http://openjdk.java.net/install/>`_; For `Windows <https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html#CHDEBCCJ>`_ and for `Mac OSX <https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#CHDBADCG>`_ and `Using OS X Homebrew <https://stackoverflow.com/questions/24342886/how-to-install-java-8-on-mac/28635465#28635465>`_
    2. **NodeJS LTS 8.11.x** - See `NodeJS LTS 8.11.x <https://nodejs.org/en/download/>`_. Alternatively you can install NVM (for `Linux <https://github.com/creationix/nvm/>`_ , `Windows <https://github.com/coreybutler/nvm-windows>`_ ) which lets you easily install parallel versions of NodeJS. 
    3. **eXist-db 3.4.1**: Download and install eXist-db 3.4.1, see `eXist-db <https://bintray.com/existdb/releases/exist/3.4.1/view>`_ Remember to note down the admin password of the eXist-db installation, you will need that later.   If you are installing eXist-db on Mac OS X, install it within the User folder, installing it in ``/Applications`` causes problems sometimes as the permissions required for eXist-db to write to the file system are for a super user.  
    4. **Ant**: Download and `install Ant <http://ant.apache.org/manual/install.html#installing>`_ 
    5. **Apache HTTPD 2.4.x**: Install Apache, on Cent OS, Ubuntu and OS X this will likely be installed by default, on Windows you will have to download and install, see `Apache for Windows <https://www.apachehaus.com/cgi-bin/download.plx>`_; enable `mod_alias`, `mod_rewrite`, `mod_proxy`, `mod_proxy_http` and enable htaccess.
    6. **KeyCloak 3.4.1**: Authentication server, see :doc:`./authentication` 
    7. *Visual Studio Code*: This is if you want play around with gawati code. Download, and setup Visual Studio Code (there are versions for Windows, OS X and Linux) for development, see `VS Code Setup <./using-vscode.rst>`_


.. contents:: Table of Contents 
  :local:

You can either build the source from github for each component, or you can install a released version of a component. For getting familiar with the system we recommend starting by installing a released version.


************************
Installing Gawati Portal
************************

Gawati Portal provides access to all legal and legislative data in the system.
See :ref:`gawati-portal-arch` for an architecture overview. 

.. note::
  .. include:: version-info.rst

**IMPORTANT**: Kindly read how Apache HTTP is used a crucial glue between the different components of Gawati :doc:`./dev-and-prod-testing`

Installing Gawati Data and Gawati Data XML
==========================================

.. note::
  .. include:: note-gawati-data.rst

Place the ``gawati-data`` and ``gw-data`` XAR files in the `autodeploy` folder within the eXist installation, and restart the eXist database server. 
They will be automatically installed.

Load Sample Data
----------------
.. note::
  The sample data is currently at version 1.2

To understand better how gawati works, we provide you with sample data, which can be loaded into the system and tested. Sample data is provided in two specific parts:

 * Xml Documents - which get loaded into the XML database (i.e. into *Gawati Data XML*) 
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

  <path_to_exist>/bin/client.sh -ouri=xmldb:exist://localhost:8080/exist/xmlrpc -u admin -P <exist_admin_password> -x "data(doc('/db/apps/gw-data/_auth/_pw.xml')/users/user[@name = 'gwdata']/@pw)"

Where ``<path_to_exist>`` is the path to the eXist-db installation, and ``<exist_admin_password>`` is the eXist-db admin password. If you installed eXist on a different port change that in the ``-ouri`` setting.

On Windows do the following; Start the eXist-db Client(``<path_to_exist>/bin/client.bat``). In the command window of the eXist-db client run the following commands:

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
  ./bin/client.sh -u gwdata -P <copied_password_hash> -d -m /db/apps/gw-data/akn_ft -p /home/data/akn_xml/akn_ft
  

On Windows you will run it as :samp:``.\\bin\\client.bat`` instead:

.. code-block:: bash
  :linenos:

  .\bin\client.bat -u gwdata -P <copied_password_hash> -d -m /db/apps/gw-data/akn -p d:\data\akn_xml\akn
  .\bin\client.bat -u gwdata -P <copied_password_hash> -d -m /db/apps/gw-data/akn_ft -p /home/data/akn_xml/akn_ft

.. note::
  if you get a password failure, log in to eXist-db as admin, and reset the password for gwdata user manually, and then use that password.

Apache Config
-------------

There are Apache HTTP configs required for both serving XML and PDF documents. See :ref:`conf-gawati-data` and :ref:`conf-binary`


Installing Gawati Portal UI
===========================

Extract the contents of the zip file onto a directory served by Apache. 
And add the corresponding Apache Server configuration entry (See :ref:`conf-portal-ui`). 


Development and Production mode
-------------------------------

See our detailed guide on setting up your environment for production and development mode testing :doc:`./dev-and-prod-testing`.

For setting up Authentication, click here:  :doc:`Authentication <./authentication>`


Installing Gawati Portal Server
===============================

Extract the contents of the zip file into any directory. 
The Gawati Portal has two runnable components, the portal http server which provides access to REST services, and a cron component that runs scheduled tasks periodically. 


Running the REST service
---------------------------

Run the following in the extracted folder to setup the server:

.. code-block:: bash
  :linenos:

  npm install 

Assuming you extracted the portal server into : `/home/web/portal-server`, from that folder, run :

.. code-block:: bash
  :linenos:

  node ./bin/www

To start up the web-service. By default it starts on PORT 9001. You can change that by running it as: 

.. code-block:: bash
  :linenos:

  PORT=11001 node ./bin/www


Running the cron service
------------------------

This is started by simply running: 

.. code-block:: bash
  :linenos:
  
  node ./cron.js


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

Accessing eXist-db via SSH tunnelling
-------------------------------------

If eXist-db is installed in a remote server, by default the server starts on port 8080 and listens only to localhost.
To access the web-based dashboard from a remote computer, you need to use ssh tunneling. For example, if your remote server  is on the I.P. Address `101.102.103.104`, and eXist-db is on port `8080`, running the following command, will give you access to the eXist-db dashboard on `http://localhost:9999` :

.. code-block:: bash
  :linenos:

   ssh -vv -i <path to private key> -p 22 -L 9999:127.0.0.1:8080 server_user@101.102.103.104


************************
Installing Gawati Client
************************

The Gawati Client is a service that enables data input and management in Gawati. It has four components: Client UI, Client Server, Client Data (an eXist-db component), and the Keycloak Auth component.

See :ref:`gawati-client-arch` for an architecture overview. 


Setting up the Client UI
========================

#. Clone https://github.com/gawati/gawati-client.git
#. Install packages

    .. code-block:: bash
          :linenos:

          npm install


Setting up the Client Server
============================
#. https://github.com/gawati/gawati-client-server.git
#. Install packages

    .. code-block:: bash
          :linenos:

          npm install


Installing Gawati Client Data
=============================
#. Clone https://github.com/gawati/gawati-client-data.git
#. Build to get the package. 

    .. code-block:: bash
      :linenos:

      cd gawati-client-data
      ant xar

    The above generates `gawati-client-data-1.0.xar` package in the ``build`` folder. Place the xar file in the ``autodeploy`` folder within the eXist installation, and restart the eXist database server. They will be automatically installed.

#. Extract and load the `Client Sample data`_.
   In eXist's dashboard -> Collections, create the path ``/db/docs/gawati-client-data``.

   Now upload the data using the following command run from the eXist-db folder:

    .. code-block:: bash
      :linenos:

      ./bin/client.sh -u admin -P <admin_password> -d -m /db/docs/gawati-client-data/akn -p <path_to_extracted_data>/akn

#. Make the necessary Apache conf entries. See :ref:`conf-client`.


Installing Keycloak Auth
========================
#. Follow the installation steps 1 - 6 from `Installing Keycloak`_.
#. Clone https://github.com/gawati/gawati-keycloak-scripts.git
#. Generate a new development realm using the command:

    .. code-block:: bash
      :linenos:

      cd gawati-keycloak-scripts
      node index.js --new_realm_name=Ethiopia --input_realm=model_realm/model-realm.json --output_file=ethiopia.json

#. Switch back to the administration console in the browser
#. Create a dev realm by importing configuration from `ethiopia.json` generated above.

    .. figure:: ./_images/kc-add-dev-realm.png
        :alt: Add Realm
        :align: center
        :figclass: align-center

#. Within the ``Ethiopia`` realm, navigate to the ``Clients`` tab. Click on ``gawati-client``. Set the other parameters as shown below. In this case we have set the root url, valid url etc to http://localhost:3000 which is the dev mode host and port for gawat-client UI. If you are deploying on a domain e.g. http://www.domain.org you can set it to that domain.

    .. figure:: ./_images/kc-edit-dev-client.png
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
          "realm": "Ethiopia",
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

#. Copy the downloaded ``keycloak.json`` contents into  ``gawati-client/src/configs/authRealm.json`` and ``gawati-client-server/auth.json``.
#. Finally, go to ``Realm Settings => Login`` and set ``User Registration`` to ``on`` and set ``Email as User name`` to ``on``.

    .. figure:: ./_images/kc-dev-login.png
      :alt: Login
      :align: center
      :figclass: align-center


Run Gawati Client
=================
#. Start eXist
#. Start keycloak

    .. code-block:: bash
      :linenos:

      cd keycloak-3.4.3.Final
      ./bin/standalone.sh

#. Start gawati-client-server

    .. code-block:: bash
      :linenos:

      cd gawati-client-server
      node ./bin/www

#. Start gawati-client

    .. code-block:: bash
      :linenos:

      cd gawati-client
      npm start

#. Load http://localhost:3000 in the browser. You should see a login screen. Register a new user.

    .. figure:: ./_images/gawati-client-login.png
      :alt: Login
      :align: center
      :figclass: align-center

#. After logging in, you should be able to see the dashboard with the two sample documents.

    .. figure:: ./_images/gawati-client-dashboard.png
      :alt: Dashboard
      :align: center
      :figclass: align-center


.. _gawati-portal-ui: https://github.com/gawati/gawati-portal-ui
.. _gawati-portal-server: https://github.com/gawati/gawati-portal-server
.. _Full Text Data set: https://github.com/gawati/gawati-data-xml/releases/download/1.6/akn_xml_ft_sample.zip
.. _XML Data set: https://github.com/gawati/gawati-data-xml/releases/download/1.2/akn_xml_sample-1.2.zip
.. _PDF Data set: https://github.com/gawati/gawati-data-xml/releases/download/1.2/akn_pdf_sample-1.2.zip
.. _Client Sample data: https://github.com/gawati/gawati-client-data/releases/download/1.0/gawati-client-data-sample.zip
.. _Installing Keycloak: http://docs.gawati.org/en/latest/development/authentication.html#installing-configuring-keycloak-for-development
.. _Model Realm: https://github.com/gawati/gawati-keycloak-scripts/blob/dev/model_realm/model-realm.json
