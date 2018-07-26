Customizing the Editor
######################

Custom Metadata
***************

Gawati Editor allows adding fields from a set of metadata specific to the document type. This is apart from the standard metadata common to all document types. 

How it works
************

- User creates a new document of a particular document type. The custom metadata tab is not available at this point.
- User fills in standard (Identity) metadata and saves the document. 
- The document gets reloaded with the document type specific custom metadata made available as a selector.
- The user may choose fields that are needed for the document, populate the values and save the document.
- The custom metadata gets saved within the <gw:gawatiMeta> tag within the <gw:proprietary> section of the XML document.
- The user may add or delete custom metadata fields to be associated with the document.

Notes
*****

- An empty <gw:gawatiMeta /> tag is created within <gw:proprietary> tag for new documents.
- When a new document is saved, it gets reloaded in the edit mode. Custom metadata tab is now enabled.
- Only the selected fields get saved as different tags within <gw:gawatiMeta />
- In the view mode, the selector and save button are hidden. Only the values are shown.