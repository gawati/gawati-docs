Gawati Architecture
###################


The Gawati architecture is described in this document. 

Gawati is composed of 2 different applications which have different audiences in mind. 

  1. The Gawati Portal - this is a public facing portal system that allows searching and accessing legal documents
  2. The Gawati Data Input System - which is a back-office application that allows managing and inputting legal documents which are presented by the Portal.

.. _gawati-portal:

********************
Gawati Portal System
********************

The Gawati Portal is composed of different application components which are indicated below in the diagram. 

.. figure:: ./_images/high-level-arch-portal.png
   :alt: Gawati Portal Architecture
   :align: center
   :figclass: align-center

The Portal System has its own dependencies which need to be installed before it can be used:
   
    * gawati-portal-ui - the UI web front-end
    * gawati-portal-fe - the back-end service for the UI
    * gawati profiles system - see below :ref:`gawati-profiles` which has its own dependencies
    * gawati-data - the Database service component 
    * gawati-portal-publisher - this is a subsystem component that allows publishing data onto the portal that has been received from the :ref:`gawati-editor`.
    * gawati-portal-qprocessor - this is a subsystem component that allows receiving data from the :ref:`gawati-editor`. 


.. _gawati-editor:

********************
Gawati Editor System
********************

The Data Input System is composed of different application components which are indicated below in the diagram. 

.. figure:: ./_images/high-level-arch-client.png
   :alt: Gawati Portal Architecture
   :align: center
   :figclass: align-center

Functionally, the data input system looks like as below: 

.. figure:: ./_images/arch_data_entry_tech.png
  :target: ./_images/arch_data_entry_tech.png
  :alt: Data Entry Architecture
  :align: center
  :figclass: align-center

The data entry client is composed of three components: 
    * Client Data Server component:  storage for documents being entered and run through the workflow for publication. Runs on `eXist-db` and is written in `XQuery <https://www.w3.org/XML/Query/>`__ (see `gawati-client-data on github <https://github.com/gawati/gawati-client-data>`__ ) 
    * Application Server component - this provides access to services like Workflow, Data conversion and other processing which needs to be done on the server. Runs on ExpressJS and NodeJS (see `gawati-client-server <https://github.com/gawati/gawati-client-server>`__ ) 
    * UI Front-end: this provides the data entry forms and validates user input. This a ReactJS app.

The Editor System has its own dependencies which need to be installed before it can be used:
   
    * gawati-editor-ui - the UI web front-end for the editor
    * gawati-editor-fe - the back-end service for the UI
    * authentication system - uses a customized keycloak domain and client template.
    * gawati-client-data - the Database service component 
    * pdf2xml-service - converts PDF documents to XML 
    * gawati-tagit - A Service that generates tags out of text documents provided to it
    * gawati-editor-qprocessor - this is a subsystem component that allows sending data from the :ref:`gawati-editor`. to the :ref:`gawati-portal`.
    * gawati-package-sign - a digital signature service used to generate digital signatures


.. _gawati-profiles:

**********************
Gawati Profiles System
**********************

See :doc:`Profiles System <./gawati-profiles-system>`

