Checklist To Fix Documents
##########################

************
Introduction
************

This check-list covers checking number and country-code missing metadata in the document.


*********************
Opening the documents
*********************

Open the XML document and Open the PDF document side by side.
They follow a similar naming convention. 

 * xml file: akn_123647.xml
 * pdf file: akn_123647.pdf

**************
Items to Check
**************

========================
1 Check for Country Code
========================

Look in the `<an:identification>` tags:

.. code-block:: xml
    :linenos:

    <an:identification source="#gawati">
        <an:FRBRWork>
            <an:FRBRthis value="/akn//act/2000-10-18/resolution_leg182/!main"/>
            <an:FRBRuri value="/akn//act/2000-10-18/resolution_leg182"/>
            <an:FRBRdate name="Work Date" date="2000-10-18"/>
            <an:FRBRauthor href="#author"/>
            <an:FRBRcountry value="" showAs=""/>
            <an:FRBRnumber value="resolution_leg182" showAs="RESOLUTION LEG.1(82)"/>
            <an:FRBRprescriptive value="false"/>
            <an:FRBRauthoritative value="false"/>
        </an:FRBRWork>
        <an:FRBRExpression>
            <an:FRBRthis value="/akn//act/2000-10-18/resolution_leg182/eng@/!main"/>
            <an:FRBRuri value="/akn//act/2000-10-18/resolution_leg182/eng@"/>
            <an:FRBRdate name="Expression Date" date="2000-10-18"/>
            <an:FRBRauthor href="#author"/>
            <an:FRBRlanguage language="eng"/>
        </an:FRBRExpression>
        <an:FRBRManifestation>
            <an:FRBRthis value="/akn//act/2000-10-18/resolution_leg182/eng@/!main.xml"/>
            <an:FRBRuri value="/akn//act/2000-10-18/resolution_leg182/eng@/.akn"/>
            <an:FRBRdate name="Manifestation Date" date="2015-09-23"/>
            <an:FRBRauthor href="#author"/>
            <an:FRBRformat value="xml"/>
        </an:FRBRManifestation>
    </an:identification>


Within this - check the :code:`<an:FRBRcountry>` element. 

In the above example it looks like:

.. code-block:: xml
    :linenos:

    <an:FRBRcountry value="" showAs=""/>

The :code:`@value` and :code:`@showAs` attributes are empty. ** This is an error **
These attributes are used in other elements of the :code:`<an:identification>` element.

For exampe, within :code:`<an:FBRthis>` and :code:`<an:FRBRuri>`

.. code-block:: xml
    :linenos:

    <an:FRBRWork>
	<an:FRBRthis value="/akn//act/2000-10-18/resolution_leg182/!main"/>
	<an:FRBRuri value="/akn//act/2000-10-18/resolution_leg182"/>
        ....
    </an:FRBRWork>



