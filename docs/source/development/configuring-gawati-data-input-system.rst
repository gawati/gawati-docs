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


 .. _input-system-access-control:
 
**************
Access Control
**************

The Data Input system is an authenticated system and access to it requires a user name and password. In Gawati users are created in the configured KeyCloak Realm.
However, just creating a user is not enough, you need to associate users to roles. In Gawati this is done via Groups, we have groups associated with one or more Roles, and users added to a group acquire the Roles on the group.

If you have used the `Model Realm <./model-realm>`__ to create your realm, then the following roles & groups will be available to you:

    - Roles
        * `client.Admin` - has access to all the states and transitions the document can go through.
        * `client.Editor` - can edit the document 
        * `client.Submitter` - can draft a document, but cannot edit it outside of the draft state
        * `client.Publisher` - can publish a document at the end of the workflow process
    - Groups
        * `clientAdmins`
        * `clientEditors`
        * `clientSubmitters`
        * `clientPublishers`

.. note::
    - These roles and groups are not baked into the system, you can change them. Keep in mind though, changing groups and roles will also require you to change Workflow configuration (see :ref:`workflow-config`).
    - Remember that roles are just labels, just because we called a Role as ``something.Admin`` doesn't automatically mean anything, the Roles are granted Permissions and that is what really determines access.


In Gawati Input System the following permissions are recognized (we don't use the KeyCloak resource Permissions infrastructure). 
Permissions are always assigned to ``Roles``, and the assignment is done view :ref:`workflow-config`.

    * ``view`` - allows viewing the document
    * ``list`` - allows showing the document in a listing (this is a subset of the view permission, the user with only ``list`` and not ``view`` can only see the document in a listing, but cannot view it)
    * ``edit`` - allows editing the document
    * ``delete`` - allows deleting the document
    * ``transit`` - allows transiting the document from one state to another. Transiting a document typically means the permissions and roles applicable on the document will change. 


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


.. _application-doc-types:

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


 .. _workflow-config:
 
===============
Workflow Config
===============

The client component presents a dashboard where documents can be created and moved through different stages.
We have already about :ref:`input-system-access-control`, and the concepts of Permissions and Roles are essential to understanding how workflows work.

The Workflow describes the permissons and roles of a document as it moves from one person to the other. This is analogous to paper based authorization chains, where a document moves thorugh different individuals in an office. 
Each person reviews the document, and sends it to a higher up, until the document is signed and released for official consumption. 

The Workflow is configured in one or more JSON files placed in the ``workflow_configs`` folder in the server component. 
Each document Type configured in the application has a corresponding workflow configuration file. 

The JSON file for a workflow is structured as below:

.. code-block:: json

    {"workflow": {
        "doctype": "act",
        "subtype": "law",
        "permissions": {"permission": [
            {
                "name": "view",
                "title": "View",
                "icon": "fa-eye"
            },
            {
                "name": "edit",
                "title": "Edit",
                "icon": "fa-pencil"
            },
            {
                "name": "delete",
                "title": "Delete",
                "icon": "fa-trash-o"
            },
            {
                "name": "list",
                "title": "List",
                "icon": "fa-flag"
            },
            {
                "name": "transit",
                "title": "Transit",
                "icon": "fa-flag"
            }
        ]},
        "states": {"state": [
                ....
            ]}
        ...
    }


--------------
Workflow State
--------------

In the Workflow system, this concept of the document moving from one person to the other is represented via ``states``. Each state represents a set of roles and permissions applicable to the document in that state.
The below JSON block describes the ``Draft`` state of a document. 

.. code-block:: json

        {
            "name": "draft",
            "title": "Draft",
            "level": "1",
            "color": "initial",
            "permission": [
                ....
            ]
        }

The relevant ``state`` config variables are described below:

    - ``name`` : the technical name of the workflow state (no spaces). 
    - ``title`` : the literal string used to display the state name on the screen 
    - ``permission`` : lists the permissions applicable in that state. 


.. code-block:: json

        "permission": [
            {
                "name": "view",
                "roles": "client.Admin client.Submitter"
            },
            {
                "name": "list",
                "roles": "client.Admin client.Submitter"
            },
            {
                "name": "edit",
                "roles": "client.Admin client.Submitter"
            },
            {
                "name": "delete",
                "roles": "client.Admin client.Submitter"
            },
            {
                "name": "transit",
                "roles": "client.Admin client.Submitter"
            }
        ]

Each permission can be associated with multiple states:

    - ``name`` : the name of the permission as described in :ref:`input-system-access-control`
    - ``roles`` : one or more roles separated by a space



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


