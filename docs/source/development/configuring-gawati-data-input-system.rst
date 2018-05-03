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
For its architectural details see :ref:`gawati-client-arch`.

****************************
Basic configuration (client)
****************************

The following is configuration that is required to be done in the client component of the data input ssytem.

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


****************************
Basic configuration (server)
****************************

The following is configuration that is required to be done in the server component of the data input system.
The server component runs on a ``NodeJS`` server. It acts as an intermediary between the client component and the actual data services.

===========================
Data Services configuration
===========================

The data services run on eXist-db, the server component knows the address of the eXist-db server from the ``configs/dataServer/json`` file:

.. code-block:: json

    {
        "xmlServer" : {
            "serviceEndPoint" : "http://localhost:8080/exist/restxq",
            "api": {
                "saveXml": { 
                    "url": "/gwdc/document/add",
                    "method": "post"
                },
                "getXml" : {
                    "url": "/gwdc/document/load",
                    "method": "post"
                },
                "updateXml" : {
                    "url": "/gwdc/document/edit",
                    "method": "post"
                },
                "getDocuments": {
                    "url": "/gwdc/documents",
                    "method": "post"
                },
                "transit" : {
                    "url": "/gwdc/document/transit",
                    "method": "post"
                },
                "saveAttachments": {
                    "url": "/gwdc/document/attachments",
                    "method": "post"
                }
            }
        }
    }


    - ``xmlServer/serviceEndPoint`` : this is the full address to service end point of the eXist-db server. In this example it is running on the same host as the server component.
    - others : the rest of the configuration parameters can be left alone, they specify the individual services accessible to the server component.

===============
Workflow Config
===============

To be done

=================
Storage Templates
=================

The Storage Template is an Akoma Ntoso document template configured as a `Handlebars Template <http://handlebarsjs.com/>`__.
These templates are used to generate documents for storage. There is a hierarchy to the defined templates

    - akntemplate.hbs : main document template
        * akntemplate.componentRef.hbs - this is used to generate  the ``componentRef`` part of the main document
        * akntemplate.embeddedContent.hbs - this is used to generate the ``embeddedContent`` elements of the document

For example, ``akntemplate.hbs`` looks like follows:

.. code-block:: json

    <gwd:package xmlns:gwd="http://gawati.org/ns/1.0/data" 
        created="{{ createdDate }}"  
        modified="{{ modifiedDate }}"
        >
        <gwd:workflow>
            <gwd:state status="draft" label="Draft" />
        </gwd:workflow>
        <gwd:permissions>
            ...
        </gwd:permissions>
        <an:akomaNtoso 
            xmlns:gw="http://gawati.org/ns/1.0" 
            xmlns:an="http://docs.oasis-open.org/legaldocml/ns/akn/3.0">
            <an:{{ aknType }} name="{{ localTypeNormalized }}">
                <an:meta>
                    <an:identification source="#gawati">
                        <an:FRBRWork>
                            <an:FRBRthis value="{{ workIRIthis }}"/>
                            <an:FRBRuri value="{{ workIRI }}"/>
                            <an:FRBRdate name="Work Date" date="{{ workDate }}"/>
                            <an:FRBRauthor href="#author"/>
                            <an:FRBRcountry value="{{ workCountryCode }}" showAs="{{ workCountryShowAs }}"/>
                            {{!-- if subType is true only then render the subtype element --}}
                            {{#if subType}} 
                            <an:FRBRsubtype value="{{ localTypeNormalized }}"/>
                            {{/if}}
                            <an:FRBRnumber value="{{ docNumberNormalized }}" showAs="{{ docNumber }}"/>
                            <an:FRBRprescriptive value="{{ docPrescriptive }}"/>
                            <an:FRBRauthoritative value="{{ docAuthoritative }}"/>
                        </an:FRBRWork>
                        <an:FRBRExpression>
                            ...
                        </an:FRBRExpression>
                        <an:FRBRManifestation>
                            ...                    
                        </an:FRBRManifestation>
                    </an:identification>
                    ....
                    <an:references source="#source">
                        <an:original eId="original" href="{{ exprIRIthis }}" showAs="{{ docNumber }}"/>
                        <an:TLCOrganization eId="all" href="/ontology/Organization/AfricanLawLibrary" showAs="African Law Library"/>
                    </an:references>
                    <an:proprietary source="#all">
                        ....
                    </an:proprietary>
                </an:meta>
                <an:body>
                    ....
                </an:body>
            </an:{{ aknType}}>
        </an:akomaNtoso>
    </gwd:package>

and ``akntemplate.componentRef.hbs`` looks like follows:

.. code-block:: json

    <an:componentRef src="{{ embeddedIRIthis }}" 
        alt="{{ embeddedFileName }}" 
        GUID="#embedded-doc-{{ embeddedIndex }}" 
        showAs="{{ embeddedShowAs }}"/>

You can change these if you like, but you need to be sure what you are changing here as it may render the application unusable.


