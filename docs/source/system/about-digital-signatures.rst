Gawati Digital Signatures
#########################

Documents can be digitally signed in Gawati using the :ref:`gawati-editor`. 
We use PKI private and public key infrastructure, along with the `XMLDSIG standard for digital XML Signatures <https://www.w3.org/TR/xmldsig-core1/>`_, more specifically we use the `Java native implementation of the XML Signature API <https://docs.oracle.com/javase/8/docs/technotes/guides/security/xmldsig/XMLDigitalSignature.html>`_.

************
How it Works
************

All metadata in :ref:`gawati-editor` is captured as Akoma Ntoso XML (AKN). The AKN document wraps the actual legal document . A hash for the actual legal document is generated, and that is wrapped within the AKN document. The AKN document is then signed using the private key and published. The digital signature aspect is split across different services:

    **Sign**

    1. :ref:`gawati-editor` UI: where the signature is initiated from. Typically this is the user clicking a button on the screen which says "sign document" 
    2. digital signature service: this is a java based REST service that accepts a document and private key, and returns the digitally signed document. 
    3. digital signature interaction service: This is a servie that allows the UI to interact with the digital signature service. This needs to be installed only on the host computer of the user who is allowed to digital sign documents. Even though the :ref:`gawati-editor` is a browser based system, the digital signature interaction service is one component that needs to be installed on the client computer. The reason we have this as a distinct service with a recommendation that it be run only on the host computer of the user signing the document is for security and safety reasons. This ensures the private key is stored only on the host signing the document, and the only service interaction where the private key is sent from the host is via an encrypted secure connection with the package signature service. 

    **Validation**
    
    1. :ref:`gawati-editor` UI: User submits IRI of the document to be validated. Typically this is the user clicking a button on the screen which says "validate document" 
    2. digital signature service: this is a java based REST service that accepts a signed document and public key, and returns the validity of the signed document. 
    3. digital signature interaction service: this verifies the hashes of the actual legal document and forwards the AKN document and public key to the signature service to check the validity of the AKN document.