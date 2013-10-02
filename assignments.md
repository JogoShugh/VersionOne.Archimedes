# Archimedes

To use queries below, see documentation on [Server Base URI](http://community.versionone.com/Developers/Developer-Library/Concepts/Server_Base_URI) and [HTTP Clients](http://community.versionone.com/Developers/Developer-Library/Concepts/HTTP_Client).

## Members' Actuals working on Implied Feature Epics

```
<Server Base URI>/rest-1.v1/Data/Member?sel=Name,Email,Actuals[Date=$DATE;Workitem.SuperAndUp.Number=$EPIC].Value.@Sum&where=Actuals[Date=$DATE;Workitem.SuperAndUp.Number=$EPIC]&with=$DATE=2011-09-20&$EPIC=E-01001
```
