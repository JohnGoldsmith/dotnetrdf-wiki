[[Home]] > [[User Guide|UserGuide]] > [[Configuration API|UserGuide-Configuration-API]] > [[Proxy Servers|UserGuide-Configuration-Proxy-Servers]]

# Configuring Proxy Servers 

Proxy Servers are configured very simply as follows:

```turtle

@prefix <http://www.dotnetrdf.org/configuration#> .

_:proxy a dnr:Proxy ;
  dnr:server "http://proxy.example.com" ;
  dnr:user "username" ;
  dnr:password "password" .
```

Username and password are optional for proxies. Note also that no `dnr:type` property is required.