Gawati Client 
#############

.. figure:: ./_images/arch_data_entry_tech.png
  :target: ./_images/arch_data_entry_tech.png
  :alt: Data Entry Architecture
  :align: center
  :figclass: align-center

The data entry client is composed of three components: 
    * Client Data Server component:  storage for documents being entered and run through the workflow for publication. Runs on `eXist-db` and is written in `XQuery <https://www.w3.org/XML/Query/>`__ (see `gawati-client-data on github <https://github.com/gawati/gawati-client-data>`__ ) 
    * Application Server component - this provides access to services like Workflow, Data conversion and other processing which needs to be done on the server. Runs on ExpressJS and NodeJS (see `gawati-client-server <https://github.com/gawati/gawati-client-server>__ ) 
    * UI Front-end: this provides the data entry forms and validates user input. This a ReactJS app.

