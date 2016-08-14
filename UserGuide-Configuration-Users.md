[[Home]] > [[User Guide|UserGuide]] > [[Configuration API|UserGuide-Configuration-API]] > [[Users|UserGuide-Configuration-Users]]

# Configuring Users 

Users are configured very simply as follows:

```turtle

@prefix <http://www.dotnetrdf.org/configuration#> .

_:user a dnr:User ;
  dnr:user "username" ;
  dnr:password "password" .
```

Users may be used in conjunction with [[User Groups|UserGuide-Configuration-User-Groups]] and [[Permissions|UserGuide-Configuration-Permissions]] in a limited way to control access to HTTP Handlers though this is not fully implemented in the current version of dotNetRDF.

You may also use a User when you need to define credentials for anything that may directly take a `dnr:user` and `dnr:password` properties.  In this case you can refer to a User using the `dnr:credentials` property.