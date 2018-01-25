What is Gawati
##############

What is Gawati and what problem is it trying to solve ? 
=======================================================

Gawati is a Legal Data Exchange platform that allows Legal documents to be Authenticated, Shared and Published. 

If you are a custodian of legal documents, and want to publish them online, Gawati may be ideally suited to fulfil your requirements. 

Gawati lets you own your legal data and authenticate it via digital signatures, and also control how you want it published, while at the same time making it searchable and accessible using open standards. 

Conventional platforms like DSpace and Fedora Commons have very rigid formats for storing documents and are spread across various relational database tables. 

Gawati makes us of `Akoma Ntoso XML <http://www.akomantoso.org>`__ an open standard for Legal and Legislative documents to capture all content and metadata. This allows easy portability of data if ever you want to take your data out of gawati and move to a different system. 

Here is how Gawati works
========================

Your typical way to access Gawati would be via the online Portal - where you would search and view legal data. 

.. figure:: ./_images/arch_portal.png
  :alt: Portal Architecture
  :align: center
  :figclass: align-center

(a more Technically oriented diagram can be accessed :download:`here <./_images/arch_portal_tech.png>` ) 

The Portal follows a conventional architecture which is popular nowadays of having different components which integrate via HTTP REST services. 
The portal is primarily three components:
    * Data Server component:  this serves all the Legal documents in Akoma Ntoso XML format. It provides to search and browse for legislation.
    * Application Server component - the application server does some processing (for e.g. summarization) of the legislative data to make it easier for the front-end to process large volumes of data.
    * UI Front-end: this shows the web-page being accessed by the user, and is the primary interface by which a user interacts with the system.

