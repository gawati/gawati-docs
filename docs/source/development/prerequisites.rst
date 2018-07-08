
  **Pre-requisites (runtime):**
  
    #. **JDK 1.8 LTS** - On Linux operating systems you can install `OpenJDK8 <http://openjdk.java.net/install/>`_; For `Windows <https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html#CHDEBCCJ>`_ and for `Mac OSX <https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#CHDBADCG>`_ and `Using OS X Homebrew <https://stackoverflow.com/questions/24342886/how-to-install-java-8-on-mac/28635465#28635465>`_
    #. **NodeJS LTS 8.11.x** - See `NodeJS LTS 8.11.x <https://nodejs.org/en/download/>`_. Alternatively you can install NVM (for `Linux <https://github.com/creationix/nvm/>`_ , `Windows <https://github.com/coreybutler/nvm-windows>`_ ) which lets you easily install parallel versions of NodeJS. 
    #. **eXist-db 4.3.0**: Download and install eXist-db 4.3.0, see `eXist-db <https://bintray.com/existdb/releases/exist/4.3.0/view>`_ Remember to note down the admin password of the eXist-db installation, you will need that later.   If you are installing eXist-db on Mac OS X, install it within the User folder, installing it in ``/Applications`` causes problems sometimes as the permissions required for eXist-db to write to the file system are for a super user.  
    #. **Apache HTTPD 2.4.x**: Install Apache, on Cent OS, Ubuntu and OS X this will likely be installed by default, on Windows you will have to download and install, see `Apache for Windows <https://www.apachehaus.com/cgi-bin/download.plx>`_; enable `mod_alias`, `mod_rewrite`, `mod_proxy`, `mod_proxy_http` and enable htaccess.
    #. **KeyCloak 3.4.1**: Authentication server, provides single-sign-on, see :doc:`./authentication` 
    #. **RabbitMQ 3.7.6**: Message Queue server, used by the content sync engine that moves data between the  :ref:`gawati-editor` and the  :ref:`gawati-portal` see `installing rabbitmq server <https://www.rabbitmq.com/download.html>`_, `version 3.7.6 <https://bintray.com/rabbitmq/all/rabbitmq-server/3.7.6>`_ you will need to install Erlang 20.3 before that, see `installing erlang <http://www.erlang.org/downloads/20.3>`_.
    #. **MongoDB 3.6.5 Community Edition**: Used by the Gawati User Profiles system, see `Download MongoDB <https://www.mongodb.com/download-center?jmp=nav#community>`_ , `Install MongoDb <https://docs.mongodb.com/manual/installation/>`_
    #. **Jetty 9.4.6.v20170531**: Used by :ref:`gawati-portal` (NOTE: required, only if you are saving legal documents in XML format), `Download <https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.4.6.v20170531/jetty-distribution-9.4.6.v20170531.zip>`_ , `Installing <https://www.eclipse.org/jetty/documentation/9.4.x/index.html>`_.
  
  **Pre-requisites (development):**
    
    #. *Ant*: Download and `install Ant <http://ant.apache.org/manual/install.html#installing>`_ 
    #. *Visual Studio Code*: This is if you want play around with gawati code. Download, and setup Visual Studio Code (there are versions for Windows, OS X and Linux) for development, see `VS Code Setup <./using-vscode.rst>`_
