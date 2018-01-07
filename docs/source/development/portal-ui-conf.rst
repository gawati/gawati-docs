.. code-block:: apacheconf
  :linenos:

    Alias /ui "/home/web/apps/gawati-portal-ui"
    <Directory "/home/web/apps/gawati-portal-ui">	
      DirectoryIndex "index.html"
      Require all granted
      AllowOverride All
      Order allow,deny
      Allow from all
    </Directory>

    ##
    ## added for portal-ui v 2.0.10
    ##

    Alias /locales "/home/web/apps/gawati-portal-ui/locales"
    <Directory "/home/web/apps/gawati-portal-ui/locales">	
      Require all granted
      AllowOverride All
      Order allow,deny
      Allow from all
    </Directory>
