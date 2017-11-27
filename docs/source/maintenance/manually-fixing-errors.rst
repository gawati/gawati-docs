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

================================
1 Check for Missing Country Code
================================

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
These attributes are used in other elements of the :code:`<an:identification>` element, :code:`<an:references>` element and the :code:`<an:componentRef>` element.

**Within `<an:FRBRWork>`:**

.. code-block:: xml
    :linenos:

    <an:FRBRWork>
	<an:FRBRthis value="/akn//act/2000-10-18/resolution_leg182/!main"/>
	<an:FRBRuri value="/akn//act/2000-10-18/resolution_leg182"/>
        ....
    </an:FRBRWork>


**Within `<an:FRBRExpression>`:**

.. code-block:: xml
    :linenos:

	<an:FRBRExpression>
	    <an:FRBRthis value="/akn//act/2000-10-18/resolution_leg182/eng@/!main"/>
	    <an:FRBRuri value="/akn//act/2000-10-18/resolution_leg182/eng@"/>
            ....
	</an:FRBRExpression>


**Within `<an:FRBRManifestation>`:**

.. code-block:: xml
    :linenos:

        <an:FRBRManifestation>
            <an:FRBRthis value="/akn//act/2000-10-18/resolution_leg182/eng@/!main.xml"/>
            <an:FRBRuri value="/akn//act/2000-10-18/resolution_leg182/eng@/.akn"/>
	    .....
        </an:FRBRManifestation>


**Within `<an:original>`:**

.. code-block:: xml
    :linenos:

    <an:references source="#source">
        <an:original eId="original" href="/akn//act/2000-10-18/resolution_leg182/eng@/!main"
            showAs="RESOLUTION LEG.1(82)"/>
	....
    </an:references>

**Within `<an:componentRef>`:** (note there are 2 attributes to set here: `@src` and `@alt` )

.. code-block:: xml
    :linenos:

    <an:componentRef src="/akn//act/2000-10-18/resolution_leg182/eng@/!main.pdf"
        alt="akn__act_2000-10-18_resolution_leg182_eng_main.pdf" GUID="#embedded-doc-1"
        showAs="ADOPTION OF AMENDMENTS OF THE LIMITATION AMOUNTS IN THE PROTOCOL OF 1992 TO AMEND THE INTERNATIONAL CONVENTION ON CIVIL LIABILITY FOR OIL POLLUTION DAMAGE, 1969"
    />


The country code should appear in the reference :code:`/akn/<<country code here >>>/act/2000-10-18....` after :code:`/akn//`


-----------------------
1.1 Fixing Country Code
-----------------------

First we have to determine what is the country of origin of the document. 

Open the PDF document corresponding to the XML document. 

For the above example, the document is here:  :download:`example_law.pdf <./_static/example_law.pdf>`

Scan the document content to understand which country it is from. The above PDF upon scanning we see:

.. figure:: ./_images/change.png
   :alt: Example Law
   :align: center
   :figclass: align-center

We can see that it is not from a specific country but is from an international organization *International Maritime Organization* . 

Now open the Excel sheet of country codes, provided here: :download:`countries.xls <./_static/countries.xls>`.

The excel sheet provides country code in the first column and the country name in the 2nd. 

You will find the country code for International Maritime Organization here as:

.. code-block:: none
    :linenos:

    un-imo	International Maritime Organization

Note that down and fix it in the XML in the following places:

    1. :code:`<an:FRBRcountry value="" showAs=""/>` becomes :code:`<an:FRBRcountry value="un-imo" showAs="International Maritime Organization"/>`
    2. :code:`<an:FRBRthis value="/akn//act... />` becomes :code:`<an:FRBRthis value="/akn/un-imo/act...." />`
    3. :code:`<an:FRBRthis value="/akn//act... />` becomes :code:`<an:FRBRthis value="/akn/un-imo/act...." />`
    4. :code:`<an:FRBRuri value="/akn//act/2000-10-1... "/>` becomes :code:`<an:FRBRuri value="/akn/un-imo/act/2000-10-18..."/>`
    5. :code:`<an:original eId="original" href="/akn//act/2000-10-18..." />` becomes :code:`<an:original eId="original" href="/akn/un-imo/act/... " />`
    6. And :code:`<an:componentRef>` where:
    
    .. code-block:: xml
        :linenos:

        <an:componentRef src="/akn//act/2000-10-18/resolution_leg182/eng@/!main.pdf" alt="akn__act_2000-10-18_resolution_leg182_eng_main.pdf" />

    becomes:

    .. code-block:: xml
        :linenos:

        <an:componentRef src="/akn/un-imo/act/2000-10-18/resolution_leg182/eng@/!main.pdf" alt="akn_un-imo_act_2000-10-18_resolution_leg182_eng_main.pdf" />

    more precisely for `@alt`:

    :code:`akn__act_2000-10-18_resolution_leg182_eng_main.pdf`  becomes:

    :code:`akn_un-imo_act_2000-10-18_resolution_leg182_eng_main.pdf`


==========================
2 Check for Missing Number
==========================


Look in the `<an:identification>` tags:

.. code-block:: xml
    :linenos:

        <an:identification source="#gawati">
            <an:FRBRWork>
                <an:FRBRthis value="/akn//act/1957-06-15//!main"/>
                <an:FRBRuri value="/akn//act/1957-06-15/"/>
                <an:FRBRdate name="Work Date" date="1957-06-15"/>
                <an:FRBRauthor href="#author"/>
                <an:FRBRcountry value="" showAs=""/>
                <an:FRBRnumber value="" showAs=""/>
                <an:FRBRprescriptive value="false"/>
                <an:FRBRauthoritative value="false"/>
            </an:FRBRWork>
            <an:FRBRExpression>
                <an:FRBRthis value="/akn//act/1957-06-15//fra@/!main"/>
                <an:FRBRuri value="/akn//act/1957-06-15//fra@"/>
                <an:FRBRdate name="Expression Date" date="1957-06-15"/>
                <an:FRBRauthor href="#author"/>
                <an:FRBRlanguage language="fra"/>
            </an:FRBRExpression>
            <an:FRBRManifestation>
                <an:FRBRthis value="/akn//act/1957-06-15//fra@/!main.xml"/>
                <an:FRBRuri value="/akn//act/1957-06-15//fra@/.akn"/>
                <an:FRBRdate name="Manifestation Date" date="2015-03-10"/>
                <an:FRBRauthor href="#author"/>
                <an:FRBRformat value="xml"/>
            </an:FRBRManifestation>
        </an:identification>


Within this - check the :code:`<an:FRBRnumber>` element. 

*NOTE*: the above example has both number and country missing !

In the above example it looks like:

.. code-block:: xml
    :linenos:

    <an:FRBRnumber value="" showAs=""/>

The :code:`@value` and :code:`@showAs` attributes are empty. *This is an error*.
These attributes are used in other elements of the :code:`<an:identification>` element, :code:`<an:references>` element and the :code:`<an:publication>` element.

**Within `<an:original>`:** (attribute :code:`@showAs`)

.. code-block:: xml
    :linenos:

    <an:references source="#source">
         <an:original eId="original" href="/akn//act/1957-06-15//fra@/!main" showAs=""/>
	    ....
    </an:references>

**Within `<an:publication>`: (in the attribute :code:`@showAs`)

.. code-block:: xml
    :linenos:

    <an:publication date="1957-06-15"
        showAs="Arrangement de Nice concernant la classification internationale des produits et des services aux fins de l’enregistrement des marques"
        name="Act" number=""/>

The number appears after the date part  as shown below (Akoma Ntoso identifiers are always separated by a "/" )

.. code-block:: text
    :linenos:

    /akn//act/1957-06-15//!main
        ^               ^
      COUNTRY         NUMBER

-----------------
2.1 Fixing Number
-----------------

First we have to determine what is the country of origin of the document. 

Open the PDF document corresponding to the XML document. 

For the above example, the document is here:  :download:`example_law_2.pdf <./_static/example_law_2.pdf>`

Scan the document content to understand which country it is from. The above PDF upon scanning we see:

.. figure:: ./_images/change_2.png
   :alt: Example Law
   :align: center
   :figclass: align-center

We can see that it is not from a specific country but is from an international organization *World Intellectual Property Organization* - WIPO (Note the logo) . 

Set the country code as described earlier by refering to the excel sheet. 

.. code-block:: none
    :linenos:

    un-wipo	World Intellectual Property Organization

Follow the steps as described before to fix the country code.

Now you need to get the number of the document. Again scan the document for an identification number. 

Typically the number appears on the header, footer or on the first page of the document. In the case of this document it appears in the footer.

.. figure:: ./_images/change_3.png
   :alt: Example Law
   :align: center
   :figclass: align-center

Note that down and fix it in the XML in the following place:

    1. :code:`<an:FRBRnumber value="" showAs=""/>` becomes :code:`<an:FRBRnumber value="" showAs="WO019FR"/>` . *NOTE* : Set only the :code:`@showAs` attribute NOT the :code:`@value` attribute for :code:`<FRBRNumber>`.
    2. :code:`<an:original eId="original" href="/akn/un-wipo/act/1957..."  showAs="" />` becomes :code:`<an:original eId="original" href="/akn/un-wipo/act/... " showAs="WO019FR" />`
    3. And :code:`<an:publication>` where:
    
    .. code-block:: xml
        :linenos:

        <an:publication date="1957-06-15"
            showAs="Arrangement de Nice concernant la classification internationale des produits et des services aux fins de l’enregistrement des marques"
            name="Act" number=""/>

    becomes:

    .. code-block:: xml
        :linenos:

        <an:publication date="1957-06-15"
            showAs="Arrangement de Nice concernant la classification internationale des produits et des services aux fins de l’enregistrement des marques"
            name="Act" number="WO019FR"/>

        

