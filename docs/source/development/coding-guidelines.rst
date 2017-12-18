Coding Guidelines
#################

General
=======

This provides general coding guidelines for Gawati code. This document
covers different programming languages used in Gawati: XQuery (.xql,
.xqm), JavaScript (.js), Java (.java). The objective of having
guidelines is to keep the code-base consistent and readable which is a
critical part keeping the code maintainable and manageable.

Basic Rules across all languages
================================

No mixing of tabs and spaces
----------------------------

The code must use 4 spaces (instead of a tab) for indentation, except
for third party libraries (e.g. XSLTforms, YUI). Most modern code
editing tools support mapping a tab to 4 spaces. Every logical depth
level in the code needs to have an indent. There should be no whitespace
at the end of the line.

Use common sense
----------------

To make code readable on the screen.

Line length
-----------

Must be kept to a maximum of 80 characters (or less). This allows
reading of the code on laptops without side-scrolling.

Unix style line-breaks
----------------------

Use `unix style
line-breaks <http://www.cs.toronto.edu/~krueger/csc209h/tut/line-endings.html>`__
rather windows line breaks.

Comments
--------

Comments need to talk about the "why". This is useful not just for
others but for yourself when you read the code at a later time. They
need to be well written and clear and need to state a date and an author
if it is important commentary worth revisiting. For such revisit
comments, a format has been specified below.

*Revisit* Comments
~~~~~~~~~~~~~~~~~~

Use *Revisit* comments for code that is temporary, a short-term solution
to be reworked, or for code on which there is still questions to be
resolved or understood. Multiple related *Revisit* comments (that use
same *label*) may be used across the project source, and across
different languages.

A *Revisit* comments should have a: 

* ``!+`` : must explicitly start with these 2 characters 
* *label* : for easier grepping, and to be able to relate multiple comments across the project source (even in different languages)
* *author identifier* : so others know who to follow-up with if needed, this can be the github handle of the committer. 
* *date* : so it is easier to judge relevance and status at a later time 
* *description* : should also indicate what conditions/events would render the comment obsolete.


Formal Documentation comments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These formal documentation comments allow running documentation
generation tools to extract documentation from code. For this we
recommend the following for different programming languages

-  Javascript - `SphinxJS <https://pypi.python.org/pypi/sphinx-js/>`__ style comments for
   functions and modules
-  XQuery - `XQDoc <http://exist-db.org/exist/apps/doc/xqdoc.xml>`__
   style comments for functions and modules
-  Java -
   `JavaDoc <http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html>`__
   style comments for functions and classes.

Language Specific Coding Guidelines
===================================

The below are coding guidelines of how the code is to be strucutured and written. The reason for doing this is to have consistent and readable code. Many programmging languages and frameworks provide multiple ways of doing the same thing - for e.g. in React JS you can declare components either declaring a class directly, or by calling a spefific createClass api, both approaches may look correct, but both result in different looking code -- which reduces readability. These coding guidelines below for different languages and frameoworks encourage uniformity and consistency. 

React JS  
--------

Our front-end is primarily using React JS, and we recommend following the `Airbnb React JS coding  style guide <<https://github.com/airbnb/javascript/tree/master/react>`__ . 


