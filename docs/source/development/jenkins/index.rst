Jenkins Setup
#############

Installation
************

Run::

  yum groupinstall 'Development Tools'
  yum install jenkins


Repository configuration
************************

- Gawati repositories are added as a single resource of type "Github Organization".
- Only branches "master" and "dev" are displayed by configuring Project Behaviour
"Filter by name (with wildcards) is configured as "master dev".
- "Automatic branch project triggering" is configured as "master|dev"

Individual repositories are included by adding a "Jenkinsfile" in the sourcecode root
folder.

Jenkinsfile and Jenkins library
*******************************

Packaging, upload to package repository and deployment are implemented using shell
script. As individual shell calls in Jenkinsfile reinstatiate with a new environment
on each call and the need for extensive escaping from within Jenkinsfile for shell
script, we collect Jenkins operations in a single library limiting commands from
within Jenkins to a few lines.

The Jenkins library resides in the "library" folder within the github
`Jenkins repository`_. 


Building packages and copy to downloadserver
********************************************

For more detailed information about Jenkins library see :doc:`Jenkins library <jenkinslib>`


Define package file name body as variable PKF in Jenkinsfile::

    environment { 
        PKF="portal-ui"
    } 


Jenkinslib uses git branch name from Jenkins environment to designate target
environment (dev or prod).

    
using npm
"""""""""
See example at https://github.com/gawati/gawati-portal-ui/blob/dev/Jenkinsfile

Parameters are loaded from npm environment defined in "package.json" (Package version).


using ant
"""""""""
See example at https://github.com/gawati/gawati-data/blob/dev/Jenkinsfile

Parameters are loaded from ant environment defined in "build.xml" (Package version).


.. _Jenkins repository: https://github.com/gawati/jenkins
