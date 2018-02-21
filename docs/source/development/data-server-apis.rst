################
Data Server APIs
################

************
Introduction
************

The Data Server has various APIs to access the Data from the Gawati System.
The Data is accessible via REST APIs, which have been documented here.
Full documents are served as `Akoma Ntoso`_ Documents.

These APIs allow accessing a list of documents or the document itself using different kinds of queries.
Each API has 2 end points, an XML endpoint that produces outputs in XML (`text/xml`), and a JSON (`application/json`) end point  that produces outputs in JSON. 

.. contents:: Table of Contents 
  :local:

Listing of documents filtered by language
=========================================

Returns a list of document abstracts (summaries) filtered by a specific language.

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/search/languages/summary`
  - JSON endpoint - `/gwd/search/languages/summary/json`
 * Parameters:
  - doclang - `ISO639-2 Alpha 3 <http://www.loc.gov/standards/iso639-2/php/English_list.php>`_ language code
  - count - the number of results to return
  - from - the paging point where to return results from.

**Example:**

The following makes a request for 1 english document in json format.  

.. code-block:: bash
  :linenos:

  $ curl "http://localhost/gwd/search/languages/summary/json?doclang=eng&count=1&from=1"


returns: 

.. code-block:: json
  :linenos:

  {
      "timestamp": "2018-02-21T10:36:34.371+05:30",
      "exprAbstracts": {
          "orderedby": "dt-updated-desc",
          "records": "23",
          "pagesize": "1",
          "itemsfrom": "1",
          "totalpages": "23",
          "currentpage": "2",
          "exprAbstract": {
              "expr-iri": "/akn/mr/act/2011-01-01/gn_no_86-2011/eng@/!main",
              "work-iri": "/akn/mr/act/2011-01-01/gn_no_86-2011/!main",
              "date": [
                  {
                      "name": "work",
                      "value": "2011-01-01"
                  },
                  {
                      "name": "expression",
                      "value": "2011-01-01"
                  }
              ],
              "type": {
                  "name": "legislation",
                  "aknType": "act"
              },
              "country": {
                  "value": "mr",
                  "showAs": "Mauritania"
              },
              "language": {
                  "value": "eng",
                  "showAs": "English"
              },
              "publishedAs": "Distributive Trades (Remuneration Order) (Amendment) Regulations 2011 (Amended)",
              "number": {
                  "value": "gn_no_86-2011",
                  "showAs": "GN No. 86/2011"
              },
              "componentLink": {
                  "src": "/akn/mr/act/2011-01-01/gn_no_86-2011/eng@/!main.pdf",
                  "value": "akn_mr_act_2011-01-01_gn_no_86-2011_eng_main.pdf"
              },
              "thumbnail": {"src": "th_akn_mr_act_2011-01-01_gn_no_86-2011_eng_main.png"}
          }
      }
  }

The following makes a request for 1 english document in XML format. 

.. code-block:: bash
  :linenos:

  $ curl "http://localhost/gwd/search/languages/summary?doclang=eng&count=1&from=1"


returns:

.. code-block:: xml
  :linenos:

  <gwd:package xmlns:gwd="http://gawati.org/ns/1.0/data" timestamp="2018-02-21T10:37:55.612+05:30">
      <gwd:exprAbstracts orderedby="dt-updated-desc" records="23" pagesize="1" itemsfrom="1" totalpages="23" currentpage="2">
          <gwd:exprAbstract expr-iri="/akn/mr/act/2011-01-01/gn_no_86-2011/eng@/!main" work-iri="/akn/mr/act/2011-01-01/gn_no_86-2011/!main">
              <gwd:date name="work" value="2011-01-01"/>
              <gwd:date name="expression" value="2011-01-01"/>
              <gwd:type name="legislation" aknType="act"/>
              <gwd:country value="mr" showAs="Mauritania"/>
              <gwd:language value="eng" showAs="English"/>
              <gwd:publishedAs>Distributive Trades (Remuneration Order) (Amendment) Regulations 2011 (Amended)</gwd:publishedAs>
              <gwd:number value="gn_no_86-2011" showAs="GN No. 86/2011"/>
              <gwd:componentLink src="/akn/mr/act/2011-01-01/gn_no_86-2011/eng@/!main.pdf" value="akn_mr_act_2011-01-01_gn_no_86-2011_eng_main.pdf"/>
              <gwd:thumbnail src="th_akn_mr_act_2011-01-01_gn_no_86-2011_eng_main.png"/>
          </gwd:exprAbstract>
      </gwd:exprAbstracts>
  </gwd:package>

The outputs are exactly the same in terms of content, only the format differs.

Listing of documents filtered by year
=====================================

Returns an abstracted list of documents, filtered by the year.

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/search/years/summary`
  - JSON endpoint - `/gwd/search/years/summary/json`
 * Parameters:
  - year - four digit year
  - count - the number of results to return
  - from - the paging point where to return results from.


Listing of documents filtered by keywords
=========================================

Returns an abstracted list of documents, filtered by one or more keywords.

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/search/keywords/summary`
  - JSON endpoint - `/gwd/search/keywords/summary/json`
 * Parameters:
  - kw(1+) - keyword, this parameter can be specified multiple times
  - count - the number of results to return
  - from - the paging point where to return results from.

Note here that the `kw` parameter can be specified multiple times. 

For example, the following returns 2 documents when the keyword `Finance` is specified: 

.. code-block:: bash
  :linenos:

  $ curl "http://localhost/gwd/search/keywords/summary?kw=Finance&count=10&from=1"

returns abstracts for 2 documents:

.. code-block:: xml
  :linenos:

  <gwd:package xmlns:gwd="http://gawati.org/ns/1.0/data" timestamp="2018-02-21T10:55:33.755+05:30">
      <gwd:exprAbstracts orderedby="dt-updated-desc" records="2" pagesize="10" itemsfrom="1" totalpages="1" currentpage="1">
          <gwd:exprAbstract expr-iri="/akn/bf/judgment/2016-04-22/arrêt_no003_-2003-2004/fra@/!main" work-iri="/akn/bf/judgment/2016-04-22/arrêt_no003_-2003-2004/!main">
              <gwd:date name="work" value="2016-04-22"/>
              <gwd:date name="expression" value="2016-04-22"/>
              <gwd:type name="legislation" aknType="act"/>
              <gwd:country value="bf" showAs="Burkina Faso"/>
              <gwd:language value="fra" showAs="Français"/>
              <gwd:publishedAs>Conseil  d'Etat , Chambre du contentieux , Madame S.F. et 14 autres Magistrats c. ETAT Burkinabé ( MJ) , 28 novembre 2003 ,Arrêt n°003 /2003-2004</gwd:publishedAs>
              <gwd:number value="arrêt_no003_-2003-2004" showAs="Arrêt n°003 /2003-2004"/>
              <gwd:componentLink src="/akn/bf/judgment/2016-04-22/arrêt_no003_-2003-2004/fra@/!main.pdf" value="akn_bf_judgment_2016-04-22_arrêt_no003_-2003-2004_fra_main.pdf"/>
              <gwd:thumbnail src="th_akn_bf_judgment_2016-04-22_arrêt_no003_-2003-2004_fra_main.png"/>
          </gwd:exprAbstract>
          <gwd:exprAbstract expr-iri="/akn/bf/judgment/2016-05-12/jugemement_no_088/fra@/!main" work-iri="/akn/bf/judgment/2016-05-12/jugemement_no_088/!main">
              <gwd:date name="work" value="2016-05-12"/>
              <gwd:date name="expression" value="2016-05-12"/>
              <gwd:type name="legislation" aknType="act"/>
              <gwd:country value="bf" showAs="Burkina Faso"/>
              <gwd:language value="fra" showAs="Français"/>
              <gwd:publishedAs>Tribunal du travail de Ouagadougou( Burkina Faso), Monsieur K.J.M.c Monsieur B.A. , 20 MAI 2005 , JUGEMEMENT N° 088</gwd:publishedAs>
              <gwd:number value="jugemement_no_088" showAs="JUGEMEMENT N° 088"/>
              <gwd:componentLink src="/akn/bf/judgment/2016-05-12/jugemement_no_088/fra@/!main.pdf" value="akn_bf_judgment_2016-05-12_jugemement_no_088_fra_main.pdf"/>
              <gwd:thumbnail src="th_akn_bf_judgment_2016-05-12_jugemement_no_088_fra_main.png"/>
          </gwd:exprAbstract>
      </gwd:exprAbstracts>
  </gwd:package>

When queried for 2 keywords, like below: 

.. code-block:: bash
  :linenos:

  $ curl "http://localhost/gwd/search/keywords/summary?kw=Finance&kw=Tax&count=10&from=1"


returns 3 documents (i.e. documents having either of the 2 keywords) : 

.. code-block:: xml
  :linenos:

  <gwd:package xmlns:gwd="http://gawati.org/ns/1.0/data" timestamp="2018-02-21T11:00:45.176+05:30">
      <gwd:exprAbstracts orderedby="dt-updated-desc" records="3" pagesize="10" itemsfrom="1" totalpages="1" currentpage="1">
          <gwd:exprAbstract expr-iri="/akn/bf/judgment/2016-04-22/arrêt_no003_-2003-2004/fra@/!main" work-iri="/akn/bf/judgment/2016-04-22/arrêt_no003_-2003-2004/!main">
              <gwd:date name="work" value="2016-04-22"/>
              <gwd:date name="expression" value="2016-04-22"/>
              <gwd:type name="legislation" aknType="act"/>
              <gwd:country value="bf" showAs="Burkina Faso"/>
              <gwd:language value="fra" showAs="Français"/>
              <gwd:publishedAs>Conseil  d'Etat , Chambre du contentieux , Madame S.F. et 14 autres Magistrats c. ETAT Burkinabé ( MJ) , 28 novembre 2003 ,Arrêt n°003 /2003-2004</gwd:publishedAs>
              <gwd:number value="arrêt_no003_-2003-2004" showAs="Arrêt n°003 /2003-2004"/>
              <gwd:componentLink src="/akn/bf/judgment/2016-04-22/arrêt_no003_-2003-2004/fra@/!main.pdf" value="akn_bf_judgment_2016-04-22_arrêt_no003_-2003-2004_fra_main.pdf"/>
              <gwd:thumbnail src="th_akn_bf_judgment_2016-04-22_arrêt_no003_-2003-2004_fra_main.png"/>
          </gwd:exprAbstract>
          <gwd:exprAbstract expr-iri="/akn/bf/judgment/2016-05-12/jugemement_no_088/fra@/!main" work-iri="/akn/bf/judgment/2016-05-12/jugemement_no_088/!main">
              <gwd:date name="work" value="2016-05-12"/>
              <gwd:date name="expression" value="2016-05-12"/>
              <gwd:type name="legislation" aknType="act"/>
              <gwd:country value="bf" showAs="Burkina Faso"/>
              <gwd:language value="fra" showAs="Français"/>
              <gwd:publishedAs>Tribunal du travail de Ouagadougou( Burkina Faso), Monsieur K.J.M.c Monsieur B.A. , 20 MAI 2005 , JUGEMEMENT N° 088</gwd:publishedAs>
              <gwd:number value="jugemement_no_088" showAs="JUGEMEMENT N° 088"/>
              <gwd:componentLink src="/akn/bf/judgment/2016-05-12/jugemement_no_088/fra@/!main.pdf" value="akn_bf_judgment_2016-05-12_jugemement_no_088_fra_main.pdf"/>
              <gwd:thumbnail src="th_akn_bf_judgment_2016-05-12_jugemement_no_088_fra_main.png"/>
          </gwd:exprAbstract>
          <gwd:exprAbstract expr-iri="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/!main" work-iri="/akn/mr/act/1963-10-12/gn_no_150-1983/!main">
              <gwd:date name="work" value="1963-10-12"/>
              <gwd:date name="expression" value="1963-10-12"/>
              <gwd:type name="legislation" aknType="act"/>
              <gwd:country value="mr" showAs="Mauritania"/>
              <gwd:language value="eng" showAs="English"/>
              <gwd:publishedAs>Sales Tax (Amendment of schedule) Regulations 1983 (Amended)</gwd:publishedAs>
              <gwd:number value="gn_no_150-1983" showAs="GN No. 150/1983"/>
              <gwd:componentLink src="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/!main.pdf" value="akn_mr_act_1963-10-12_gn_no_150-1983_eng_main.pdf"/>
              <gwd:thumbnail src="th_akn_mr_act_1963-10-12_gn_no_150-1983_eng_main.png"/>
          </gwd:exprAbstract>
      </gwd:exprAbstracts>
  </gwd:package>



Listing of documents filtered by countries
==========================================

Returns an abstracted list of documents, filtered by one or more countries.

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/search/countries/summary`
  - JSON endpoint - `/gwd/search/countries/summary/json`
 * Parameters:
  - country(1+) - country, this parameter can be specified multiple times. Countries are specified as `ISO ALPH-2 country codes <http://www.nationsonline.org/oneworld/country_code_list.htm>`_.
  - count - the number of results to return
  - from - the paging point where to return results from.

Note here that the `country` parameter can be specified multiple times. 


Listing of recent documents
===========================

Returns documents based on recency. Most recent appear first. Recency is established based on the updated date metadata of the document : 

.. code-block:: xml
  :linenos:

  <gw:dateTime refersTo="#dtModified" datetime="2016-05-09T11:07:51.725Z"/>


 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/recent/expressions/summary`
  - JSON endpoint - `/gwd/recent/expressions/summary/json`
 * Parameters:
  - count - the number of results to return
  - from - the paging point where to return results from.


Return a complete document based on its IRI
===========================================

Returns documents based on recency. Most recent appear first. Recency is established based on the updated date metadata of the document : 

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/doc`
  - JSON endpoint - `/gwd/doc/json`
 * Parameters:
  - iri - The FRBRthis/@value of the document to be retrieved

For example, to retreive the document with the IRI `/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/!main` the following: 

.. code-block:: bash
  :linenos:

  $ curl "http://localhost/gwd/doc?iri=/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/%21main"

Note that to run this with curl we need to escape the `!` character in the IRI as `%21`.
returns, the XML document:

.. code-block:: xml
  :linenos:
    
  <an:akomaNtoso xmlns:gw="http://gawati.org/ns/1.0" xmlns:gxsl="http://gawati.org/ns/1.0/xsl" xmlns:an="http://docs.oasis-open.org/legaldocml/ns/akn/3.0">
      <an:act name="act">
          <an:meta>
              <an:identification source="#gawati">
                  <an:FRBRWork>
                      <an:FRBRthis value="/akn/mr/act/1963-10-12/gn_no_150-1983/!main"/>
                      <an:FRBRuri value="/akn/mr/act/1963-10-12/gn_no_150-1983"/>
                      <an:FRBRdate name="Work Date" date="1963-10-12"/>
                      <an:FRBRauthor href="#author"/>
                      <an:FRBRcountry value="mr" showAs="Mauritania"/>
                      <an:FRBRnumber value="gn_no_150-1983" showAs="GN No. 150/1983"/>
                      <an:FRBRprescriptive value="false"/>
                      <an:FRBRauthoritative value="false"/>
                  </an:FRBRWork>
                  <an:FRBRExpression>
                      <an:FRBRthis value="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/!main"/>
                      <an:FRBRuri value="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@"/>
                      <an:FRBRdate name="Expression Date" date="1963-10-12"/>
                      <an:FRBRauthor href="#author"/>
                      <an:FRBRlanguage language="eng"/>
                  </an:FRBRExpression>
                  <an:FRBRManifestation>
                      <an:FRBRthis value="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/!main.xml"/>
                      <an:FRBRuri value="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/.akn"/>
                      <an:FRBRdate name="Manifestation Date" date="2016-05-09"/>
                      <an:FRBRauthor href="#author"/>
                      <an:FRBRformat value="xml"/>
                  </an:FRBRManifestation>
              </an:identification>
              <an:publication date="1963-10-12" showAs="Sales Tax (Amendment of schedule) Regulations 1983 (Amended)" name="Act" number="GN No. 150/1983"/>
              <an:classification source="#legacy">
                  <an:keyword eId="ontology.dictionary.gawati.legacy.Regulation" value="Regulation" showAs="Regulation" dictionary="#gawati-legacy"/>
                  <an:keyword eId="ontology.dictionary.gawati.legacy.Sale" value="Sale" showAs="Sale" dictionary="#gawati-legacy"/>
                  <an:keyword eId="ontology.dictionary.gawati.legacy.Tax" value="Tax" showAs="Tax" dictionary="#gawati-legacy"/>
                  <an:keyword eId="ontology.dictionary.gawati.legacy.Act" value="Act" showAs="Act" dictionary="#gawati-legacy"/>
                  <an:keyword eId="ontology.dictionary.gawati.legacy.Schedule" value="Schedule" showAs="Schedule" dictionary="#gawati-legacy"/>
              </an:classification>
              <an:lifecycle source="#all">
                  <an:eventRef date="1963-10-12" source="#original" type="generation"/>
                  <an:eventRef date="1983-10-12" source="#original" type="generatio" refersTo="#dtInForce"/>
              </an:lifecycle>
              <an:references source="#source">
                  <an:original eId="original" href="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/!main" showAs="GN No. 150/1983"/>
                  <an:TLCOrganization eId="all" href="/ontology/Organization/AfricanLawLibrary" showAs="African Law Library"/>
                  <an:TLCEvent eId="dtInForce" href="/ontology/Event/ALL/InForce" showAs="Entry into Force Date"/>
                  <an:TLCConcept eId="ct-Legal-Finance" href="/ontology/Concept/Legacy/Legal/Finance" showAs="Finance"/>
                  <an:TLCConcept eId="ct-Legal-Tax" href="/ontology/Concept/Legacy/Legal/Tax" showAs="Tax"/>
                  <an:TLCConcept eId="ct-Legal-TradeAndIndustry" href="/ontology/Concept/Legacy/Legal/TradeAndIndustry" showAs="Trade and Industry"/>
                  <an:TLCConcept eId="ct-Legal-TradeLaw" href="/ontology/Concept/Legacy/Legal/TradeLaw" showAs="Trade Law"/>
              </an:references>
              <an:proprietary source="#all">
                  <gw:gawati>
                      <gw:legacyIdentifier>africanlawlib:6741344</gw:legacyIdentifier>
                      <gw:legacyOwner>16956378</gw:legacyOwner>
                      <gw:legacyCollection>L03033 Mauritius</gw:legacyCollection>
                      <gw:languages>
                          <gw:language code="eng"/>
                      </gw:languages>
                      <gw:embeddedContents>
                          <gw:embeddedContent eId="embedded-doc-1" type="pdf" file="MUSCM_1983GN150.pdf" state="true"/>
                      </gw:embeddedContents>
                      <gw:dateTime refersTo="#dtCreated" datetime="2016-03-18T11:12:51.964Z"/>
                      <gw:dateTime refersTo="#dtModified" datetime="2016-05-09T11:07:51.725Z"/>
                      <gw:date refersTo="#dtInForce" date="1983-10-12"/>
                      <gw:themes source="#legacy">
                          <gw:theme refersTo="#ct-Legal-Finance"/>
                          <gw:theme refersTo="#ct-Legal-Tax"/>
                          <gw:theme refersTo="#ct-Legal-TradeAndIndustry"/>
                          <gw:theme refersTo="#ct-Legal-TradeLaw"/>
                      </gw:themes>
                  </gw:gawati>
              </an:proprietary>
          </an:meta>
          <an:body>
              <an:book refersTo="#mainDocument">
                  <an:componentRef src="/akn/mr/act/1963-10-12/gn_no_150-1983/eng@/!main.pdf" alt="akn_mr_act_1963-10-12_gn_no_150-1983_eng_main.pdf" GUID="#embedded-doc-1" showAs="Sales Tax (Amendment of schedule) Regulations 1983 (Amended)"/>
              </an:book>
          </an:body>
      </an:act>
  </an:akomaNtoso>


Advanced Document Filtering API
===============================

This API allows a more complex combination of filters, and conducting searches on the data server. 

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/search/filter`
  - JSON endpoint - `/gwd/search/filter/json`
 * Parameters:
  - q - filter query, based on filter XQuery syntax (see below).
  - count - the number of results to return
  - from - the paging point where to return results from.

The query syntax for the `q` parameter runs pseudo XQuery: 

For example, when `q` is set to the below, it returns documents only from the year 2016:

.. code-block:: xml
  :linenos:

  [.//an:FRBRdate[ year-from-date(@date) eq 2016 ]]

The below instead returns documents from both 2014 and 2016: 

.. code-block:: xml
  :linenos:

  [.//an:FRBRdate[ year-from-date(@date) eq 2016 or year-from-date(@date) eq 2014]]

Note that the Portal front-end does not directly compose this query, there is an intermediate query translation api that lets the server make the request using a JSON based syntax. The below is the JSON query of the above :  

.. code-block:: json
  :linenos:

  {"year": [2014, 2016]}

These filters can be stacked, the below searches for documents from 2014 and 2016 which are from "Burkina Faso": 

.. code-block:: xquery
  :linenos:

  [.//an:FRBRdate[ year-from-date(@date) eq 2016 or year-from-date(@date) eq 2014 ]][.//an:FRBRcountry[ @value eq 'bf' ]]

The Json query of the same would look like:

.. code-block:: json
  :linenos:

  {"year": [2014, 2016], "countries": ["bf"]}

At this point the data server does not support JSON queries yet, but eventually the XQuery based API will be migrated to support only the JSON based API.

Another stacked filter supported is the language of the document: 

.. code-block:: xquery
  :linenos:

  [.//an:FRBRdate[ year-from-date(@date) eq 2016 or year-from-date(@date) eq 2014 ]]
  [.//an:FRBRcountry[ @value eq 'bf' ]]
  [.//an:FRBRlanguage[ @language eq 'eng' ]] <=== 

And finally Keywords: 

.. code-block:: xquery
  :linenos:

  [.//an:FRBRdate[ year-from-date(@date) eq 2016 or year-from-date(@date) eq 2014 ]]
  [.//an:FRBRcountry[ @value eq 'bf' ]]
  [.//an:FRBRlanguage[ @language eq 'eng' ]]
  [.//an:classification/an:keyword[ @value eq 'Trade' or @value eq 'FinancialLaw' ]] <== 





.. _Akoma Ntoso: https://en.wikipedia.org/wiki/Akoma_Ntoso
