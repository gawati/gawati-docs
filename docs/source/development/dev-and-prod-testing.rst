Testing Development & Production modes side by side
###################################################

This document is specific to React based components in Gawati. Specifically ``gawati-portal-ui`` and ``gawati-editor-ui``. 

The standard procedure for development testing is to run ``npm start`` and the React application starts up on port ``3000``, for production testing we run ``npm run build`` and the built output is provided in the ``build`` folder, from where it is served via Apache httpd (explained below see :ref:`gawati-local`. For the application routing we use React Router, which intercepts client URL requests and channels them appropriately, however this causes problems when we proxy the data back-end on the same domain as the main system. 

For instance, if the production site is running on ``http://www.domain.com`` , and the data back-end is being reverse proxied on ``http://www.domain.com/gwd/*`` and the media files (AKN pdf files) are being reverse proxied on ``http://www.domain.com/akn/*`` -- in this scenario direct browser requests for PDF files will with a 404 because React Router detects that there is no specifed route to the ``/akn`` path. 

For this reason, we recommend running the data-backend and media services on distinct domains or sub-domains. 

For local production and development testing we have specified a setup below. 

.. contents:: Table of Contents 
  :local:


**********************
Specify multiple hosts
**********************

Specify distinct hosts for the front-end, back-end services and the media, all mapped to the localhost IP. Below we have specified 3 distinct hosts. In Linux the hosts file is typically ``/etc/hosts`` on Windows you will need to open `C:/Windows/System32/drivers/etc/hosts`, and add the settings there.

.. code-block:: apacheconf
    :linenos:

    127.0.0.1       gawati.local  # front-end
    127.0.0.1       data.local    # data services
    127.0.0.1       media.local   # media files
    127.0.0.1       auth.local    # auth / keycloak if you want be a purist

Then specify in your apache vhosts conf file, configurations for each host as follows. 

 .. _gawati-local:
 
*************************************************
gawati.local : front-end (for production testing)
*************************************************

This is relevant only for production testing. When you run ``npm start`` in development mode, the front-end runs on ``localhost:3000`` and these settings are ignored. But when you build the production system using `` npm run build `` the production system can be accessed via the ``http://gawati.local`` url.

.. code-block:: apacheconf
    :linenos:

    <VirtualHost 127.0.0.1:80>
        ProxyRequests off
        ServerName gawati.local
        ServerAlias gawati.local

        DocumentRoot "/path/to/portal-ui/build"

        <Directory "/path/to/portal-ui/build">
            DirectoryIndex "index.html"
            Require all granted
            AllowOverride All
            
            ## the following is required for client side routing to work, 
            ## otherwise results in a 404 on refresh
            
            RewriteEngine on
            # Don't rewrite files or directories
            RewriteCond %{REQUEST_FILENAME} -f [OR]
            RewriteCond %{REQUEST_FILENAME} -d
            RewriteRule ^ - [L]
            # Rewrite everything else to index.html to allow html5 state links
            RewriteRule ^ index.html [L]
        </Directory>

    </VirtualHost>


 .. _data-local:


*********************************************************************
data.local : services back-end (for development & production testing)
*********************************************************************

We proxy the eXist-db services on the data.local host. Data is provided by different services running on different ports, ``data.local`` provides a single point proxy to access all these data services. Each data service is mapped to a different proxy root path: 

    * ``/gwd``: ``gawati-data`` an eXist-db service used by :ref:`gawati-portal`
    * ``/gwp`` : ``gawati-portal-fe`` a node js service used by :ref:`gawati-portal`
    * ``/gwdc`` : ``gawati-client-data`` an eXist-db service used by :ref:`gawati-editor`
    * ``/gwc`` : ``gawati-editor-fe`` a node js service used by :ref:`gawati-editor`
    * ``/gwu`` : ``gawati-profiles-fe`` user service for profiles used by :ref:`gawati-portal`, :ref:`gawati-profiles`

The full apache configuration is shown below. 

.. code-block:: apacheconf
    :linenos:

    <VirtualHost 127.0.0.1:80>
        ProxyRequests off
        ServerName data.local
        ServerAlias data.local

        ### CORS BEGIN    
        # Always set these headers.
        <IfModule mod_headers.c>
            SetEnvIfNoCase Origin "https?://(www\.)?(localhost|gawati\.local)(:\d+)?$" AccessControlAllowOrigin=$0
            Header set Access-Control-Allow-Origin %{AccessControlAllowOrigin}e env=AccessControlAllowOrigin
        </IfModule>
        Header always set Access-Control-Allow-Methods "POST, GET, OPTIONS, DELETE, PUT"
        Header always set Access-Control-Max-Age "1000"
        Header always set Access-Control-Allow-Headers "x-requested-with, Content-Type, origin, authorization, accept, client-security-token"
        
        # Added a rewrite to respond with a 200 SUCCESS on every OPTIONS request.
        RewriteEngine On
        RewriteCond %{REQUEST_METHOD} OPTIONS
        RewriteRule ^(.*)$ $1 [R=200,L]
        ### CORS END

        # gawati-data
        <Location ~ "/gwd/(.*)">
        AddType text/cache-manifest .appcache
        ProxyPassMatch  "http://localhost:8080/exist/restxq/gw/$1"
        ProxyPassReverse "http://localhost:8080/exist/restxq/gw/$1"
        ProxyPassReverseCookiePath /exist /
        SetEnv force-proxy-request-1.0 1
        SetEnv proxy-nokeepalive 1
        </Location>

        # gawati-portal-fe
        <Location ~ "/gwp/(.*)">
        AddType text/cache-manifest .appcache
        ProxyPassMatch  "http://localhost:9001/gwp/$1"
        ProxyPassReverse "http://localhost:9001/gwp/$1"
        SetEnv force-proxy-request-1.0 1
        SetEnv proxy-nokeepalive 1
        </Location>

        # gawati-client-data
        <Location ~ "/gwdc/(.*)">
        AddType text/cache-manifest .appcache
        ProxyPassMatch  "http://localhost:8080/exist/restxq/gwdc/$1"
        ProxyPassReverse "http://localhost:8080/exist/restxq/gwdc/$1"
        SetEnv force-proxy-request-1.0 1
        SetEnv proxy-nokeepalive 1
        </Location>

        # gawati-profiles-fe
        <Location ~ "/gwu/(.*)">
        AddType text/cache-manifest .appcache
        ProxyPassMatch  "http://localhost:9003/gwu/$1"
        ProxyPassReverse "http://localhost:9003/gwu/$1"
        SetEnv force-proxy-request-1.0 1
        SetEnv proxy-nokeepalive 1
        </Location>

        # gawati-editor-fe
        <Location ~ "/gwc/(.*)">
        AddType text/cache-manifest .appcache
        ProxyPassMatch  "http://localhost:9002/gwc/$1"
        ProxyPassReverse "http://localhost:9002/gwc/$1"
        SetEnv force-proxy-request-1.0 1
        SetEnv proxy-nokeepalive 1
        </Location>

        # gawati-xslt-transformer
        <Location ~ "/gwx/(.*)">
        AddType text/cache-manifest .appcache
        ProxyPassMatch  "http://localhost:9005/Gawati/xml/xslt/$1"
        ProxyPassReverse "http://localhost:9005/Gawati/xml/xslt/$1"
        SetEnv force-proxy-request-1.0 1
        SetEnv proxy-nokeepalive 1
        </Location>

        # gawati-viewer-service
        <Location ~ "/gwv/(.*)">
        AddType text/cache-manifest .appcache
        ProxyPassMatch  "http://localhost:9005/gwv/$1"
        ProxyPassReverse "http://localhost:9005/gwv/$1"
        SetEnv force-proxy-request-1.0 1
        SetEnv proxy-nokeepalive 1
        </Location>

    </VirtualHost>    

.. note::
    Note the use of the CORS header in ``data.local``, specifically:

    .. code-block:: apacheconf

        <IfModule mod_headers.c>
            SetEnvIfNoCase Origin "https?://(www\.)?(localhost|gawati\.local)(:\d+)?$" AccessControlAllowOrigin=$0
            Header set Access-Control-Allow-Origin %{AccessControlAllowOrigin}e env=AccessControlAllowOrigin
        </IfModule>
    
    Which allows requests coming in from both ``localhost:3000`` and ``gawati.local`` hosts.

*********************************************************************
media.local : media files (for development & production testing)
*********************************************************************

The Akoma Ntoso PDF and thumbnail files are served via this host. 

.. code-block:: apacheconf
    :linenos:

    <VirtualHost 127.0.0.1:80>
        ProxyRequests off
        ServerName media.local
        ServerAlias media.local

        ### CORS BEGIN    
        # Always set these headers.
        <IfModule mod_headers.c>
        SetEnvIfNoCase Origin "https?://(www\.)?(localhost|gawati\.local)(:\d+)?$" AccessControlAllowOrigin=$0
        Header set Access-Control-Allow-Origin %{AccessControlAllowOrigin}e env=AccessControlAllowOrigin
        </IfModule>
        Header always set Access-Control-Allow-Methods "POST, GET, OPTIONS, DELETE, PUT"
        Header always set Access-Control-Max-Age "1000"
        Header always set Access-Control-Allow-Headers "x-requested-with, Content-Type, origin, authorization, accept, client-security-token"
        
        # Added a rewrite to respond with a 200 SUCCESS on every OPTIONS request.
        RewriteEngine On
        RewriteCond %{REQUEST_METHOD} OPTIONS
        RewriteRule ^(.*)$ $1 [R=200,L]
        ### CORS END

        Alias /akn "/path/to/akn"
        <Directory "/path/to/akn">	
            DirectoryIndex "index.html"
            Require all granted
            AllowOverride All
        </Directory>

    </VirtualHost>


*************************************
Set the PROXY Params in the front-end
*************************************

In ``public/index.html`` set the 2 proxy parameters as below.

.. code-block:: html

    <script>
      gawati = {
        GAWATI_PROXY: "http://data.local",
        GAWATI_DOCUMENT_SERVER: "http://media.local"
      };
    </script>


***********************************
Set the PROXY Param in package.json
***********************************

This parameter is used only in development mode, set it to ``http://data.local`` 

.. code-block:: json

    {
    "name": "gawati-portal-ui",
    "version": "2.0.22",
    "private": true,
    "proxy": "http://data.local",  <<<<<<
    ....
    }


With all this set-up restart Apache HTTPD and you are all set to use both development and production mode testing side by side. 

************************
Apache configs explained 
************************

.. _conf-portal-ui:

Apache configuration for portal-ui
----------------------------------

For example: if you want to serve the portal from the `/ui` virtual directory of your domain, and your files are located in `/home/web/apps/gawati-portal-ui`, then use the following apache configuration --  

.. include:: portal-ui-conf.rst

.. _conf-profiles-ui:

Apache configuration for profiles-ui
----------------------------------

For example: if you want to serve the portal from the `/profiles` virtual directory of your domain, and your files are located in `/home/web/apps/gawati-profiles-ui`, then use the following apache configuration --  

.. include:: profiles-ui-conf.rst


.. _conf-binary:

Apache configuration for binary data
------------------------------------

The Apache configuration will allow accessing gawati data server services over a web-browser using the URL:

To do this, open the `httpd.conf` (or equivalent) file of your apache installation and add the following:

.. code-block:: apacheconf
  :linenos:

    Alias /akn "/home/data/akn_pdf"
    <Directory "/home/data/akn_pdf">
      Require all granted
      Options Includes FollowSymLinks
      AllowOverride All
      Order allow,deny
      Allow from all
    </Directory>

.. _conf-gawati-data:

Apache configuration for gawati data services
---------------------------------------------

The services provided by *Gawati Data* to access the XML documents in Gawati are not directly exposed to the outside, they are reverse proxied using Apache. The full configuration of apache config entries is provided below: 

.. include:: gawati-data-conf.rst

The above assumes:
  * eXist-db is running on port 8080 (if that is not the case in your installation change it appropriately in line 16 and 17)

.. note::
  On Windows the Apache Alias directory path need to use the back slash instead of the standard windows forward slash. For e.g. if the templates are in: `d:\\code\\gawati-templates` then the path in the Apache configuration should be: `d:/code/gawati-templates`

.. _conf-portal-server:

Apache configuration for portal server services
-----------------------------------------------

Add the following Apache entry for it:

.. include:: portal-server-conf.rst

.. _conf-client:

Apache configuration for gawati client
--------------------------------------

    .. code-block:: apacheconf
            :linenos:

            # for gawati-client-data
            <Location ~ "/gwdc/(.*)">
            AddType text/cache-manifest .appcache
            ProxyPassMatch  "http://localhost:8080/exist/restxq/gwdc/$1"
            ProxyPassReverse "http://localhost:8080/exist/restxq/gwdc/$1"
            SetEnv force-proxy-request-1.0 1
            SetEnv proxy-nokeepalive 1
            </Location>

            # for gawati-client-server
            <Location ~ "/gwc/(.*)">
            AddType text/cache-manifest .appcache
            ProxyPassMatch  "http://localhost:9002/gwc/$1"
            ProxyPassReverse "http://localhost:9002/gwc/$1"
            SetEnv force-proxy-request-1.0 1
            SetEnv proxy-nokeepalive 1
            </Location>


.. _faqs-dev-prod:
 
***
FAQ
***

.. _faq-dev-prod-eperm:

EPERM errors when running ``npm run build`` on windows
------------------------------------------------------

You attempt to build on windows and you get EPERM errors related to symlinking. 
This happens because on Windows you have to explicitly allow your user symlink permission prior to running ``npm run build``. 
You can remove the permission after completion. 

The permission is configured in Group Policies of local Computer at ``Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment`` . 
Please see here for more details `click here <https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links>`_.
