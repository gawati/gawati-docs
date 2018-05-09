Writing Docs Locally
--------------------

Requisites:
  * Python

Installation
  * Install pip and virtualenv (see https://pip.pypa.io/en/stable/installing/, https://virtualenv.pypa.io/en/stable/installation/ )
  * Activate the virtualenv
  * Install sphinx within the virtualenv:
	```
	pip install sphinx sphinx_rtd_theme
	```
  * Edit the documentation
  * To test run: `make html`. This will produce the html in the build folder.

