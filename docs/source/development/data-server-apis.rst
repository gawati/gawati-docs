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

Getting a summary of recent expressions
=======================================

This API returns a summary of the expressions ordered by modified date in
descending order (most recent appears first).

+--------------------------------------------------------------+
| API PATH & METHOD                                            |
+==============================================================+
| /gw/recent/expressions/summary  (GET)                        |
+--------------------------------------------------------------+
| PARAMETERS                                                   |
+--------------------------------------------------------------+
| count (integer) - the number of documents to return per page |
+--------------------------------------------------------------+
| from (integer) - the page offset to start from               |
+--------------------------------------------------------------+
| PRODUCES                                                     |
+--------------------------------------------------------------+
| text/xml                                                     |
+--------------------------------------------------------------+

Example, a call like this::

  /gw/recent/expressions/summary?count=10&amp;from=1

Will return::

  <gwd:package xmlns:gwd="http://gawati.org/ns/1.0/data" timestamp="2017-09-26T23:07:42.287+05:30">
      <gwd:exprAbstracts orderedby="dt-updated-desc" totalpages="408" currentpage="1">
          <gwd:exprAbstract expr-iri="/akn/bj/act/1987-06-03/arrete_interministeriel_no_040/fra@/!main" work-iri="/akn/bj/act/1987-06-03/arrete_interministeriel_no_040/!main">
              <gwd:date name="work" value="1987-06-03"/>
              <gwd:date name="expression" value="1987-06-03"/>
              <gwd:country value="bj" showAs="Benin"/>
              <gwd:language value="fra"/>
              <gwd:publishedAs showAs="ARRETE INTERMINISTERIEL NO. 040 MISPAT/MCAT/DCM/DTH/DSP DU 03 JUIN 1987 PORTANT APPLICATION DU DECRET NO. 87-76 DU 7 AVRIL 1987 RELATIF AUX MODALITES D'INSTALLATION ET D'EXPLOITATION DES ETABLISSEMENTS DE RESTAURATION ET ASSIMILES EN REPUBLIQUE POPULAIRE DU BENIN. MISPAT/MCAT/DCM/DTH/DSP DU 03 JUIN 1987 PORTANT APPLICATION DU DECRET NO. 87-76 DU 7 AVRIL 1987 RELATIF AUX MODALITES D'INSTALLATION ET D'EXPLOITATION DES ETABLISSEMENTS DE RESTAURATION ET ASSIMILES EN REPUBLIQUE POPULAIRE DU BENIN."/>
              <gwd:number value="arrete_interministeriel_no_040" showAs="ARRETE INTERMINISTERIEL NO. 040"/>
              <gwd:componentLink value="akn_bj_act_1987-06-03_arrete_interministeriel_no_040_fra_main.pdf"/>
              <gwd:thumbnailPresent value="false"/>
          </gwd:exprAbstract>
          <gwd:exprAbstract expr-iri="/akn/bj/act/1986-10-01/loi_86-014/fra@/!main" work-iri="/akn/bj/act/1986-10-01/loi_86-014/!main">
              <gwd:date name="work" value="1986-10-01"/>
              <gwd:date name="expression" value="1986-10-01"/>
              <gwd:country value="bj" showAs="Benin"/>
              <gwd:language value="fra"/>
              <gwd:publishedAs showAs="Loi 86-014 du 26 Septembre 1986 portant Codes des Pensions Civiles et Militaires et conformement aux dispositions."/>
              <gwd:number value="loi_86-014" showAs="Loi 86-014"/>
              <gwd:componentLink value="akn_bj_act_1986-10-01_loi_86-014_fra_main.pdf"/>
              <gwd:thumbnailPresent value="false"/>
          </gwd:exprAbstract>
          .....
  </gwd:package>


Getting a summary of expressions filtered by theme
==================================================

Currently themes have been implemented as Keywords on documents. You can retrieve a set of documents
matching one ore more keywords.

+-------------------------------------------------------------------------------------------+
| API Path & Method                                                                         |
+===========================================================================================+
| /gw/themes/expressions/summary  (GET)                                                     |
+-------------------------------------------------------------------------------------------+
| PARAMETERS                                                                                |
+-------------------------------------------------------------------------------------------+
| themes (string) - the themes to filter for, one or more of these parameters can be passed |
+-------------------------------------------------------------------------------------------+
| count (integer) - the number of documents to return per page                              |
+-------------------------------------------------------------------------------------------+
| from (integer) - the page offset to start from                                            |
+-------------------------------------------------------------------------------------------+
| PRODUCES                                                                                  |
+-------------------------------------------------------------------------------------------+
| text/xml                                                                                  |
+-------------------------------------------------------------------------------------------+

Example calls are provided below::

  /gw/themes/expressions/summary?themes=Education&amp;themes=Candidature&amp;count=10&amp;from=1

Returns 10 documents which have either *Education* or *Candidature* as a theme.

.. _Akoma Ntoso: https://en.wikipedia.org/wiki/Akoma_Ntoso
