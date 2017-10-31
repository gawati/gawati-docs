Developing with Gawati on a remote server, the lazy way
#######################################################

This will have your development consist of 2 components:

  - Gawati on a CentOS server or virtual machine
  - Your desktop machine for development

You will use SSH to securely connect your desktop to the server.


Chocolatey installation system
******************************

For MS Windows
""""""""""""""

Install Chocolatey
''''''''''''''''''

Open administrative cmd.exe and execute ::

  @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

Close and reopen administrative cmd.exe
The Chocolatey installation commands below, are to be executed in this administrative cmd.exe


Install Gawati server
*********************

Prepare a CentOS minimal server
"""""""""""""""""""""""""""""""

You can do this as a VM on your local machine, or use a remote installation


Gawati Server on local VM
'''''''''''''''''''''''''

We recommend Virtualbox. Install using the command ::

  choco install 7zip virtualbox -y

and run the :doc:`Gawati installer<./setup-essentials>`.



Install your desktop development tools
**************************************

For MS Windows
""""""""""""""

Install development applications
''''''''''''''''''''''''''''''''

choco install openssh git jdk8 ant visualstudiocode -y


Configure Visual Studio Code
''''''''''''''''''''''''''''

Go to File -> Preferences -> Settings (Ctrl+,). Paste into rightmost tab titled 'Place your settings here...' ::

  {
    "telemetry.enableTelemetry": false,
    "telemetry.enableCrashReporter": false,
    "files.autoGuessEncoding": true
  }

Go to View -> Extensions (Ctrl+Shift+X)

Install the following plugins from there:

 - XML Tools
 - XML Formatter

For writing documentation install:

 - reStructuredText


Connect to Gawati server
''''''''''''''''''''''''

In a new cmd shell, replace 'my.gawati.org' with your server name in the following command and run ::

  ssh -L 10447:localhost:10447 root@my.gawati.org

This will tunnel localhost:10447 to your server:10447 and encrypt the communication on its path. You can lower this shell, leaving it running in the background.


Point your webbrowser to http://localhost:10083 , log in as admin user (credentials received in server installation) and open 'eXide - XQuery IDE'

Paste following query into main tab 'new-document' ::

  xquery version "3.1";
  data(doc('/db/apps/gawati-portal/_auth/_pw.xml')/users/user[@name='gawatiportal']/@pw)

and execute by clicking 'Eval' button in top row.
Copy the content in the 'Adaptive Output' Tab at the bottom. This is the password of user 'gwdata' we need below.


In a new cmd shell, replace 'yourpastedpasswordhere' with the password retrieved above and run ::

  net use x: "https://localhost:10447/exist/webdav/db/apps/gawati-portal" /user:gawatiportal yourpastedpasswordhere

You can close this cmd window.

Open the new drive in Visual Studio Code in File -> Open Folder (CTRL+K -> CTRL+O)
