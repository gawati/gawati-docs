################
Data Server APIs
################

************
Introduction
************

The Data Server has various APIs to access the Data from the Gawati System.
The Data is accessible via REST APIs, which have been documented here.
Full documents are served as `Akoma Ntoso`_ Documents.

************
Listing APIs
************

These APIs allow accessing a list of documents using different kinds of queries.
Each API has 2 end points, an XML endpoint that produces outputs in XML (`text/xml`), and a JSON (`application/json`) end point  that produces outputs in JSON. 

Listing of documents filtered by language
=========================================

Returns a list of document abstracts (summaries) filtered by a specific language.

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/search/languages/summary`
  - JSON endpoint - `/gwd/search/languages/summary/json`
 * Parameters:
  - doclang - `ISO639-2 Alpha 3<http://www.loc.gov/standards/iso639-2/php/English_list.php>`_ language code
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


returns 3 documents: 

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



.. _Akoma Ntoso: https://en.wikipedia.org/wiki/Akoma_Ntoso
