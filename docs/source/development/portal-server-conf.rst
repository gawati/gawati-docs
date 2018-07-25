.. code-block:: apacheconf
  :linenos:

  <Location ~ "/gwp/(.*)">
    AddType text/cache-manifest .appcache
    ProxyPassMatch  "http://localhost:9001/gwp/$1"
    ProxyPassReverse "http://localhost:9001/gwp/$1"
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>

  # gawati-viewer-service
  <Location ~ "/gwv/(.*)">
    AddType text/cache-manifest .appcache
    ProxyPassMatch  "http://localhost:9005/gwv/$1"
    ProxyPassReverse "http://localhost:9005/gwv/$1"
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>