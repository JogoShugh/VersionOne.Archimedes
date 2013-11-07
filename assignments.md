# Archimedes

To use queries below, see documentation on [Server Base URI](http://community.versionone.com/Developers/Developer-Library/Concepts/Server_Base_URI) and [HTTP Clients](http://community.versionone.com/Developers/Developer-Library/Concepts/HTTP_Client).

## Members' Actuals working on Epics

With newlines for readability:

```
<Server Base URI>/rest-1.v1/Data/Member
	?sel=Name,Email,Actuals[Date=$DATE;Workitem.SuperAndUp.Number=$EPIC].Value.@Sum
	&where=Actuals[Date=$DATE;Workitem.SuperAndUp.Number=$EPIC]
	&with=$DATE=2011-09-20&$EPIC=E-01001
```

## Actuals with Member and Root Epic

```
<Server Base URI>/rest-1.v1/Data/Actual
	?sel=Date,Value,Member.Name,Member.Email,Workitem.SuperAndUp:Epic[SuperAndUp.@Count=$LEVEL]
	&where=Workitem.SuperAndUp:Epic[SuperAndUp.@Count=$LEVEL]
	&with=$LEVEL=0
```

The query needs to be run twice to account for actuals that:
* roll up to only a root level epic
* roll up to a "feature epic" that is a child of a root epic

Note: it would be a good idea to have additional `where` parameters to narrow the scope, for example with `Date>$DATE`. In systems where people have been entering time for a while, the result set may exceed timeouts and result in an HTTP 500.

## Actuals with Member and Root Epic by Date

```
<Server Base URI>/rest-1.v1/Data/Actual
	?sel=Date,Value,Member.Name,Member.Email,Workitem.SuperAndUp:Epic[SuperAndUp.@Count=$LEVEL]
	&where=Workitem.SuperAndUp:Epic[SuperAndUp.@Count=$LEVEL];Date=$DATE
	&with=$LEVEL=0=$DATE=2011-09-20
```

The query needs to be run twice to account for actuals that:
* roll up to only a root level epic
* roll up to a "feature epic" that is a child of a root epic

