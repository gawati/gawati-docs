########################
Authentication in Gawati
########################

Authentication in Gawati is handled by a separate component service called `KeyCloak`. 
Keycloak is a solution for Identity Management (IDM) and Single Sign On (SSO); See `KeyCloak <http://www.keycloak.org/>`_ .

Keycloak has the following components:
 * Keycloak server – an authentication server to store the user information and used to get the access token.
 * Keycloak adapter – It is a componenet which help us to integrate keycloak in the existing application. 

It needs to be installed and integrated to work with Gawati. For an overview of how KeyCloak works in Gawati, see `How Trust and Security works in Gawati <../system/how-authentication.rst>`_

************************************************
Installing & Configuring KeyCloak for Production
************************************************

-----
Setup
-----

The following instructions deploy keycloak behind an Apache reverse proxy and SSL.

#. Download `KeyCloak 3.4.3 Final <https://downloads.jboss.org/keycloak/3.4.3.Final/keycloak-3.4.3.Final.zip>`_ and unzip into the `/opt` folder. (We are assuming these commands are run as root)

   .. code-block:: bash

      cd /opt
      wget https://downloads.jboss.org/keycloak/3.4.3.Final/keycloak-3.4.3.Final.zip
      unzip keycloak-3.4.3.Final.zip


#. Add a user for keycloak called `keycloak`, and change the ownership of the unzipped files to this user.

   .. code-block:: bash

      sudo chown keycloak: -R keycloak-3.4.3.Final


#. Add a keycloak ``admin`` to ``/opt/keycloak-3.4.3.Final/standalone/configuration/keycloak-add-user.json``.

   .. code-block:: bash

      # run this in /opt/keycloak-3.4.3.Final if you are not already there
      # run ``su keycloak`` to run this as the ``keycloak`` user
      
      ./bin/add-user-keycloak.sh --user admin --password <set an appropriate password> --realm master


#. Modify ``standalone/configuration/standalone.xml`` to enable proxying to Keycloak:

   Run these 3 commands as the ``keycloak`` user:

   .. code-block:: sh
      
      # enable proxy forwarding, we need to forward traffic from port 8443 the secure port of keycloak to port 443 on Apache
      ./bin/jboss-cli.sh 'embed-server,/subsystem=undertow/server=default-server/http-listener=default:write-attribute(name=proxy-address-forwarding,value=true)'
      # add the redirect-socket attribute to enable https proxy forwarding
      ./bin/jboss-cli.sh 'embed-server,/subsystem=undertow/server=default-server/http-listener=default:write-attribute(name=redirect-socket,value=proxy-https)'
      # enable forwarding to port 443
      ./bin/jboss-cli.sh 'embed-server,/socket-binding-group=standard-sockets/socket-binding=proxy-https:add(port=443)'


#. Add a service startup config ``keycloak.service`` for systemd to start keycloak as a service:

   .. code-block:: none

      [Unit]
      Description=KeyCloak
      After=network.target

      [Service]
      Type=idle
      User=keycloak
      Group=keycloak
      ExecStart=/opt/keycloak-3.4.3.Final/bin/standalone.sh -b 127.0.0.1
      TimeoutStartSec=600
      TimeoutStopSec=600

      [Install]
      WantedBy=multi-user.target


   Now enable the service:

   .. code-block:: bash

      systemctl daemon-reload
      systemctl start keycloak.service


   Now check if the service is running:

   .. code-block:: bash

      systemctl status keycloak.service


#. Now add the appropriate configuration to apache.

   .. code-block:: apacheconf

        <IfModule mod_ssl.c>
        <VirtualHost *:443>

            ProxyRequests off
            ServerName auth.xyz.com
            ServerAlias auth.xyz.com

            ErrorLog ${APACHE_LOG_DIR}/error_auth.log
            CustomLog ${APACHE_LOG_DIR}/access_auth.log combined

            ProxyPreserveHost On
            ProxyPass / http://localhost:8080/
            ProxyPassReverse / http://localhost:8080/

            RequestHeader set X-Forwarded-Proto "https"
            RequestHeader set X-Forwarded-Port "443"

            SSLCertificateFile /etc/letsencrypt/live/auth.xyz.com/fullchain.pem
            SSLCertificateKeyFile /etc/letsencrypt/live/auth.xyz.com/privkey.pem
            Include /etc/letsencrypt/options-ssl-apache.conf
        </VirtualHost>
        </IfModule>


  .. note::
    If you only wish to install and test the system, See :doc:`Setup <../setup/index>`.
    Here we created the SSL Certificates using ``letsencrypt``. Instructions for setting up signed SSL Certificates can be found here:

     * On `Ubuntu 16.04 <https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04>`_
     * On `CentOS 7 <https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-centos-7>`_ 


---------------------------
Installing the Gawati Theme
---------------------------

KeyCloak is themed independently of Gawati. 

#. Download the `gawati keycloak theme <https://github.com/gawati/gawati-keycloak-theme/releases/download/1.0.0/gawati-keycloak-theme-1.0.0.zip>`_images

#. Go to the ``themes`` folder, and extract the gawati theme into a folder called ``gawati``.

#. Navigate to ``standalone/configuration/standalone.xml`` and add, a ``<welcomeTheme>`` with the value ``gawati``.

   .. code-block:: xml

        <theme>
            <staticMaxAge>2592000</staticMaxAge>
            <cacheThemes>true</cacheThemes>
            <cacheTemplates>true</cacheTemplates>
            <welcomeTheme>gawati</welcomeTheme>
            <dir>${jboss.home.dir}/themes</dir>
        </theme>


    .. note::
          You can set ``cacheThemes`` and ``cacheTemplates`` to ``false`` for development purposes


#. Change the ``Display Name`` and the ``HTML Display Name``




*************************************************
Installing & Configuring KeyCloak for Development
*************************************************

-------------
Prerequisites
-------------

 1) Java 8 JDK
 2) zip or gzip and tar
 3) At least 512M of RAM
 4) At least 1 GB of diskspace

------------------
Installation Steps
------------------

#. Install the Java 8 JDK

#. Visit http://www.keycloak.org/downloads.html  and download  `KeyCloak 3.4.3 Final <https://downloads.jboss.org/keycloak/3.4.3.Final/keycloak-3.4.3.Final.zip>`_. 

#. Unzip this and move to ``bin`` directory.

    .. note::
        To prevent KeyCloak from hanging due to lack of available entropy, change the jvm to use ``urandom`` instead of ``random``:
        
        * Open the ``$JAVA_HOME/jre/lib/security/java.security`` file in a text editor.
        * Change the line:
            - Change the entry ``securerandom.source=file:/dev/random`` to read: 
            - ``securerandom.source=file:/dev/urandom`` ; Save your change and exit the text editor.


#. Run ``standalone.sh`` (or in windows ``standalone.bat``). By default it starts on port 8080. You should change the default port as it clashes with the default ports of eXist-db. You will need to do that in `standalone/configuration/standalone.xml`.

    .. code-block:: xml

        <socket-binding-group name="standard-sockets" default-interface="public" port-offset="${jboss.socket.binding.port-offset:0}">
            ...
            <socket-binding name="http" port="${jboss.http.port:11080}"/>
            <socket-binding name="https" port="${jboss.https.port:11443}"/>
            ...
        </socket>


#. Restart the service and visit the link : ``http://localhost:11080`` 

#. Click on the administration console. Login with the admin and admin.

#. Create a test realm called `gawati`: 
    
    .. figure:: ./_images/kc-add-realm.png
     :alt: Add Realm
     :align: center
     :figclass: align-center
  
    .. note::
        If you are getting a https related error. You can disable it from command line

            .. code-block:: sh
            
              ./bin/add-user-keycloak.sh -r master -u <user> -p <password>
              ./bin/kcadm.sh config credentials --server http://localhost:11080/auth --realm master --user <user> --password <password>
              ./bin/kcadm.sh update realms/master -s sslRequired=NONE
             
        
        Restart the server


#. Within the ``gawati`` realm, Navigate to client tab and click new client. Fill the name of client (``gawati-portal-ui``), the client root url and hit save:
    
    .. figure:: ./_images/kc-add-client.png
     :alt: Add Client
     :align: center
     :figclass: align-center
 

#. Now edit the same  ``gawati-portal-ui`` client document, and set the other parameters as shown below. In this case we have set the root url, valid url etc to `http://localhost:3000` which is the dev mode host and port for the `gawati-portal-ui`, if you are deploying on `localhost` and apache you can set this to ``http://localhost``. Correspondingly if you are deploying on a domain e.g. ``http://www.domain.org`` you can set it to that domain. 

   .. figure:: ./_images/kc-edit-client.png
    :alt: Add Client
    :align: center
    :figclass: align-center


#. Switch to the ``Installation`` tab in the client section, and choose the format as ``KeyCloak OIDC JSON``. Change the following variables, ``auth-server-url`` to ``url`` and change ``resource`` to ``clientId``:
 
    .. code-block:: JSON
        :linenos:

        {
            "realm": "gawati",
            "url": "http://localhost:11080/auth",
            "ssl-required": "external",
            "clientId": "gawati-portal-ui",
            "public-client": true,
            "confidential-port": 0
        }


   Save it is ``keycloak.json`` into the ``gawati-portal-ui`` ``src/configs`` folder. Note that, you don't need to do this, if you have the above defaults as the portal ships with ``keycloak.json`` with the same contents.

#. Finally, go to ``Realm Settings => Login`` and set ``User Registration`` to ``on`` and set ``Email as User name`` to ``on``. 

   .. figure:: ./_images/kc-login.png
    :alt: Login
    :align: center
    :figclass: align-center


    

