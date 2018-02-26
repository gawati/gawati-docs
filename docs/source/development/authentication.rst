########################
Authentication in Gawati
########################

Authentication in Gawati is handled by a separate component service called `KeyCloak`. 
Keycloak is a solution for Identity Management (IDM) and Single Sign On (SSO); See `KeyCloak <http://www.keycloak.org/>`_ .

Keycloak has the following components:
 * Keycloak server – It is an authentication server to store the user information and used to get the access token. We need to just redirect client to keycloak server.
 * Keycloak adapter – It is a componenet which help us to integrate keycloak in the existing application. 

It needs to be installed and integrated to work with Gawati. 


*********************************
Installing & Configuring KeyCloak
*********************************

Prerequisites: 

 1) Java 8 JDK
 2) zip or gzip and tar
 3) At least 512M of RAM
 4) At least 1 GB of diskspace

Installation Steps:

 1) Install the Java 8 JDK
 2) Visit http://www.keycloak.org/downloads.html  and download  `KeyCloak 3.4.3 Final <https://downloads.jboss.org/keycloak/3.4.3.Final/keycloak-3.4.3.Final.zip>`_. 
 3) Unzip this and move to bin directory.
 4) Run `standalone.sh` (or in windows `standalone.bat`). By default it starts on port 8080. You should change the default port as it clashes with the default ports of eXist-db. You will need to do that in `standalone/configuration/standalone.xml` : 
        
    .. code-block:: xml

    <socket-binding-group name="standard-sockets" default-interface="public" port-offset="${jboss.socket.binding.port-offset:0}">
        ...
        <socket-binding name="http" port="${jboss.http.port:11080}"/>
        <socket-binding name="https" port="${jboss.https.port:11443}"/>
        ...
    </socket>

 5) Restart the service and visit the link : `http://localhost:11080` 
 6) Click on the administration console. Login with the admin and admin.
 7) Create a realm called `gawati`: 
    
    .. figure:: ./_images/kc-add-realm.png
    :alt: Add Realm
    :align: center
    :figclass: align-center
 
 8) If you are getting a https related error. You can disable it from command line
  * `./bin/add-user-keycloak.sh -r master -u <user> -p <password>`
  * `./bin/kcadm.sh config credentials --server http://localhost:11080/auth --realm master --user <user> --password <password>`
  * `./bin/kcadm.sh update realms/master -s sslRequired=NONE`
  * Restart the server
 9) Within the `gawati` realm, Navigate to client tab and click new client. Fill the name of client (`gawati-portal-ui`), the client root url and hit save:
    
    .. figure:: ./_images/kc-add-client.png
    :alt: Add Client
    :align: center
    :figclass: align-center
 
 10) Now edit the same  `gawati-portal-ui` client document, and set the other parameters as shown below. In this case we have set the root url, valid url etc to `http://localhost:3000` which is the dev mode host and port for the `gawati-portal-ui`, if you are deploying on `localhost` and apache you can set this to `http://localhost`. Correspondingly if you are deploying on a domain e.g. `http://www.domain.org` you can set it to that domain. 

   .. figure:: ./_images/kc-edit-client.png
    :alt: Add Client
    :align: center
    :figclass: align-center
 
 11) Switch to the `Installation` tab in the client section, and choose the format as `KeyCloak OIDC JSON`. Change the following variables, `auth-server-url` to `url` and change `resource` to `clientId`:

   .. code-block:: JSON

    {
     "realm": "gawati",
     "url": "http://localhost:11080/auth",
     "ssl-required": "external",
     "clientId": "gawati-portal-ui",
     "public-client": true,
     "confidential-port": 0
    }

 Save it is `keycloak.json` into the `gawati-portal-ui` `src/configs` folder. Note that, you don't need to do this, if you have the above defaults as the portal ships with `keycloak.json` with the same contents.
 12) Finally, go to `Realm Settings => Login` and set `User Registration` to `on` and set `Email as User name` to `on`. 

   .. figure:: ./_images/kc-login.png
    :alt: Login
    :align: center
    :figclass: align-center
 


