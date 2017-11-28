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

Install Chocolatey
''''''''''''''''''

Install `Chocolatey`_ from their website or open administrative cmd.exe and execute ::

  @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

Close and reopen administrative cmd.exe
The `Chocolatey`_ installation commands below, are to be executed in this administrative cmd.exe


Install Gawati server
*********************

Prepare a CentOS minimal server
"""""""""""""""""""""""""""""""

You can do this as a VM on your local machine, or use a remote installation.
Below we will run on a preconfigured VM image that you can download.


Gawati Server on local VM
'''''''''''''''''''''''''

We recommend `Virtualbox`_. Install using the command ::

  choco install kitty 7zip virtualbox -y

We provide a Centos 7 Minimal Install `Virutalbox Image`_ using LVM and seprate
filesystems mounted where Gawati takes up space:

  http://gawati.org/GawatiVM.7z

The VM is 7zip packed. Unpack it into folder "%USERPROFILE%\\VirtualBox VMs".
Add the VM to VirtualBox Manage using Machine -> Add , browse into the "Gawati"
Folder and select the Gawati (.vbox) file.

To run it for development we recommend to not start this instance, instead create
a linked clone and run that. To do so, highlight the Gawati VM, right click and
"Clone", select "Expert Mode", activate "Linked Clone" and name it "Gawati Clean".
This clone will keep a fresh installation of Gawati. Start the "Gawati Clean" VM.

The VM is configured with dynamic IP (if its your first VM, tends to be 192.168.56.101).
Log in to the VM console:

Log in credentials::

  User root
  Password MyGawatiLocal

Check IP addr::

  ip addr show dev eth0

Add an entry to your hosts file at %WINDIR%\\system32\\drivers\\etc using an
administrative instance of notepad and add an entry equivalent to this, using the
IP of your VM::

  192.168.56.101  my.gawati.local

You can connect to it using ssh::

  kitty -pw MyGawatiLocal -ssh root@my.gawati.local

Allow all traffic from your PC to your VM (dont do this for internet facing servers) ::

  firewall-cmd --zone=trusted --change-interface=eth0 --permanent

From :doc:`Gawati installer<./setup-essentials>` documentation, just download the
installer as described, run it twice and reboot the system after installation
to activate kernel configurations and have services bind to IPs correctly ::

  curl "https://gawati.org/setup" -o setup
  chmod 755 setup
  ./setup
  ./setup
  echo Take note of the admin credentials, then press >ENTER<
  read
  poweroff

Create another linked clone as above, but name it "Gawati Dev".
You can then run this clone headless, and when you are done with it or broke it,
delete it (with deleting files) and create a new clone to restart on a clean slate.


Install your desktop development tools
**************************************

Install development applications
""""""""""""""""""""""""""""""""

in administrative cmd.exe run ::

  choco install git jdk8 ant visualstudiocode -y


Configure Visual Studio Code
""""""""""""""""""""""""""""

Go to File -> Preferences -> Settings (Ctrl+,). Paste into rightmost tab titled
'Place your settings here...' ::

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


Map a drive to Gawati server
""""""""""""""""""""""""""""

Exist DB server allows WebDav access from localhost only, so we will use SSH
forwarding to make our connection appear local.

Open a new cmd shell and connect to your VM using ::

  kitty -pw MyGawatiLocal -ssh root@my.gawati.local -L 10443:localhost:10443

This will tunnel localhost:10443 to your server:10443 and encrypt the communication
on its path. You can lower this shell, leaving it running in the background. This
forwarding allows you to access the exist instance as a local service. For example
you can now browse https://localhost:10443 where you can log in as admin user (credentials
received in server installation) to the (remote) server.

In a new cmd shell, replace 'youradminpassword' with the password retrieved
above and run ::

  net use x: "https://localhost:10443/exist/webdav/db/apps" /user:admin youradminpassword

You can close this cmd window.

Open the new X: drive in Visual Studio Code in File -> Open Folder (CTRL+K -> CTRL+O)


.. _Chocolatey: https://chocolatey.org/
.. _Virtualbox: https://www.virtualbox.org/
.. _Virutalbox Image: https://drive.google.com/open?id=0B6u3y5jrQTubSnRtWEE3cFdyLWc
