.. code-block:: apacheconf
  :linenos:

  <Location "/gw/service/short-filter-cache/">
    AddType text/cache-manifest .appcache
    ProxyPass  "http://localhost:9001/gwp/short-filter-cache"
    ProxyPassReverse "http://localhost:9001/gwp/short-filter-cache"
    ProxyPassReverseCookiePath /exist /
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>

  <Location "/gw/service/smart-filter-cache/">
    AddType text/cache-manifest .appcache
    ProxyPass  "http://localhost:9001/gwp/smart-filter-cache"
    ProxyPassReverse "http://localhost:9001/gwp/smart-filter-cache"
    ProxyPassReverseCookiePath /exist /
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>

  <Location "/gw/service/full-filter-cache/">
    AddType text/cache-manifest .appcache
    ProxyPass  "http://localhost:9001/gwp/full-filter-cache"
    ProxyPassReverse "http://localhost:9001/gwp/full-filter-cache"
    ProxyPassReverseCookiePath /exist /
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>

  ##
  ## Added for portal-server 1.0.7
  ##

  ## keyword filter service
  <Location "/gw/service/keyword/">
    AddType text/cache-manifest .appcache
    ProxyPass  "http://localhost:9001/gwp/keyword"
    ProxyPassReverse "http://localhost:9001/gwp/keyword"
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>

  ## keyword value query service
  <Location "/gw/service/keywordValue/">
    AddType text/cache-manifest .appcache
    ProxyPass  "http://localhost:9001/gwp/keywordValue"
    ProxyPassReverse "http://localhost:9001/gwp/keywordValue"
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>

  ## used for cms page delivery
  <Location "/gw/service/content/">
    AddType text/cache-manifest .appcache
    ProxyPass  "http://localhost:9001/gwp/content"
    ProxyPassReverse "http://localhost:9001/gwp/content"
    SetEnv force-proxy-request-1.0 1
    SetEnv proxy-nokeepalive 1
  </Location>
