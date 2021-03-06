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

.. figure:: ./_images/portal_arch.png
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

While the :ref:`gawati-portal` system provided access to documents, the documents are published onto the Portal via the Gawati Editor System described here. 

The Editor System is composed of different application components which are indicated below in the diagram. 

.. figure:: ./_images/client_arch.png
   :alt: Gawati Portal Architecture
   :align: center
   :figclass: align-center

Functionally, the data input system looks like as below: 

.. figure:: ./_images/arch_data_entry_tech.png
  :target: ./_images/arch_data_entry_tech.png
  :alt: Data Entry Architecture
  :align: center
  :figclass: align-center

The Gawati Editor system has been concieved as an offline / online data entry system which can be used to publish information onto the :ref:`gawati-portal` even if offline. This is to cater for scenarios where data input is being done from places where internet connectivity is not always stable. 

The Gawati Editor system uses a `message-queue <https://en.wikipedia.org/wiki/Message_queue>`_ based architecture to support such a functionality. 

====================
Architecture in Depth
====================

The Editor System follows the same basic architecture as that of the Portal system, and provides a browser based experience for adding and managing documents: 

    *  gawati-editor-ui - which is the front-end client which provides the UI and UX for the editor system. It encompasses a dashboard, a workflow and a form based system to manage content (see `gawati-editor-ui <https://github.com/gawati/gawati-editor-ui>`_ ). 
    *  gawati-editor-fe - which provides front-end services that the ``gawati-editor-ui`` can interact with.
    *  gawati-client-data - which provides back-end services that are primarily used for getting data in and out of the XML database. The front-end ``gawati-editor-ui`` client does not access this directly but accesses this service via ``gawati-editor-fe`` which acts an authentication proxy for these services. 

In addition to these there are some supporting services which are required by the editor system, that provide functional support for various other services: 

    - Full text extraction - a lot of content in Gawati is PDF based. We extract the full text of the PDF document as XML and make that searchable by integrating it with the main metadata search. This full-text extraction is provided by a separated full-text extraction service. (See `pd2xml-service <https://github.com/gawati/pdf2xml-service>`_)
    - Automatic Tagging - a document whose full text has been extract can be automatically tagged by a specialized service which uses a trained legal model to identify and generate tags based on the document's content. (see `gawati-tagit <https://github.com/gawati/gawati-tagit>`_).
    - Digital Signatures - Document packages in Gawati can be digitally signed and published. This digital signature functionality is provided on the back of 2 services:

        1. package signature service: this accepts a gawati document as a gawati package and a private key to sign the document, and returns the signed document. (See `gawati-package-sign <https://github.com/gawati/gawati-package-sign>`_). This service also allows validating a document based on its public key. 
        2. digital signature interaction service: this is intended (and recommended) to be run on the ``host computer`` signing the document.(See `gawati-digisign-fe <https://github.com/gawati/gawati-digisign-fe>`_).  For more information how digital signatures work in Gawati see :doc:`About Digital Signatures <./about-digital-signatures>`. 
    
    - Asynchronous Publication - The gawati editor system uses an asynchronous publication mechanism to move documents from the Editor System on to the :ref:`gawati-portal`. For more information on this see :doc:`About Publication <./about-publication>`. On the editor system end the expected services: 
        1. RabbitMQ - this is an enterprise grade Message Queue system which we just use out of the box for its message queue capabilities. 
        2. Editor QProcessor - this is a specialized service that runs as a background process in the Editor system and periodically scanes the message queue for publication events. It transmits documents which are to be published onto the portal system. (See `Editor QProcessor <https://github.com/gawati/gawati-editor-qprocessor>`_).
    

.. _gawati-profiles:

**********************
Gawati Profiles System
**********************

The Gawati Profiles System allows authenticated users in the Gawati system to have user profiles. 

The profiles system is technically composed of two main components:

        * Front-end :  `gawati-profiles-ui <https://github.com/gawati/gawati-profiles-ui>`_
        * Back-end service: `gawati-profiles-fe <https://github.com/gawati/gawati-profiles-fe>`_ which in turn requires a MongoDb database back-end (see :ref:`inst-prerequisites`). 


.. figure:: ./_images/profiles_arch.png
  :target: ./_images/profiles_arch.png
  :alt: Data Entry Architecture
  :align: center
  :figclass: align-center

The profiles system does not provide authentication or a user directory on its own, for that it simply integrates with authentication and user directory services provided by KeyCloak (see :ref:`inst-prerequisites`).

The profiles system integrates with the portal system (see :ref:`gawati-portal`) using KeyCloak single-sign-on (SSO) if the user logs into the profiles system, they are transparently logged into the portal. The Profiles system allows storing extended user information e.g. in the portal users can save their favourite searches. In the user interface it may appear as if it is the portal that is providing this information in reality this served by the profiles service. 


