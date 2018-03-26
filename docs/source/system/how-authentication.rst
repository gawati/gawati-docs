####################################
How Trust & Security Works in Gawati
####################################

.. contents:: Table of Contents 
  :local:

************
Introduction
************

This document focuses on how trust and security works in Gawati, what are the components we use, how they interact with each other, and what are the specific configuration choices we have made in Gawati. We cover aspects governing both authentication and authorization in Gawati. 

In Gawati we use a third-party open source server called `Key Cloak <https://www.keycloak.org>`_.  KeyCloak uses JWT (JSON Web Tokens) to implement security. For a quick introduction to JWT see `Understanding JSON Web Tokens <https://medium.com/vandium-software/5-easy-steps-to-understanding-json-web-tokens-jwt-1164c0adfcec>`_ .


***************************
Realms, Users, Applications
***************************

Users in KeyCloak are defined within authentication realms.  Applications wanting to authenticate with a Realm, need to be registered and configured on the Realm, a registered Application in KeyCloak is called a ``Client``. Applications can define specific roles which can be assigned to users on the Realm. 






