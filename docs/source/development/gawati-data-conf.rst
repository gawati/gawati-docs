.. code-block:: apacheconf
  :linenos:

<Location ~ "/gwd/(.*)">
  AddType text/cache-manifest .appcache
  ProxyPassMatch  "http://localhost:8080/exist/restxq/gw/$1"
  ProxyPassReverse "http://localhost:8080/exist/restxq/gw/$1"
  ProxyPassReverseCookiePath /exist /
  SetEnv force-proxy-request-1.0 1
  SetEnv proxy-nokeepalive 1
</Location>
