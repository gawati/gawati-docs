Gawati Document Publication
###########################

Document Publication in Gawati is a process handled by the :ref:`gawati-editor`. We use an asyncrhonous model in Gawati for document publication. This model is used to allow the Data management aspect and the Publication and search aspect to be used independently of each other even when either of those ends is down. It also has a bearing on how the :ref:`gawati-editor` and the :ref:`gawati-portal` are deployed. All these aspects have been discussed below. 

****************************
Publication in the Editor UI 
****************************

The End user using the system encounters the publication process typically via the :ref:`gawati-editor`. The Publication process is integrated into the application workflow, and appears as a workflow transition (typically called ``publish``). When the user clicks the ``publish`` transition, the document is sent for publication. Note that - it does not imply that the document is immedately published, but only records a desire by the user to publish a particular document. The document status is changed to ``under processing`` and the user can proceed to do other work, note that even if the :ref:`gawati-portal` system is down, the :ref:`gawati-editor` system will work in exactly the same way described here - simply record an event that  a document needs to be published and return control to the user. 

*************************
Publication Message Queue
*************************

When the user clicks a ``publish`` transition. The address of the document (lets call it document 'X', but in reality it is the document's IRI) that is required to be published is placed on a queue reserved for documents that are to be published. A separate server-side process (lets call it editor-queue-processor) continually checks this queue for publication messages, and when it finds one it prepares the document as a compressed zip package based on the address provided and transmits it to the :ref:`gawati-portal`. 

One the :ref:`gawati-portal` end there is a service that receives the document (lets call it the portal-queue-processor) verifies its validity, and then places it on a publication queue at the :ref:`gawati-portal` end. A separate publisher process periodically checks this publication queue, and posts the received document onto the :ref:`gawati-portal` database, at which point the document 'X' is available online on the portal. The publisher process adds a message to a status event queue indicating that document 'X' has been successfully published. 

The portal-queue-processor periodically listens for events on the status queue, and once it received a successfull publication message it sends a message to the editor-queue-processor (over http) that document X has been successfully published. The editor-queue-processor on receipt of the message updates the status of document X on its internal queue which is then used to initiate a workflow transition on the document in the editor system. At the end of a successful publication cycle, document 'X' will go from 'under processing' to 'published'. 

The above process has been described in the below diagram:

.. figure:: ./_images/arch_data_entry_tech.png
  :target: ./_images/arch_data_entry_tech.png
  :alt: Data Entry Architecture
  :align: center
  :figclass: align-center


