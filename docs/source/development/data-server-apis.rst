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

 * Method: GET
 * End-points:
  - XML endpoint - `/gwd/search/languages/summary`
  - JSON endpoint - `/gwd/search/languages/summary/json`
 * Parameters:
  - doclang - `ISO639-2 Alpha 3<http://www.loc.gov/standards/iso639-2/php/English_list.php>`_ language code
  - count - the number of results to return
  - from - the paging point where to return results from.

Example:

The following makes a request for 1 english document in json format.  

.. code-block:: bash
  :linenos:

  $ curl "http://localhost/gwd/search/languages/summary/json?doclang=eng&count=1&from=1"
  { "timestamp" : "2018-02-21T10:36:34.371+05:30", "exprAbstracts" : { "orderedby" : "dt-updated-desc", "records" : "23", "pagesize" : "1", "itemsfrom" : "1", "totalpages" : "23", "currentpage" : "2", "exprAbstract" : { "expr-iri" : "/akn/mr/act/2011-01-01/gn_no_86-2011/eng@/!main", "work-iri" : "/akn/mr/act/2011-01-01/gn_no_86-2011/!main", "date" : [{ "name" : "work", "value" : "2011-01-01" }, { "name" : "expression", "value" : "2011-01-01" }], "type" : { "name" : "legislation", "aknType" : "act" }, "country" : { "value" : "mr", "showAs" : "Mauritania" }, "language" : { "value" : "eng", "showAs" : "English" }, "publishedAs" : "Distributive Trades (Remuneration Order) (Amendment) Regulations 2011 (Amended)", "number" : { "value" : "gn_no_86-2011", "showAs" : "GN No. 86/2011" }, "componentLink" : { "src" : "/akn/mr/act/2011-01-01/gn_no_86-2011/eng@/!main.pdf", "value" : "akn_mr_act_2011-01-01_gn_no_86-2011_eng_main.pdf" }, "thumbnail" : { "src" : "th_akn_mr_act_2011-01-01_gn_no_86-2011_eng_main.png" } } } }


The following makes a request for 1 english document in XML format. 

.. code-block:: bash
  :linenos:

  $ curl "http://localhost/gwd/search/languages/summary?doclang=eng&count=1&from=1"
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


.. _Akoma Ntoso: https://en.wikipedia.org/wiki/Akoma_Ntoso
