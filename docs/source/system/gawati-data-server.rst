Gawati Data Server
##################

`Gawati Data Server <https://github.com/gawati/gawati-data>`__ is the
document repository for Gawati documents.

It provides REST services to provide access to the data from other
applications, the principal application being the Portal.

The document repository can reside on the same app-server installation
as the Portal, or on a different application server or an entirely
different physical server.

The only interface provided to read and write data to the document
repository is as REST services.

The diagram below expresses this --

.. figure:: ./_images/gawati-data-service.png
   :alt: Gawati Data Server
   :align: center
   :figclass: align-center

   Gawati Data Server

This allows having multiple consumers and producers of documents
independent of the Portal.

The Gawati portal is also a native eXist application, but we don't
connect to the data natively to keep our system flexible. That way we
can deploy the data independent of the portal or any other application,
and we can do things like failover for just the data server component.
