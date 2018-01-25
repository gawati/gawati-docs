What is Gawati
##############

What exactly is Gawati and what problem is it trying to solve ? 

Gawati is a Legal Data Exchange platform that allows Legal documents to be Authenticated, Shared and Published. 

If you are a custodian of legal documents, and want to publish them online, Gawati may be ideally suited to fulfil your requirements. 

Gawati lets you own your legal data and authenticate it via digital signatures, and also control how you want it published, while at the same time making it searchable and accessible using open standards. 

Here is how Gawati works. 

Your typical way to access Gawati would be via the online Portal - which supports searching and viewing legal data. 

.. figure:: ./_images/arch_portal.png
  :alt: Portal Architecture
  :align: center
  :figclass: align-center

(a more Technically oriented diagram can be accessed `here <./_images/arch_portal_tech.png>`__ ) 

The Portal follows a conventional architecture which is popular nowadays of having different components which integrate via HTTP REST services. 
The portal is primarily three components:
    * Data Server component:  this serves all the Legal documents in Akoma Ntoso XML format. It provides to search and browse for legislation.
    * Application Server component - the application server does some processing (for e.g. summarization) of the legislative data to make it easier for the front-end to process large volumes of data.
    * UI Front-end: this shows the web-page being accessed by the user, and is the primary interface by which a user interacts with the system.

