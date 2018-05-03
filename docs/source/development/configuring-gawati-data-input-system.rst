Configuring Gawati Data Input System
####################################

This document is about how to configure the gawati client for use within an organization. The order of configuration is important because there are dependencies between different settings. 
For the initial setup, we recommend strictly following the order specified here.

.. contents:: Table of Contents 
  :local:


*********************
About the Application
*********************

The application allows adding documents to the system and the format of storage is in Akoma Ntoso XML format.
The system provides a workflow which allows users with different roles to systematically input data into the system, following a structured process. 

*******************
Basic configuration
*******************

=====================
Akoma Ntoso Doc Types
=====================

The basic building block for a document is its typology. Akoma Ntoso supports various typologies. All other typologies derive from these basic typologies. 
The client specifies root Akoma Ntoso typologies in ``configs/aknDocTypes.json``: 

.. code-block:: json

    {"aknDocTypes": [
        "amendmentList",
        "officialGazette", 
        "documentCollection", 
        "act", 
        "bill", 
        "debateReport", 
        "debate",
        "statement",
        "amendment",
        "judgment",
        "portion",
        "doc" 
    ]}

.. note::
    Do not change these basic types or add new ones, they could render your documents invalid.

=====================
Application Doc Types
=====================

The data entry application needs to be configured for specific types within the allowed Akoma Ntoso Types. 

Akoma Ntoso has a concept of sub-Types which we make use of here. The need for such a construct is obvious -- different legal jurisdictions use different nomenclatures. 

For instance, an "Act" in one country and can be called "Atto" in another. Similarly, a "Resolution" is a distinct type, but it is modelled on the lines of an Akoma Ntoso Act. The sub-Type mechanism allows using the "Act" model of Akoma Ntoso to model documents with custom nomenclatures which follow the same model as the Act. 
So for the example here, "Atto" and "Resolution" would be modelled as sub-Types of an Act. 

Each type allowed for data entry and management needs to be configured in ``configs/docTypes.json``. Only the types specified here appear in the document Type dropdown in the application:

.. code-block:: json

    {"docTypes": [
        {
            "aknType": "act",
            "aknDocTag": "act",
            "localTypeName": "Law",
            "localTypeNameNormalized": "law",
            "category": "legislation"
        },
        {
            "aknType": "judgment",
            "aknDocTag": "judgment",
            "localTypeName": "Judgement",
            "localTypeNameNormalized": "courtjudgment",
            "category": "case law"
        }
    ]
    }

Each item in the above ``JSON`` structure represents a configured document type, for instance the below represents a configured document Type:

.. code-block:: json

        {
            "aknType": "act",
            "aknDocTag": "act",
            "localTypeName": "Law",
            "localTypeNameNormalized": "law",
            "category": "legislation"
        },

Each of the config items is explained below:

    - ``aknType``: the name of an allowd Akoma Ntoso document type
    - ``aknDocTag`` : this is typically the same value as the ``aknType``
    - ``localTypeName``:  what the document is called in a local jurisdictions
    - ``localTypeNormalized``: the ``localTypeName`` in `lower camel case <http://wiki.c2.com/?LowerCamelCase>`__. This is done because the normalized name appers in URLs.
    - ``category``: This is custom category you can specify

==============
Document Parts
==============

A Legal document is typically compose of main document and one or more annexes. In Gawati each of these is recorded individually.
Document Parts shown in the UI for selection are listed in ``configs/docParts.json`` :

.. code-block:: json

    {"docParts": [
        {
            "partName": "main",
            "partLabel": "Main"
        },
        {
            "partName": "annex",
            "partLabel": "Annex"
        }
    ]
    }


    - ``partName`` : the part name is always in `lower camel case <http://wiki.c2.com/?LowerCamelCase>`__
    - ``partLabel`` : this is the label that appears in selector dropdown for the partName. 




