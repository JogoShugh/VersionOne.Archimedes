# Archimedes

To use queries below, see documentation on [Server Base URI](http://community.versionone.com/Developers/Developer-Library/Concepts/Server_Base_URI) and [HTTP Clients](http://community.versionone.com/Developers/Developer-Library/Concepts/HTTP_Client).

## Root Epics

```
<Server Base URI>/rest-1.v1/Data/Epic?where=SuperAndUp.@Count<'1'&sel=Name,Number,Reference,Category,SuperAndUp.@Count
```

We want a list of Epics so we use `Epic` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want to filter on root Epics so we use `SuperAndUp.@Count<'1'` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where). In other words, all of the Epics that have zero parent epics.

We want the name and ID for the feature epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). The VersionOne [OID](http://community.versionone.com/Developers/Developer-Library/Concepts/OID_Token) comes "free" with every query, so we don't need to specify it explicitly. But the ID that people see in the UI is `Number`. And if the ID we really want is a Clarity PPM Project ID, then we would need to store that in another attribute like `Reference`. (I recommend using custom field because other integrations may use and overwrite Reference for other purposes.) In this example, `Category` is added in case we want to also check the type. The `SuperAndUp.@Count` is added only to help verify the query is correct. If other attributes would be useful, the entire set of `Epic` attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Epic?xsl=api.xsl
```

## Root Epics and Immediate Children

```
<Server Base URI>/rest-1.v1/Data/Epic?where=SuperAndUp.@Count<'2'&sel=Name,Number,Reference,Super,Category,SuperAndUp.@Count
```

We want a list of Epics so we use `Epic` as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want to filter on root Epics so we use `SuperAndUp.@Count<'2'` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where). In other words, all of the Epics that have zero or one parent epics.

We want the name and ID for the feature epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). The VersionOne [OID](http://community.versionone.com/Developers/Developer-Library/Concepts/OID_Token) comes "free" with every query, so we don't need to specify it explicitly. But the ID that people see in the UI is `Number`. And if the ID we really want is a Clarity PPM Project ID, then we would need to store that in another attribute like `Reference`. (I recommend using custom field because other integrations may use and overwrite Reference for other purposes.) To help reconstruct this subset of the tree, we also need the parent Epic, known as `Super`. In this example, `Category` is added in case we want to also check the type. The `SuperAndUp.@Count` is added only to help verify the query is correct.