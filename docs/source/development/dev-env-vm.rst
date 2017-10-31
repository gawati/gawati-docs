Gawati on a server / VM
#######################

This will have your development consist of 2 components:

  - Gawati on a CentOS server or virtual machine
  - Your desktop machine for development

You will use SSH to securely connect your desktop to the server.


Chocolatey installation system
******************************

For MS Windows
""""""""""""""

Install `Chocolatey`_
'''''''''''''''''''''

Open administrative cmd.exe and execute ::

  @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

Close and reopen administrative cmd.exe
The `Chocolatey`_ installation commands below, are to be executed in this administrative cmd.exe


Install Gawati server
*********************

Prepare a CentOS minimal server
"""""""""""""""""""""""""""""""

You can do this as a VM on your local machine, or use a remote installation


Gawati Server on local VM
'''''''''''''''''''''''''

We recommend `Virtualbox`_. Install using the command ::

  choco install openssh 7zip virtualbox -y

We provide a Centos 7 Minimal Install `Virutalbox Image`_ using LVM and seprate
filesystems mounted where Gawati takes up space:

  https://drive.google.com/open?id=0B6u3y5jrQTubSnRtWEE3cFdyLWc

The VM is 7zip packed. Unpack it into folder "%USERPROFILE%\VirtualBox VMs".
Add the VM to VirtualBox Manage using Machine -> Add , browse into the "Gawati"
Folder and select the Gawati (.vbox) file.

To run it for development we recommend to not start this instance, instead create
a linked clone and run that. To do so, highlight the Gawati VM, right click and
"Clone", select "Expert Mode" and activate "Linked Clone". You can then run this
clone, and when you are done with it or broke it, delete it and create a new
clone to restart with a clean slate.

The VM is configured with dynamic IP (if its your first VM, tends to be 192.168.56.101).
Log in to the VM console:

Log in credentials::

  User root
  Password MyGawatiLocal

Check IP addr::

  ip addr

Add an entry to your hosts file at %WINDIR%\\system32\\drivers\\etc using an
administrative instance of notepad and add an entry equivalent to this, using the
IP of your VM::

  192.168.56.101  my.gawati.local

You can connect to it using ssh::

  ssh -L 10443:localhost:10443 root@my.gawati.org

From :doc:`Gawati installer<./setup-essentials>` documentation, just download the installer as described and run it twice.


Install your desktop development tools
**************************************

For MS Windows
""""""""""""""

Install development applications
''''''''''''''''''''''''''''''''

choco install git jdk8 ant visualstudiocode -y


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

In a new cmd shell, and connect to your VM using ::

  ssh -L 10443:localhost:10443 root@my.gawati.local

This will tunnel localhost:10443 to your server:10443 and encrypt the communication on its path. You can lower this shell, leaving it running in the background.


Point your webbrowser to http://localhost:10443 , log in as admin user (credentials received in server installation) and open 'eXide - XQuery IDE'

Paste following query into main tab 'new-document' ::

  xquery version "3.1";
  data(doc('/db/apps/gawati-portal/_auth/_pw.xml')/users/user[@name='gawatiportal']/@pw)

and execute by clicking 'Eval' button in top row.
Copy the content in the 'Adaptive Output' Tab at the bottom. This is the password of user 'gwdata' we need below.


In a new cmd shell, replace 'yourpastedpasswordhere' with the password retrieved above and run ::

  net use x: "https://localhost:10443/exist/webdav/db/apps/gawati-portal" /user:gawatiportal yourpastedpasswordhere

You can close this cmd window.

Open the new drive in Visual Studio Code in File -> Open Folder (CTRL+K -> CTRL+O)


.. _Chocolatey: https://chocolatey.org/
.. _Virtualbox: https://www.virtualbox.org/
.. _Virutalbox Image: https://drive.google.com/open?id=0B6u3y5jrQTubSnRtWEE3cFdyLWc
