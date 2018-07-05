Gawati Profiles System
######################

The Gawati Profiles System allows authenticated users in the Gawati system to have user profiles. 
The profiles system is technically composed of two main components:

        * Front-end :  `gawati-profiles-ui <https://github.com/gawati/gawati-profiles-ui>`_
        * Back-end service: `gawati-profiles-fe <https://github.com/gawati/gawati-profiles-fe>`_ which in turn requires a MongoDb database back-end (see :ref:`inst-prerequisites`). 

The profiles system does not provide authentication or a user directory on its own, for that it simply integrates with authentication and user directory services provided by KeyCloak (see :ref:`inst-prerequisites`).

The profiles system integrates with the portal system (see :ref:`gawati-portal`) using KeyCloak single-sign-on (SSO) if the user logs into the profiles system, they are transparently logged into the portal. The Profiles system allows storing extended user information e.g. in the portal users can save their favourite searches. In the user interface it may appear as if it is the portal that is providing this information in reality this served by the profiles service. 

        