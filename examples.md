# Archimedes

To use queries below, see documentation on [Server Base URI](http://community.versionone.com/Developers/Developer-Library/Concepts/Server_Base_URI) and [HTTP Clients](http://community.versionone.com/Developers/Developer-Library/Concepts/HTTP_Client).

## VersionOne Initiative Epics

```
<Server Base URI>/rest-1.v1/Data/Epic?where=Category.Name='Initiative'&sel=Name,Number,Reference,Category
```

We want a list of Epics so we use Epics as the [from](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/from).

We want to filter on just Initiative Epics so we use `Category.Name='Initiative'` as the [where](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/where). If we only used `Category` we would need to specify an OID for the list value of 'Initiative'.

We want the Name and ID for the Epic so we specify those attributes in the [select](http://community.versionone.com/Developers/Developer-Library/Documentation/API/Queries/select). The VersionOne OID comes "free" with every query, so we don't need to specify it explicitly. But the ID that people see in the UI is `Number`. And if the ID we really want is a Clarity PPM Project ID, then we would need to store that in another attribute like `Reference`. (I recommend using custom field because other integrations may use and overwrite Reference for other purposes.) In the example, `Category` is added only as a way to confirm the where. If other attributes would be useful, the entire set of Epic attributes can be viewed using meta:

```
<Server Base URI>/meta.v1/Epic?xsl=api.xsl
```
