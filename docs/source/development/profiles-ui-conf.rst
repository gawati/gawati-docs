.. code-block:: apacheconf
  :linenos:

    Alias /profiles "/home/web/apps/gawati-profiles-ui"
    <Directory "/home/web/apps/gawati-profiles-ui">	
      DirectoryIndex "index.html"
      Require all granted
      AllowOverride All
      Order allow,deny
      Allow from all
    </Directory>
